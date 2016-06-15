##PHP守护进程

###什么是Daemon进程

这又是一个有趣的概念，daemon在英语中是"精灵"的意思，就像我们经常在迪斯尼动画里见到的那些，有些会飞，有些不会，经常围着动画片的主人公转来转去，啰里啰唆地提一些忠告，时不时倒霉地撞在柱子上，有时候还会想出一些小小的花招，把主人公从敌人手中救出来，正因如此，daemon有时也被译作"守护神"。所以，daemon进程在国内也有两种译法，有些人译作"精灵进程"，有些人译作"守护进程"，这两种称呼的出现频率都很高。

与真正的daemon相似，daemon进程也习惯于把自己隐藏在人们的视线之外，默默为系统做出贡献，有时人们也把它们称作"后台服务进程"。daemon进程的寿命很长，一般来说，从它们一被执行开始，直到整个系统关闭，它们才会退出。几乎所有的服务器程序，包括我们熟知的Apache和wu-FTP，都用daemon进程的形式实现。很多Linux下常见的命令如inetd和ftpd，末尾的字母d就是指daemon。

为什么一定要使用daemon进程呢？Linux中每一个系统与用户进行交流的界面称为终端（terminal），每一个从此终端开始运行的进程都会依附于这个终端，这个终端就称为这些进程的控制终端（Controlling terminal），当控制终端被关闭时，相应的进程都会被自动关闭。关于这点，读者可以用X-Window中的XTerm试验一下，（每一个XTerm就是一个打开的终端，）我们可以通过键入命令启动应用程序，比如：$netscape 然后我们关闭XTerm窗口，刚刚启动的netscape窗口也会随之一同突然蒸发。但是daemon进程却能够突破这种限制，即使对应的终端关闭，它也能在系统中长久地存在下去，如果我们想让某个进程长命百岁，不因为用户或终端或其他的变化而受到影响，就必须把这个进程变成一个daemon进程。

###Daemon进程的编程规则

如果想把自己的进程变成daemon进程，我们必须严格按照以下步骤进行：

1. 调用fork产生一个子进程，同时父进程退出。我们所有后续工作都在子进程中完成。这样做我们可以：

     1.1 如果我们是从命令行执行的该程序，这可以造成程序执行完毕的假象，shell会回去等待下一条命令；
     1.2 刚刚通过fork产生的新进程一定不会是一个进程组的组长，这为第2步的执行提供了前提保障。

     这样做还会出现一种很有趣的现象：由于父进程已经先于子进程退出，会造成子进程没有父进程，变成一个孤儿进程（orphan）。每当系统发现一个孤儿进程，就会自动由1号进程收养它，这样，原先的子进程就会变成1号进程的子进程。

2. 调用setsid系统调用。这是整个过程中最重要的一步。setsid的介绍见附录2，它的作用是创建一个新的会话（session），并自任该会话的组长（session leader）。如果调用进程是一个进程组的组长，调用就会失败，但这已经在第1步得到了保证。调用setsid有3个作用：

     2.1 让进程摆脱原会话的控制；
     2.2 让进程摆脱原进程组的控制；
     2.3 让进程摆脱原控制终端的控制；

     总之，就是让调用进程完全独立出来，脱离所有其他进程的控制。

3. 把当前工作目录切换到根目录。

     如果我们是在一个临时加载的文件系统上执行这个进程的，比如：/mnt/floppy/，该进程的当前工作目录就会是/mnt/floppy/。在整个进程运行期间该文件系统都无法被卸下（umount），而无论我们是否在使用这个文件系统，这会给我们带来很多不便。解决的方法是使用chdir系统调用把当前工作目录变为根目录，应该不会有人想把根目录卸下吧。

     关于chdir的用法，参见附录1。

     当然，在这一步里，如果有特殊的需要，我们也可以把当前工作目录换成其他的路径，比如/tmp。

4. 将文件权限掩码设为0。

     这需要调用系统调用umask，参见附录3。每个进程都会从父进程那里继承一个文件权限掩码，当创建新文件时，这个掩码被用于设定文件的默认访问权限，屏蔽掉某些权限，如一般用户的写权限。当另一个进程用exec调用我们编写的daemon程序时，由于我们不知道那个进程的文件权限掩码是什么，这样在我们创建新文件时，就会带来一些麻烦。所以，我们应该重新设置文件权限掩码，我们可以设成任何我们想要的值，但一般情况下，大家都把它设为0，这样，它就不会屏蔽用户的任何操作。

     如果你的应用程序根本就不涉及创建新文件或是文件访问权限的设定，你也完全可以把文件权限掩码一脚踢开，跳过这一步。

5. 关闭所有不需要的文件。

     同文件权限掩码一样，我们的新进程会从父进程那里继承一些已经打开了的文件。这些被打开的文件可能永远不被我们的daemon进程读或写，但它们一样消耗系统资源，而且可能导致所在的文件系统无法卸下。需要指出的是，文件描述符为0、1和2的三个文件（文件描述符的概念将在下一章介绍），也就是我们常说的输入、输出和报错这三个文件也需要被关闭。很可能不少读者会对此感到奇怪，难道我们不需要输入输出吗？但事实是，在上面的第2步后，我们的daemon进程已经与所属的控制终端失去了联系，我们从终端输入的字符不可能达到daemon进程，daemon进程用常规的方法（如printf）输出的字符也不可能在我们的终端上显示出来。所以这三个文件已经失去了存在的价值，也应该被关闭。 

###使用PHP编写Gearman的Worker守护进程

在我之前的文章中，介绍过Gearman的使用。在我的项目中，我使用了PHP来编写一直运行的Worker。如果按照Gearman官方推荐的例子，只是简单的一个循环来等待任务，会有一些问题，包括：1、当代码进行过修改之后，如何让代码的修改生效；2、重启Worker的时候，如何保证当前的任务处理完成才重启。

针对这个问题，我考虑了以下的解决方法：

1. 每次修改完代码后，Worker需要手工重启（先杀死然后启动）。这个只能解决重新加载配置文件的问题。
2. 在Worker中设置，单次任务循环完成后，就对Worker进行重启。这个方案的问题在于消耗比较大。
3. 在Worker中添加一个退出函数，如果需要Worker退出的时候，在Client端发送一个优先级比较高的退出调用。这个需要客户端配合，在使用后台类任务时，不太适合。
4. 在Worker中检查文件是否发生变化，如果发生了变化，退出并重启自身。
5. 为Worker编写信号控制，接受重启指令，类似于 http restart graceful 指令。

最后，结合4和5两种方法，可以实现这样一个Daemon，如果配置文件发生了变化，他就会自动重启；如果接受到了用户的 kill  -1 pid 信号，也会重新启动。

代码如下：

```php
<?php

declare( ticks = 1 );
// This case will check the config file regularly, if the config file changed, it will restart it self
// If you want to restart the daemon gracefully, give it a HUP signal
// by shiqiang<cocowool@gmail.com> at 2011-12-04

$init_md5 = md5_file( 'config.php');

// register signal handler
pcntl_signal( SIGALRM, "signal_handler", true );
pcntl_signal( SIGHUP, 'signal_handler', TRUE );

$job_flag = FALSE;    //Job status flag, to justify if the job has been finished
$signal_flag = FALSE;    //Signal status flag, to justify whether we received the kill -1 signal

while( 1 ){
    $job_flag = FALSE;    //Job status flag
    print "Worker start running ... \n";
    sleep(5);
    print "Worker's task done ... \n";
    $flag = TRUE;    //Job status flag
    AutoStart( $signal_flag );
}

function signal_handler( $signal ) {
    global $job_flag;
    global $signal_flag;

    switch( $signal ){
        case SIGQUIT:
            print date('y-m-d H:i:s', time() ) . " Caught Signal : SIGQUIT - No : $signal \n";
            exit(0);
            break;
        case SIGSTOP:
            print date('y-m-d H:i:s', time() ) . " Caught Signal : SIGSTOP - No : $signal \n";
            break;
        case SIGHUP:
            print date('y-m-d H:i:s', time() ) . " Caught Signal : SIGHUP - No : $signal \n";
            if( $flag === TRUE ){
                AutoStart( TRUE );
            }else{
                $signal_flag = TRUE;
            }
            break;
        case SIGALRM:
            print date('y-m-d H:i:s', time() ) . " Caught Signal : SIGALRM - No : $signal \n";
            //pcntl_exec( '/bin/ls' );
            pcntl_alarm( 5 );
            break;
        default:
            break;
    }
}

function AutoStart( $signal = FALSE, $filename = 'config.php' ){
    global $init_md5;

    if( $signal || md5_file( $filename ) != $init_md5 ){
        print "The config file has been changed, we are going to restart. \n";
        $pid = pcntl_fork();
        if( $pid == -1 ){
            print "Fork error \n";
        }else if( $pid > 0 ){
            print "Parent exit \n";
            exit(0);
        }else{
            $init_md5 = md5_file( $filename );
            print "Child continue to run \n";
        }
    }
}
```