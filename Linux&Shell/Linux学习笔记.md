## 命令行
1. Linux gpasswd 是 Linux 下工作组文件 /etc/group 和 /etc/gshadow 管理工具，用于将一个用户添加到组或者从组中删除
```shell
gpasswd -a $USER  groupName
```
2. less命令
```
less /var/log/xxx.log
# ---- 前后翻页的快捷键 ----
# 空格键：文件内容读取下一个终端屏幕的行数，相当于前进一个屏幕（页）。很常用的快捷键。与键盘上的 PageDown（下一页）效果一样；
# 回车键：文件内容读取下一行，也就是前进一行，与键盘上的向下键效果是一样的；
# d 键：前进半页（半个屏幕）；
# b 键：后退一页，与键盘上的 PageUp（上一页）效果一样；
# y 键：后退一行，与键盘上的向上键效果是一样的；
# u 键：后退半页（半个屏幕）；
# q 键：停止读取文件，中止 less 命令。

# ---- 搜索相关快捷键 ----
# = 号：显示你在文件中的什么位置（会显示当前页面的内容是文件中第几行到第几行，整个文件所含行数，所含字符数，整个文件所含字符）；
# h 键：显示帮助文档，按 q 键退出帮助文档；
# /（斜杠）：进入搜索模式，只要在斜杠后面输入你要搜索的文字，按下回车键，就会把所有符合的结果都标识出来。要在搜索所得结果中跳# 转，可以按 n 键（跳到下一个符合项目），N 键（shift 键 + n。跳到上一个符合项目）。当然了，正则表达式（Regular # Expression）也是可以用在搜索内容中的。这里我们就不细说什么是正则表达式了，有兴趣可以 百度 / Google 看看；
# n 键：跳到下一个符合的搜索结果；
# N 键：跳到上一个符合的搜索结果。

```

3. tail命令
```
# 实时日志查看
tail -f syslog

#每隔 4 秒检查一次文件是否有更新
tail -f -s 4 syslog
```

4. touch 命令和 mkdir 命令
```
# touch 创建文件， 可以同时创建多个文件
touch new_file new_file_2 


# mkdir 创建目录, 可以同时创建多个目录
mkdir new_folder new_folder_2

# 用 -p 参数来递归地创建目录结构。
mkdir -p one/two/three

```


## 相关概念
### 1. SELinux、Netfilter、iptables、firewall和ufw五者关系
-  Netfilter管网络，selinux管本地。
- iptables是用于设置防火墙，防范来自网络的入侵和实现网络地址转发、QoS等功能，而SELinux则可以理解为是作为Linux文件权限控制（即我们知道的rwx）的补充存在的
- ufw是自2.4版本以后的Linux内核中一个简易的防火墙配置工具，底层还是调用iptables来处理的，iptables可以灵活的定义防火墙规则， 功能非常强大。但是产生的副作用便是配置过于复杂。因此产生一个相对iptables简单很多的防火墙配置工具：ufw
- firewall是centos7里面新的防火墙管理命令，底层还是调用iptables来处理的，主要区别是iptables服务，每次更改都意味着刷新所有旧规则并从/etc/sysconfig/iptables读取所有新规则，firewall可以在运行时更改设置，而不丢失现有连接。
- iptables是Linux下功能强大的应用层防火墙工具, 说到iptables必然提到Netfilter，iptables是应用层的，其实质是一个定义规则的配置工具，而核心的数据包拦截和转发是Netfiler。Netfilter是Linux操作系统核心层内部的一个数据包处理模块  

### 2. SElinux
- 安全增强型 Linux（SELinux）是一种采用安全架构的 Linux® 系统，它能够让管理员更好地管控哪些人可以访问系统。
- 它最初是作为 Linux 内核的一系列补丁，由美国国家安全局（NSA）利用 Linux 安全模块（LSM）开发而成。


## 软连接和硬链接
### 1. Linux软连接和硬链接 
- 参考文章https://zhuanlan.zhihu.com/p/67366919
- 示例
Physical link：物理链接或硬链接
Symbolic link：符号链接或软链接

```shell
# 创建文件
touch f1
# 创建硬链接,  inode节点相同， 多个硬链接， 删除其中某一个对实际文件不影响， 只有所有硬链接都删除后文件才会实际被删除
ln f1 f2
#创建软连接
ln -s f1 f3

# 查看文件inode节点信息
ls -li

```


