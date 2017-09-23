#打开Mac OSX原生的NTFS功能

Mac OSX由于版权等原因没有在默认情况支持这种格式，不过确实可以做到不需要装软件，达到原生支持NTFS的读取，只是每次需要在终端执行命令而已

1、打开终端，输入命令

    diskutil list
2、输入命令

    sudo vim /etc/fstab

3、编辑内容如下

    LABEL=Win\040Ntfs\040Drive none ntfs rw,auto,nobrowse

`Win\040Ntfs\040Drive` 这串字符中`\040`代表空格，`Win\040Ntfs\040Drive` 这一串出现在`diskutil list`那个屏幕里面，比如下图就是HD-E1
![](/assets/20161228101320061.png)

4、最后一步

    sudo ln -s /Volumes ~/Desktop/Volumes

