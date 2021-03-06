如何在RHEL/CentOS 7.0中使用tmpfs
================================================================================
 7中的tmpfs，这是一个将所有文件和文件夹写到虚拟内存中而不是实际写到磁盘中的虚拟文件系统。这意味中tmpfs中所有的内容都是临时的，在取消挂载、系统重启或者电源切断后内容都将会丢失。技术的角度上来说，tmpfs将搜有的内容放在内核内部缓存中并且会增大或者缩小来容纳文件并可从交换空间中交换处不需要的页。

CentOS默认使用tmpfs做的事可用df -h命令的输出来看：

    # df –h

![df](http://blog.linoxide.com/wp-content/uploads/2015/05/tmpfs1.jpg)

/dev - 含有针对所有设备的设备文件的目录
/dev/shm – 包含共享内存分配
/run - 用于系统日志
/sys/fs/cgroup - 用于cgrpups， 一个针对特定进程限制、管制和审计资源利用的内核特性

/tmp目录， 你可以用下面的两种方法来做到：

### 使用systemctl来在/tmp中启用tmpfs ###

你可以使用systemctl命令在tmp目录启用tmpfs， 首先用下面的命令来检查这个特性是否可用：

    # systemctl is-enabled tmp.mount

这会显示当先的状态，你可以使用下面的命令来启用它：

    # systemctl enable tmp.mount

![systemctl](http://blog.linoxide.com/wp-content/uploads/2015/05/tmpfs3.jpg)

这会控制/tmp目录并挂载tmpfs。

### 手动挂载/tmp/文件系统 ###

你可以在/etc/fstab中添加下面这行在/tmp挂载tmpfs。

    tmpfs /tmp tmpfs size=512m 0 0

接着运行这条命令

    # mount –a

![df](http://blog.linoxide.com/wp-content/uploads/2015/05/tmpfs2.jpg)

这应该就会在df -h中显示tmpfs了，同样也会在你下次重启是会自动挂载。

### 立即创建tmpfs ###

如果由于一些原因，你写昂立即创建tmpfs，你可以使用下面的命令：

    # mount -t tmpfs -o size=1G tmpfs /mnt/mytmpfs

当然你可以在size选项中指定你希望的大小和希望的挂载点，只要记住是有效的目录就行了。

--------------------------------------------------------------------------------

via: http://linoxide.com/file-system/use-tmpfs-rhel-centos-7-0/

作者：[Adrian Dinu][a]
译者：[geekpi](https://github.com/geekpi)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创翻译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:http://linoxide.com/author/adriand/
