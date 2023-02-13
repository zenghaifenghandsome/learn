# 大数据技术之Linux
## Linux 文件
> Linux 中一切皆文件
## Linux 主要目录结构
- / ：根目录
- /bin：时Binary的缩写，这个目录存放着经常使用的可执行文件
- /etc：整个系统的配置文件都在这个里面
- /opt：给主机安装额外软件的目录。
- /tmp：存放临时文件，
- /usr：存放用户文件的地方
- /sbin:存放系统管理员使用的系统管理程序
- /home：存放系统普通用户文件的主要位置
- /root:系统管理员用户的主目录
- /lib:系统开机所需要的最基本的动态连接共享库，几乎所有的应用程序都需要用到这些共享库
- /boot：启动Linux时使用的一些核心文件
- /dev：设备管理器，Linux所有的硬件都用文件的形式保存在这里
- /media：Linux自动识别的一些设备，挂载在这里
- /mnt：挂载别的文件系统的目录
  
## vi/vim
### 一般模式
> 以vi打开一个文件，就直接进入一般模式，在这个模式中，可以上下左右按键来移动光标，可以做一些删除、粘贴，复制等操作，但不可以输入
#### 常用语法
|语法|功能描述|
|-|-|
|yy|复制光标所在的一整行内容|
|y数字y|复制从光标所在后的n行内容|
|p|粘贴|
|u|撤销|
|dd|删除光标所在的一行内容|
|d数字d|删除从光标所在后的n行内容|
|x|剪切一个字母|
|X|剪切前面一个字母|
|yw|复制一个词|
|dw|删除一个词|
|shift6|移动到行头|
|shift4|移动到行尾|
|1 shift g|移动到页头|
|shift g|移动到页尾|
|数字n shift g|移动到目标行|

### 编辑模式
> 按下[i,I,o,O,a,A]中的任意一个字母将会进入编辑模式，在画面左下方出现 INSERT的字样，就代表进入了编辑模式，才可以编辑文件的内容，
> 按下ESC键，退出编辑模式，进入一般模式
#### 常用语法
|语法|功能描述|
|-|-|
|i|当前光标行|
|I|光标所在行的最前面|
|o|当前光标所在的下一行|
|O|当前光标所在的上一行|
|a|当前光标后|
|A|光标所在行的最后面|

### 指令模式
> 在一般模式下输入[：？ /]3个中的任意一个，进入指令模式
#### 常用语法
|语法|功能描述|
|-|-|
|：w|保存|
|：q|退出|
|：！|强制执行|
|/要查找的词|n找下一个，N找上一个|
|：noh|取消高亮显示|
|：set nu|显示行号|
|：set nonu|关闭行号|
|：%/old/new/g|替换内容 /g global 替换匹配到的所有内容|
|：wq！|强制保持退出|

## 配置网络
> ifconfig 显示所有网络接口的详细信息
> ping ip/网址 测试当前主机和目标间的网络是否连通
> vim /etc/sysconfig/network-scripts/ifcfg-ens33 修改IP配置文件
> systemctl restart network 重启网络
> systemctl stop NetworkManager 停止NetworkManager服务
> systemctl disable NetworkManager 禁用NetworkManager服务
## 配置主机名
- hostname 查看当前主机的主机名
- vim /etc/hostname 修改主机名
- vim /etc/hosts 修改hosts映射文件
## systemctl
- systemctl start|restart|stop|status 服务名
- systemctl list-unit-files 查看服务开机启动状态
- 