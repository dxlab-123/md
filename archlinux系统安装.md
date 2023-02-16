**在开头我还是要说一句，多看[Arch Wiki](https://wiki.archlinux.org/)！**

### 下载Arch Linux安装镜像

https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/

下载iso后缀的镜像文件，这个应该不用多说了。

### 刷写安装镜像至U盘

这个也不用多说了吧，刷写软件我用的是Rufus，操作前注意备份数据。

### 重启进入LiveCD环境

不说了……一切正常的话应该会有`root@archiso ~ #`的显示。

### 磁盘分区、格式化以及如何挂载

**数据无价，谨慎操作。**

执行`fdisk -l`，查看硬盘信息，看看哪块硬盘是你要操作的。这里以你要安装Arch Linux的硬盘是/dev/sda，而你要使用整块硬盘安装Arch Linux为前提做示范。

执行`fdisk /dev/sda`，进入fdisk命令行。这里将根据两种启动方式做不同操作。

#### UEFI启动

输入`g`创建GPT分区表。

输入`n`，创建新分区

第一问：`Partition number`（分区号）保持默认。

第二问：`First sector`（起始扇区）保持默认。

第三问：`Last sector`（结束扇区）填入`+300MB`，表示创建一个300MB的分区。

输入`t`，修改分区类型。

第一问：`Partition type`（分区类型）输入`1`（EFI分区）。

EFI分区创建完成。

再次输入`n`，创建新分区

第一问：`Partition number`（分区号）保持默认。

第二问：`First sector`（起始扇区）保持默认。

第三问：`Last sector`（结束扇区）填入`+4096MB`，表示创建一个4G的分区。
swap分区创建完成。

再次输入`n`，创建新分区

第一问：`Partition number`（分区号）保持默认。

第二问：`First sector`（起始扇区）保持默认。

第三问：`Last sector`（结束扇区）保持默认。

系统分区创建完成。

输入`w`，写入修改。

分区完成

格式化`boot分区`。

```sh
mkfs.fat -F32 /dev/sda1
```

格式化`系统分区`。

```sh
mkfs.ext4 /dev/sda3
```

格式化`swap分区`。

```sh
mkswap /dev/sda2
```

激活`swap分区`。

```sh
swapon /dev/sda2
```

格式化完成。

将系统分区/dev/sda3挂载到/mnt目录。

```sh
mount /dev/sda3 /mnt
```

创建boot分区的挂载点。

```sh
mkdir /mnt/boot
```

将boot分区/dev/sda1挂载到/mnt/boot目录。

```sh
mount /dev/sda1 /mnt/boot
```

挂载操作完成

### 系统安装

终于进入正题了。

先连接网络。

```
dhcpcd
```

如果你用的是无线网络，要连接WLAN的话就运行`wifi-menu`，会弹出来伪GUI，用键盘操作……

#### 修改软件源（法一）

为什么要修改软件源呢？因为默认的软件源在国外，下载速度很慢。

输入`nano /etc/pacman.d/mirrorlist`，打开nano。

按下`Ctrl+W`，键入`TUNA`，查找清华镜像源的位置。先把光标移动到行尾，按住`Shift`再配合方向键移动光标，你会惊奇地发现nano可以选中文本……你需要框选这么两行文字。

```shsh
## China
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch     （重要）
```

按下`ctrl+K`，你会更加惊奇地发现nano还能剪切文本。按住`PgUp`按键，把光标移动到文件首部。

把光标移动到注释下面的那个空行，按`Enter`创建一个新的行，再按下`ctrl+U`粘帖。

```sh
##
## Arch Linux repository mirrorlist
## Filtered by mirror score from mirror status page
## Generated on xxxx-xx-xx
##
```

按下`Ctrl+O`，按`Enter`保存。最后按下`Ctrl+X`，退出nano。

#### 修改软件源（法二）

```sh
echo "## Score: 5.9, China" >> /etc/pacman.d/mirrorlist

echo "Server = http://mirrors.163.com/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist     (重要)

echo "## Score: 5.9, China" >>/etc/pacman.d/mirrorlist

echo "Server = http://mirrors.sohu.com/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist

echo "## Score: 5.9, China" >> /etc/pacman.d/mirrorlist

echo "Server = http://mirrors.hustunique.com/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist

echo "## Score: 5.9, China" >> /etc/pacman.d/mirrorlist

echo "Server =http://mirrors.ustc.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist
```

#### 安装基本系统

安装linux和linux-firmware

```
pacstrap /mnt base base-devel linux linux-firmware
```

#### 配置系统

用这个命令生成fstab，fstab记录了自动挂载分区的信息。

```
genfstab -U /mnt >> /mnt/etc/fstab
```

chroot到新安装的系统，也就是说你现在使用的终端里`/`被改成了`/mnt`，按`Ctrl+D`可以退出。

```
arch-chroot /mnt
```

先安装点常用工具。

```
pacman -Syu && pacman -S vim dhcpcd networkmanager grub
```

设置时区，反正都是东8区，就设置成上海吧。

```
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

好像是同步时间用的……

```
hwclock --systohc
```

修改`locale.gen`，设置你所在的地区。

```
vim /etc/locale.gen
```

按下`/`，输入`zh_CN.UTF-8 UTF-8`，按下回车。

按下`i`，进入编辑模式，用键盘移动光标删除掉跳转到的这一行头部的`#`。

这一行应该看起来是这样的。

```
zh_CN.UTF-8 UTF-8
```

最后按`Esc`，输入`:wq`，写入并退出。

接着执行`locale-gen`以生成 locale 信息。

```
locale-gen
```

编辑`locale.conf`以设置语言。

```
vim /etc/locale.conf
```

按下`i`，进入编辑模式，输入`LANG=en_US.UTF-8`，按下`Esc`，输入`:wq`，写入并退出。

最后这个文件看起来应该是这样的，为什么不设置成中文呢？因为这样做的话tty会乱码……

```
LANG=en_US.UTF-8
```

键盘布局就不修改了，反正大部分人用的都是qwerty键盘。

设置主机名。

```
vim /etc/hostname
```

按照上文教的编辑方法输入你喜欢的名字并保存，我设置成了`jetlinks-PC`。

修改`hosts`，这可以看做一个本地的DNS。

```
vim /etc/hosts
```

输入以下内容并保存。

```
127.0.0.1	localhost
::1		localhost
```

设置root密码。

```
passwd
```

输入你想要设置的密码，密码不会有任何显示，你觉得没问题就按回车吧，密码要求输入2遍。

#### 安装引导程序

很多人都在这一步翻了车啊，包括我。这里我将使用GRUB做示范，想用其他引导器的话请自行翻阅Arch Wiki。这里还是要分两种引导方式讲解……

##### UEFI

现在应该已经没人用32位的处理器了吧。

```
grub-install --target=x86_64-efi --efi-directory=/boot/efi --removable
```

##### 传统BIOS

```
grub-install --target=i386-pc /dev/sda
```

生成grub配置文件，千万别忘了这一步，我曾经因为这个翻车了。

```
grub-mkconfig -o /boot/grub/grub.cfg
```

### 重启进入系统

按下`Ctrl+D`退出chroot，执行`reboot`重启。

系统启动后你会看到这样几行字。

```
Arch Linux x.x.x-arch1-1 (tty1)
xxxxxxxx login:
```

输入`root`，再输入你设置的密码，你应该就成功登陆了，到这里就已经算是安装完成了。

#### 联网设置

启动`dhcpcd`。

```sh
systemctl start dhcpcd

systemctl enable dhcpcd
```

网络设置好后，可以ping一下看网络是否连通。

```sh
ping baidu.com
```

#### 更新系统

```sh
pacman -Syyu
```

#### 添加普通用户

```sh
useradd -mG wheel faker
```

**给新用户设置密码**

```sh
passwd faker
```

**修改 /etc/sudoers 文件**

```sh
vim /etc/sudoers
```

在/etc/sudoers文件中放开以下代码的注释可以使用sudo命令。

```
# 放开此行的注释
%wheel ALL=(ALL) ALL
```

#### 配置远程登录

安装openssh

```sh
pacman -S openssh
```

设置密码，开启服务

```sh
passwd
systemctl restart sshd.service
systemctl enable sshd.service
```

编辑`/etc/ssh/sshd_config`
 去掉一下两项前的`#`号，PermitRootLogin 改为yes

```bash
#Port 22
#PermitRootLogin prohibit-password
```

```bash
# Port可以改为较大端口
Port 22
PermitRootLogin yes
```
