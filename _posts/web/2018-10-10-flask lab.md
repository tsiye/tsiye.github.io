##12.23
###shadowsocks
科学上网突然不行了，开始找原因，原来是昨天练python的时候把python2.7卸载了，现在下回来就没什么问题
###flask_server
1. `virtualenv -p /usr/bin/python3 first_flask_server` 创建python3 virtualenv  
> python3 虚拟环境貌似已经成为内置模块，使用`python3 -m venv venv`创建既可
可能需要先安装dependence `sudo apt-get install python3-venv`
2. `cd first_flask_server/bin/activate` 激活
3. (first_flask_server) ➜  python `python -V`
Python 3.5.2`版本3.5.2
4. `pip freeze > requirements.txt` 产生requirement.txt 
5. `pip list`查看dependence
6. `pip install Flask-PyMongo` 安装flask-pymongo

###nginx
`nginx`开启
`nginx -s quit` 关闭
开启遇到的问题nginx: [emerg] bind() to [::]:80 failed (98: Address already in use) 80端口被重复占用
[解决nginx](http://www.hankcs.com/appos/linux/fix-nginx-bind-err.html)
`sudo netstat -nptl`
清干净apache
###mongodb
`mongod`是启动mongodb服务器在27017端口
`mongo`是连接mongodb服务器
之前已经设好mongod开机自启 所以会出现
2018-12-23T12:38:59.116+0800 E STORAGE  [initandlisten] Failed to set up listener: SocketException: Address already in use

###localhost、127.0.0.1 和 本机IP之间的区别：
1. localhost等于127.0.0.1，不过localhost是域名，127.0.0.1是IP地址。
2. localhost和127.0.0.1不需要联网，都是本机访问。
3. 本机IP需要联网，本机IP是本机或外部访问， 本机 IP 就是本机对外放开访问的IP地址，这个网址就是与物理网卡绑定的IP地址。
 当操作系统初始化本机的TCP/IP协议栈时，设置协议栈本身的IP地址为127.0.0.1（保留地址），并注入路由表。
当IP层接收到目的地址为127.0.0.1（准确的说是：网络号为127的IP）的数据包时，不调用网卡驱动进行二次封装，而是立即转发到本机IP层进行处理，由于不涉及底层操作。
因此ping 127.0.0.1一般作为测试本机TCP/IP协议栈正常与否的判断之一。
本机IP，我们可以理解为本机有三块网卡，一块网卡叫做loopback（虚拟网卡），一块叫做ethernet（有线网卡），一块叫做wlan（你的无线网卡）。
###apt-get vs wget
[Linux下载：wget、yum、rpm、dpkg、apt-get](https://www.jianshu.com/p/41de6d045de1)
**apt-get** 是Debian Linux发行版中的APT软件包管理工具。所有基于Debian的发行都使用这个包管理系统。deb包可以把一个应用的文件包在一起，大体就如同Windows上的安装文件。 
**wget** wget命令用来从指定的URL下载文件。特点：在带宽很窄的情况下和不稳定网络中有很强的适应性，如果是由于网络的原因下载失败，wget会不断的尝试。
###kill
sudo kill -9 进程号
###查找文件
1. whereis + name
查找二进制文件，说明文件，源代码文件
2. find / -name + name
遍历查找 消耗较大
3. locate + name
linux会把系统内所有的文件都记录在一个数据库文件中，使用locate+文件名的方法会在linux系统维护的这个数据库中去查找目标，相比find命令去遍历磁盘查找的方式，效率会高很多，比较推荐使用这种方法。
###权限
1 x 执行 2 write 写 4 read 读
###链接
sudo ln -s /var/www/demoapp/demoapp_nginx.conf /etc/nginx/conf.d/
软链接：
1.软链接，以路径的形式存在。类似于Windows操作系统中的快捷方式
2.软链接可以 跨文件系统 ，硬链接不可以
3.软链接可以对一个不存在的文件名进行链接
4.软链接可以对目录进行链接

## nginx 常用指令
`ps -ef|grep nginx` 查看nginx指令
`sudo nginx`或者`sudo /usr/sbin/nginx` 启动nginx
`sudo nginx -s stop` 停止nginx
`sudo nginx -s reload` 平滑启动 (在不停止 nginx 的情况下，重启 nginx，重新加载配置文件，用新的工作进程代替旧的工作进程。)