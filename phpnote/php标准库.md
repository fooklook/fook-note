## php标准库

### 查看所有标准库

```php
foreach(spl_classes() as $key=>$value)
{
    echo $key.' -&gt; '.$value.'<br />';
}
```

内置类有很多，但是并不常用。下面只介绍常用的。

### SimpleXMLIterator

这个类用来遍历xml文件。

```php
try{
    /**
     * $xmlstring为读取xml文件的内容
     */
    $it = new SimpleXMLIterator($xmlstring);
    //new RecursiveIteratorIterator($it,1)表示显示所有包括父元素在内的子元素。
    foreach(new RecursiveIteratorIterator($it,1) as $name => $data){
        echo $name.' -- '.$data.'<br />';
    }
}catch(Exception $e){
    echo $e->getMessage();
}
```

- SimpleXMLIterator::current — Returns the current element
- SimpleXMLIterator::getChildren — Returns the sub-elements of the current element
- SimpleXMLIterator::hasChildren — Checks whether the current element has sub elements
- SimpleXMLIterator::key — Return current key
- SimpleXMLIterator::next — Move to next element
- SimpleXMLIterator::rewind — Rewind to the first element
- SimpleXMLIterator::valid — Check whether the current element is valid


### DirectoryIterator

这个类用来查看一个目录中的所有文件和子目录：

```php
try{
    foreach ( new DirectoryIterator('./') as $Item )
    {
        echo $Item.'<br />';
    }
}catch(Exception $e){
    echo 'No files Found!<br />';
}
```

- DirectoryIterator::__construct — Constructs a new directory iterator from a path
- DirectoryIterator::current — Return the current DirectoryIterator item
- DirectoryIterator::getATime — Get last access time of the current DirectoryIterator item
- DirectoryIterator::getBasename — Get base name of current DirectoryIterator item
- DirectoryIterator::getCTime — Get inode change time of the current DirectoryIterator item
- DirectoryIterator::getExtension — Gets the file extension
- DirectoryIterator::getFilename — Return file name of current DirectoryIterator item
- DirectoryIterator::getGroup — Get group for the current DirectoryIterator item
- DirectoryIterator::getInode — Get inode for the current DirectoryIterator item
- DirectoryIterator::getMTime — Get last modification time of current DirectoryIterator item
- DirectoryIterator::getOwner — Get owner of current DirectoryIterator item
- DirectoryIterator::getPath — Get path of current Iterator item without filename
- DirectoryIterator::getPathname — Return path and file name of current DirectoryIterator item
- DirectoryIterator::getPerms — Get the permissions of current DirectoryIterator item
- DirectoryIterator::getSize — Get size of current DirectoryIterator item
- DirectoryIterator::getType — Determine the type of the current DirectoryIterator item
- DirectoryIterator::isDir — Determine if current DirectoryIterator item is a directory
- DirectoryIterator::isDot — Determine if current DirectoryIterator item is '.' or '..'
- DirectoryIterator::isExecutable — Determine if current DirectoryIterator item is executable
- DirectoryIterator::isFile — Determine if current DirectoryIterator item is a regular file
- DirectoryIterator::isLink — Determine if current DirectoryIterator item is a symbolic link
- DirectoryIterator::isReadable — Determine if current DirectoryIterator item can be read
- DirectoryIterator::isWritable — Determine if current DirectoryIterator item can be written to
- DirectoryIterator::key — Return the key for the current DirectoryIterator item
- DirectoryIterator::next — Move forward to next DirectoryIterator item
- DirectoryIterator::rewind — Rewind the DirectoryIterator back to the start
- DirectoryIterator::seek — Seek to a DirectoryIterator item
- DirectoryIterator::__toString — Get file name as a string
- DirectoryIterator::valid — Check whether current DirectoryIterator position is a valid file

### SplFileInfo

获取文件基本信息

- SplFileInfo::__construct — Construct a new SplFileInfo object
- SplFileInfo::getATime — Gets last access time of the file
- SplFileInfo::getBasename — Gets the base name of the file
- SplFileInfo::getCTime — Gets the inode change time
- SplFileInfo::getExtension — Gets the file extension
- SplFileInfo::getFileInfo — Gets an SplFileInfo object for the file
- SplFileInfo::getFilename — Gets the filename
- SplFileInfo::getGroup — Gets the file group
- SplFileInfo::getInode — Gets the inode for the file
- SplFileInfo::getLinkTarget — Gets the target of a link
- SplFileInfo::getMTime — Gets the last modified time
- SplFileInfo::getOwner — Gets the owner of the file
- SplFileInfo::getPath — Gets the path without filename
- SplFileInfo::getPathInfo — Gets an SplFileInfo object for the path
- SplFileInfo::getPathname — Gets the path to the file
- SplFileInfo::getPerms — Gets file permissions
- SplFileInfo::getRealPath — Gets absolute path to file
- SplFileInfo::getSize — Gets file size
- SplFileInfo::getType — Gets file type
- SplFileInfo::isDir — Tells if the file is a directory
- SplFileInfo::isExecutable — Tells if the file is executable
- SplFileInfo::isFile — Tells if the object references a regular file
- SplFileInfo::isLink — Tells if the file is a link
- SplFileInfo::isReadable — Tells if file is readable
- SplFileInfo::isWritable — Tells if the entry is writable
- SplFileInfo::openFile — Gets an SplFileObject object for the file
- SplFileInfo::setFileClass — Sets the class used with SplFileInfo::openFile
- SplFileInfo::setInfoClass — Sets the class used with SplFileInfo::getFileInfo and SplFileInfo::getPathInfo
- SplFileInfo::__toString — Returns the path to the file as a string

### SplFileObject

对文件进行遍历，效率更高，并且可以逐行读取。

- SplFileObject::__construct — Construct a new file object
- SplFileObject::current — Retrieve current line of file
- SplFileObject::eof — Reached end of file
- SplFileObject::fflush — Flushes the output to the file
- SplFileObject::fgetc — Gets character from file
- SplFileObject::fgetcsv — Gets line from file and parse as CSV fields
- SplFileObject::fgets — Gets line from file
- SplFileObject::fgetss — Gets line from file and strip HTML tags
- SplFileObject::flock — Portable file locking
- SplFileObject::fpassthru — Output all remaining data on a file pointer
- SplFileObject::fputcsv — Write a field array as a CSV line
- SplFileObject::fread — Read from file
- SplFileObject::fscanf — Parses input from file according to a format
- SplFileObject::fseek — Seek to a position
- SplFileObject::fstat — Gets information about the file
- SplFileObject::ftell — Return current file position
- SplFileObject::ftruncate — Truncates the file to a given length
- SplFileObject::fwrite — Write to file
- SplFileObject::getChildren — No purpose
- SplFileObject::getCsvControl — Get the delimiter, enclosure and escape character for CSV
- SplFileObject::getCurrentLine — Alias of SplFileObject::fgets
- SplFileObject::getFlags — Gets flags for the SplFileObject
- SplFileObject::getMaxLineLen — Get maximum line length
- SplFileObject::hasChildren — SplFileObject does not have children
- SplFileObject::key — Get line number
- SplFileObject::next — Read next line
- SplFileObject::rewind — Rewind the file to the first line
- SplFileObject::seek — Seek to specified line
- SplFileObject::setCsvControl — Set the delimiter, enclosure and escape character for CSV
- SplFileObject::setFlags — Sets flags for the SplFileObject
- SplFileObject::setMaxLineLen — Set maximum line length
- SplFileObject::__toString — Alias of SplFileObject::current
- SplFileObject::valid — Not at EOF