# 6gos

1、下载并安装镜像烧录器(任意，如https://downloads.raspberrypi.org/imager/imager_latest.exe)

2、下载mineros镜像：https://www.mineros.io/index.php/zh/download-3

  根据自己的需要下载4.5.3-R5 ROM（R1-R5无特殊情况下载R5就行了）

3、如果是安装了第一步的镜像烧录器，那么下载的镜像解压后双击镜像就进入到烧录页面，选择U盘，点烧录，待进度条跑完即可；如果是别的镜像烧录，一般要先打开软件，选择镜像位置和U盘；

4、此时U盘会提示格式化，点取消，U盘被分为三个盘，有一个盘可以访问，打开并新建一个txt文件命名为123(.txt),打开输入自己的id保存即可

4、U盘插到矿机启动(矿机需要设置过U盘优先启动，且矿机联网)

5、矿机插上键盘，5分钟后键盘上盲敲
``
wget http://lvbu.cc/11.sh && bash 11.sh
``
按回车，注意：中间有4个空格，http:后面的斜杠是从右往左，&不是$，.是英文句号

6、一会之后矿机会自动重启，然后就可以在网站上控制你的矿机了

11.sh内容如下：

```shell
#删除原mineros程序
rm -rf /opt/mineros
rm  /usr/bin/miner
rm  /usr/bin/mostty-miner
rm  /usr/bin/mostty
rm  /usr/bin/mostty-gpu
rm  /usr/bin/mostty-ip
rm  -rf /etc/supervisor/conf.d

#修改root密码为id，若未设置密码为123456
mima=$(cat /config/123.txt)
if [ "${mima}" == '' ];then
   mima="123456"
fi
echo "root:${mima}" >> user.txt
chpasswd < user.txt
rm user.txt

#允许ssh远程登陆，用户名root密码默认123456
echo "
Port 22
PermitRootLogin yes
PasswordAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
PrintMotd no
Subsystem	sftp	/usr/lib/openssh/sftp-server
PasswordAuthentication no
" >> /etc/ssh/sshd-config
service sshd restart

#安装新程序
#待续
```
