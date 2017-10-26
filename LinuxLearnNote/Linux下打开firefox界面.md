#Linux下打开firefox界面

安装火狐浏览器 
 
    yum install firefox  

修改环境变量

    vi .bashrc
    #在末尾添加：
    DISPLAY=localhost:10.0
    export DISPLAY

加载环境变量

    source .bashrc

安装相关依赖：
  
    yum install xorg-x11-xauth xorg-x11-fonts-* xorg-x11-utils
    yum install xdm
    
修改相关文件
    
    vi /etc/X11/xdm/Xaccess  

修改内容为，去掉下面这一行最前面的#号

    # *           #any  host  can  get  a  login  window 

安装相关依赖：

    yum install gdm
    yum install xorg-x11-server-Xvfb
    yum install xorg-x11-drivers

修改配置文件

    vi /etc/gdm/custom.conf

在[security]和[xdmcp]下添加一下内容
    
    [daemon]
    
    [security]
    AllowRemoteRoot=true
    DisallowTCP=false
    
    [xdmcp]
    Enable=true
    Port=177
    
    [greeter]
    
    [chooser]
    
    [debug]

 
修改custom.conf后，必须重启gdm才可以生效。具体做法就是kill进程中的gdm，然后重启gdm:

查看gdm进程

    ps -ef | grep gdm
    ＃root     10179  3513  0 11:04 pts/8    00:00:00 grep gdm

杀死gdm进程

    kill -9 10179

重启gdm

    /usr/sbin/gdm -restart