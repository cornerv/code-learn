# 第一章

~~~perl
#! /usr/bin/perl
第一行是特殊的注释。在 Unix 系统中◆，如果文本的第一行前两个字符是“#!”，接着的就是执行下面文件的程序。在本例中，这个程序是/usr/bin/perl。

#！行和程序的可移植性相关，需要找到每台机器的存放地点。幸运的是，通常都被放在/usr/bin/perl 或/usr/local/bin/perl 中。

如果不是这样，则需要找到你自己机器上 perl 的存放地点，然后使用那个路径。在 Unix 系统中，可能使用如下一行找到:
which perl
/usr/bin/perl

#一个简单示例：
#! /usr/bin/perl
print "Hello,word!\n"; #和python不同的是，需要自己加\n，且命令行结尾要写分号；
hhh
