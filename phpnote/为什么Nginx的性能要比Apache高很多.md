##为什么Nginx的性能要比Apache高很多

为什么Nginx的性能要比Apache高很多？ 这得益于Nginx使用了最新的epoll（Linux 2.6内核）和kqueue（freebsd）网络I/O模型，而Apache则使用的是传统的select模型。

目前Linux下能够承受高并发访问的Squid、Memcached都采用的是epoll网络I/O模型。

处理大量的连接的读写，Apache所采用的select网络I/O模型非常低效。

下面用一个比喻来解析Apache采用的select模型和Nginx采用的epoll模型进行之间的区别：

假设你在大学读书，住的宿舍楼有很多间房间，你的朋友要来找你。

select版宿管大妈就会带着你的朋友挨个房间去找，直到找到你为止。

而epoll版宿管大妈会先记下每位同学的房间号，

你的朋友来时，只需告诉你的朋友你住在哪个房间即可，不用亲自带着你的朋友满大楼找人。

如果来了10000个人，都要找自己住这栋楼的同学时，select版和epoll版宿管大妈，谁的效率更高，不言自明。

同理，在高并发服务器中，轮询I/O是最耗时间的操作之一，select和epoll的性能谁的性能更高，同样十分明了。

在linux的网络编程中，很长的时间都在使用select来做事件触发。

在linux新的内核中，有了一种替换它的机制，就是epoll。

相比于select，epoll最大的好处在于它不会随着监听fd数目的增长而降低效率。

因为在内核中的select实现中，它是采用轮询来处理的，轮询的fd数目越多，自然耗时越多。

并且，在linux/posix_types.h头文件有这样的声明：`#define __FD_SETSIZE    1024`

表示select最多同时监听1024个fd，当然，可以通过修改头文件再重编译内核来扩大这个数目，但这似乎并不治本。

###epoll的接口非常简单，一共就三个函数：

####int epoll_create(int size);

创建一个epoll的句柄，size用来告诉内核这个监听的数目一共有多大。

这个参数不同于select()中的第一个参数，给出最大监听的fd+1的值。

需要注意的是，当创建好epoll句柄后，它就是会占用一个fd值，在linux下如果查看/proc/进程id/fd/，

是能够看到这个fd的，所以在使用完epoll后，必须调用close()关闭，否则可能导致fd被耗尽。

####int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);

epoll的事件注册函数，它不同与select()是在监听事件时告诉内核要监听什么类型的事件，

而是在这里先注册要监听的事件类型。第一个参数是epoll_create()的返回值，

第二个参数表示动作，用三个宏来表示：

EPOLL_CTL_ADD：注册新的fd到epfd中；

EPOLL_CTL_MOD：修改已经注册的fd的监听事件；

EPOLL_CTL_DEL：从epfd中删除一个fd；

第三个参数是需要监听的fd，第四个参数是告诉内核需要监听什么事，struct epoll_event结构如下：

```shell
typedef union epoll_data {
void *ptr;
int fd;
__uint32_t u32;
__uint64_t u64;
} epoll_data_t;
struct epoll_event {
__uint32_t events; /* Epoll events */
epoll_data_t data; /* User data variable */
};
```

events可以是以下几个宏的集合：

EPOLLIN ：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）；

EPOLLOUT：表示对应的文件描述符可以写；

EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）；

EPOLLERR：表示对应的文件描述符发生错误；

EPOLLHUP：表示对应的文件描述符被挂断；

EPOLLET： 将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的。

EPOLLONESHOT：只监听一次事件，当监听完这次事件之后，

如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里

####int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);

等待事件的产生，类似于select()调用。

参数events用来从内核得到事件的集合，maxevents告之内核这个events有多大，这个 maxevents的值不能大于创建epoll_create()时的size，参数timeout是超时时间（毫秒，0会立即返回，-1将不确定，也有说法说是永久阻塞）。

该函数返回需要处理的事件数目，如返回0表示已超时。

####关于ET、LT两种工作模式：

可以得出这样的结论：

ET模式仅当状态发生变化的时候才获得通知,这里所谓的状态的变化并不包括缓冲区中还有未处理的数据，也就是说,如果要采用ET模式,需要一直read/write直到出错为止，很多人反映为什么采用ET模式只接收了一部分数据就再也得不到通知了，大多因为这样；而LT模式是只要有数据没有处理就会一直通知下去的。

那么究竟如何来使用epoll呢？其实非常简单。

通过在包含一个头文件#include <sys/epoll.h> 以及几个简单的API将可以大大的提高你的网络服务器的支持人数。

首先通过create_epoll(int maxfds)来创建一个epoll的句柄，其中maxfds为你epoll所支持的最大句柄数。

这个函数会返回一个新的epoll句柄，之后的所有操作将通过这个句柄来进行操作。

在用完之后，记得用close()来关闭这个创建出来的epoll句柄。

之后在你的网络主循环里面，每一帧的调用epoll_wait(int epfd, epoll_event events, int max events, int timeout)来查询所有的网络接口，看哪一个可以读，哪一个可以写了。基本的语法为：

nfds = epoll_wait(kdpfd, events, maxevents, -1);

其中kdpfd为用epoll_create创建之后的句柄，events是一个epoll_event*的指针，当epoll_wait这个函数操作成功之后，epoll_events里面将储存所有的读写事件。

max_events是当前需要监听的所有socket句柄数。最后一个timeout是 epoll_wait的超时，为0的时候表示马上返回，为-1的时候表示一直等下去，直到有事件范围，为任意正整数的时候表示等这么长的时间，如果一直没有事件，则范围。一般如果网络主循环是单独的线程的话，可以用-1来等，这样可以保证一些效率，如果是和主逻辑在同一个线程的话，则可以用0来保证主循环的效率。

epoll_wait范围之后应该是一个循环，遍利所有的事件。

几乎所有的epoll程序都使用下面的框架：

```shell
for( ; ; )
    {
        nfds = epoll_wait(epfd,events,20,500);
        for(i=0;i<nfds;++i)
        {
            if(events[i].data.fd==listenfd) //有新的连接
            {
                connfd = accept(listenfd,(sockaddr *)&clientaddr, &clilen); //accept这个连接
                ev.data.fd=connfd;
                ev.events=EPOLLIN|EPOLLET;
                epoll_ctl(epfd,EPOLL_CTL_ADD,connfd,&ev); //将新的fd添加到epoll的监听队列中
            }
            else if( events[i].events&EPOLLIN ) //接收到数据，读socket
            {
                n = read(sockfd, line, MAXLINE)) < 0    //读
                ev.data.ptr = md;     //md为自定义类型，添加数据
                ev.events=EPOLLOUT|EPOLLET;
                epoll_ctl(epfd,EPOLL_CTL_MOD,sockfd,&ev);//修改标识符，等待下一个循环时发送数据，异步处理的精髓
            }
            else if(events[i].events&EPOLLOUT) //有数据待发送，写socket
            {
                struct myepoll_data* md = (myepoll_data*)events[i].data.ptr;    //取数据
                sockfd = md->fd;
                send( sockfd, md->ptr, strlen((char*)md->ptr), 0 );        //发送数据
                ev.data.fd=sockfd;
                ev.events=EPOLLIN|EPOLLET;
                epoll_ctl(epfd,EPOLL_CTL_MOD,sockfd,&ev); //修改标识符，等待下一个循环时接收数据
            }
            else
            {
                //其他的处理
            }
        }
    }
```