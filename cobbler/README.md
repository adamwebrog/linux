* 安装cobbler  
on centos6/rhel6,  
```
rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum -y install cobbler fence-agents pykickstart dnsmasq
chkconfig cobblerd on
chkconfig httpd on
chkconfig dnsmasq on
service cobblerd start
service httpd start
```  
其中配置文件在/etc/cobbler/,其余文件在/var/{lib|www}/cobbler

* 修改配置文件  
```
vi /etc/cobbler/setting
修改内容如下
    manage_dhcp：1
    manage_dns：1
    manage_tftpd：1
    restart_dhcp：1
    restart_dns：1
    pxe_just_once：1
    next_server：<服务器的 IP 地址>
    server：<服务器的 IP 地址
    default_password_crypted: "use openssl passwd -1"
```  

* 采用dnsmasq提供dns和dhcp服务  
```
vi /etc/cobbler/modules.conf
修改内容如下
[dns]
module = manage_dnsmasq
[dhcp]
module = manage_dnsmasq
[tftpd]
module = manage_in_tftpd

vi /etc/cobbler/dnsmasq.template
修改内容如下
dhcp-range=起始IP,终止IP
```  

* 更新cobbler  
```
service cobblerd restart
cobbler check
cobbler sync
```  

* 安装Centos6.5  
导入安装文件  
```
mount -t iso9660 -o loop,ro ~/CentOS-6.5-x86_64-bin-DVD1.iso /mnt/
cobbler import --name=centos6.5 --arch=x86_64 --path=/mnt/
cobbler distro report
cobbler profile report
```  

* 创建配置文件  
