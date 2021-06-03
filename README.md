# VPN



2018.11.15：更换系统为 CentOS 7 x64，建议使用 BBR 进行优化加速。

1. 内容概览：
2. 注册 & 安装 VPS
3. 连接 VPS
4. vultr搭建shadowsocks
5. 使用方法
6. 效果测试
7. 常见问题
   
   

##### 一、注册 & 安装

##### 注册

打开[Vultr](http://www.vultr.com 'Vultr') 官网：[Vultr](http://www.vultr.com 'Vultr')



##### 1.选择节点

我选择的 “Tokyo” 日本节点，距离最近，效果不错。当然，网络环境不同，效果也有差别，大家可以自己测试下，方法可以在后文的 Tips 中查看。

2018.7.26 更新：日本节点太火了，现在效果不太好，建议更换为其它节点，如纽约、洛杉矶等。节点选择工具：HTTP Ping VPS 节点选择工具



##### 2.选择系统

选择 “CentOS 7 x64” 系统。



##### 3.选择套餐



一般自己用 $5/月 的每月有 1000G 流量也够用了，可以按个人需要选择。

注意：最低 $2.5/mo 的套餐现在只支持 IPv6 网络，非 IPv6 不要选这个套餐，请选择其他的！



##### 4.其他选项

有 IPv6，私密，域名、标签等，无特殊需要全部默认即可。

选好后点击右下角的 “Deploy Now” ，开始安装 VPS。

查看管理 VPS

已添加的 VPS 都会显示在 “Servers” 面板中，当显示 “Running” 时即表示安装完成。点击 “Manage” 进入 VPS 管理面板。

VPS 管理面板，这里需要记录下 IP 地址（IP Address）、用户名（Username）、密码（Password）供稍后连接时使用。

##### 二、连接

使用 ssh 工具连接我们的 VPS，Windows 推荐 PuTTY/XShell，Mac 可使用自带的终端。

xshell 网盘链接：pan.baidu.com/s/1hoH6qLZIx-cFw4WLtqMprw    密码：p6dp



Windows
安装 PuTTY/XShell，打开软件。

点击 “文件” —> “新建” —> “连接”，输入 “名称” 和 “主机” （即VPS ip）。
出现 root@vultr:~# 即连接成功。



Mac
打开终端，输入以下代码登录 VPS，其中 root 即用户名，将 ip 更改为 VPS 的 IP 地址，回车。

ssh root@ip
输入 yes 确认，粘贴密码，回车。需要提醒的是，密码输入时并不会显示出来，直接复制粘贴，回车即可。

出现 root@vultr:~# 即连接成功。



##### 三、 vultr搭建shadowsocks

连接上之后，就可以开始搭建了。

##### 1.安装锐速 / 谷歌 BBR 加速优化

说明：锐速和 BBR 都是加速优化的方法，两者效果差不多。锐速是收费项目，这里用的是 91yun 破解版；BBR 为谷歌免费开源项目，二选一即可。（选择困难症直接 BBR）

需要注意的是，锐速已停止更新，新内核不太兼容，尽量用 BBR 优化加速。

锐速

使用锐速加速优化，直接复制粘贴下面命令，回车运行。

```shell
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh
1
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh

```



逐行执行下面命令安装 BBR。

#### 谷歌 BBR

##### 下载脚本

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
```



##### 添加执行权限

```
chmod +x bbr.sh
```



##### 执行脚本安装

```
./bbr.sh
```



##### 下载脚本

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
```



##### 添加执行权限

```
chmod +x bbr.sh
```



##### 执行脚本安装

```shell
./bbr.sh

显示 “Press any key to start…” 按回车确认。
```

安装完后，按提示重启 VPS，输入 Y 回车重启。稍候 1min 等待重启完成，再重新连接 。

重启后输入 lsmod | grep bbr ，出现 tcp_bbr 即说明 BBR 已经启动。



##### 2.安装SS

依次运行下面三行命令，如下图所示按要求输入相应信息。建议：端口选择大于 1000 的。

```shell
wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

chmod +x shadowsocks.sh

./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

安装完成，把标红的连接信息记录下来，就可以关闭 了。

服务端搭建完成！(*^▽^*)

##### 3.多用户配置

自己用的话不建议配置这一步，可跳过该步，因为不熟悉操作的话，很容易在这一步出问题！

如果想和一两个亲友一起用的话，可以继续本步。先说注意事项：

输入法切 英文
核对正确再保存 不要漏输代码
配置好后 重启 shadowsocks 才会生效
首先，我们 把配置信息准备好，把下面的代码复制到记事本中（# 开头的是注释，不要复制进去），按要求把 "port_password"{……} 中的端口和密码改为自己需要的，其他默认。



##### 1.先设置编辑好端口和对应的密码



##### 2.添加或删除的用户都在 "port_password"{……} 中



##### 3.用户信息格式，注意末尾的英文逗号："端口"："密码",  如 "8006": "123456",



##### 4."method" 为加密方式，可修改，默认也可以，客户端的加密方式也是这个

```
{
    "server":"0.0.0.0",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
         "8989":"password0",
         "9001":"password1",
         "9002":"password2",
         "9003":"password3",
         "9004":"password4"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
```

然后，在 /etc 下新建 shadowsocks.json 配置文件：

##### 创建配置文件

vi /etc/shadowsocks.json

2
3

# 创建配置文件

vi /etc/shadowsocks.json
出现一列波浪线即进入 vim。

注意，敲黑板了！！！下面的操作很重要：

进入 vim 后，按 a ，然后把刚才准备好的 “配置信息” 粘贴进去，检查无误；

再按 Esc，输入 :wq 保存退出。

比如下面这个 "port_password"{……} 中的配置，每个端口对应一个用户，设置了 3 个用户，分别使用 8000, 8001, 8002 端口，冒号后面是对应的密码。注意核对最后一个的末尾是没有逗号的。

20184261529.png
重启 shadowsocks 生效：

/etc/init.d/shadowsocks restart
关闭防火墙：
systemctl stop firewalld.service
systemctl disable firewalld.service
多用户配置完成！(๑•̀ㅂ•́)و✧

4.其他命令
卸载
./shadowsocks.sh uninstall
控制
/etc/init.d/shadowsocks start      # 启动
/etc/init.d/shadowsocks stop       # 停止
/etc/init.d/shadowsocks restart    # 重启
/etc/init.d/shadowsocks status     # 状态
四、使用
下面只要下载客户端连接就行了。包含 win、mac 和 安卓客户端。（IOS 需要在商店中安装）
GitHub：
安卓：shadowsocks-android
Win：shadowsocks-windows
Mac：ShadowsocksX-NG-R
电脑
打开客户端，将上面记录的相应连接信息填入客户端，确定。
vultr（23）.png
右键任务栏托盘小飞机图标，“启动”，可以选择合适的代理模式。
PAC：只代理国外网站；
全局：所有网站都通过SS。
vultr（24）.png
手机等其他平台
和电脑类似，填入对应连接信息，启用即可。其他选项如果不懂的话，保持默认就好。
玩游戏 的话可以参考：SSTap：实现SS(R)真实全局代理

完成！现在就可以浏览 Google 啦 ╰(￣▽￣)╭

五、效果测试
简单粗暴，直接 youtube 4K 视频看效果。联通 100M 光纤，如下图。

vultr（20）.png
本地 Ping 测试。

vultr（21）.png
全国 Ping 测试。

vultr（22）.png
常见问题
最常见的两个问题：

xshell 连接不上
Ping 下 IP 看是否超时，或使用 ping.pe 检测，如果国内部分全部超时，则是该 IP 被墙，销毁该机器，再新建一个；
如果不超时，但是延迟特别大，则可能是线路不合适，可以更换其他地区的节点。
SS 搭好不能访问 Google
客户端未启动，需要右键托盘图标启动；
检查核对客户端连接信息，服务器 ip 地址、端口、密码、 加密方式 是否正确，尤其是加密方式，很多朋友会忘记更改；
更换浏览器，使用 Chrome 测试；
配置多用户时存在错误，返回该步核对配置信息。
Tips
我这里效果还不错，youtube 1080P 没问题，4k 视频要看时间段，毕竟大家使用高峰时线路拥挤会受到影响。我的测试数据只能提供参考，因为网络环境和位置不一样，效果肯定是存在差异的。大家可以自己测试下。

vultr 按时间计费还是很不错的，用一个小时收一个小时的费用，销毁机器即可停止计费（是 Destroy，不是 Stop），有空的话可以多开几台不同地区的试试，选择最好的用。

测试方法：

必备网络知识和测试方法介绍
HTTP Ping VPS 节点选择工具
要测试各国家地区的线路，可以使用 Vultr 提供的测试 IP 和文件进行测速
地址：downloadspeedtests
