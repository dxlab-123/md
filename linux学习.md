# linux学习

## 一、基础指令

系统日志：journalctl -xe

更新：sudo pacman -Syyu

清理空间：sudo pacman -Scc

查找文件：find / -name xxxx

正确的关机流程为：sync > shutdown > reboot > halt

关机指令为：shutdown

sync 将数据由内存同步到硬盘中。

shutdown 关机指令，你可以man shutdown 来看一下帮助文档。

具体操作：

shutdown –h 10 ‘This server will shutdown after 10 mins’ 计算机将在10分钟后关机，并且会显示在登陆用户的当前屏幕中。

shutdown –h now 立马关机

shutdown –h 20:25 系统会在今天20:25关机

shutdown –h +10 十分钟后关机

shutdown –r now 系统立马重启

shutdown –r +10 系统十分钟后重启

reboot 就是重启，等同于 shutdown –r now

halt 关闭系统，等同于shutdown –h now 和 poweroff

## 二、目录结构及常用命令

![20210118170147](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)

目录就相当于 Windows 中的文件夹，目录中存放的既可以是文件，也可以是其他的子目录，而文件中存储的是真正的信息。

文件系统的最顶层是由根目录开始的，系统使用“/”来表示根目录，在根目录之下的既可以是目录，也可以是文件，而每一个目录中又可以包含（子）目录或文件。如此反复就可以构成一个庞大的文件系统。（对大小写敏感）

其中比较重要的：

/dev ：dev 是 Device(设备) 的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的。

/etc：etc 是 Etcetera(等等) 的缩写,这个目录用来存放所有的系统管理所需要的配置文件和子目录。

/home：用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的

/mnt：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，然后进入该目录就可以查看光驱里的内容了。

/opt：opt 是 optional(可选) 的缩写，这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

/root：该目录为系统管理员，也称作超级权限者的用户主目录。

### 1.  ls命令

ls：列出当前目录下的所有文件夹

ls -l:使用列表的形式列出文件夹

![20210119111731](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image004.gif)

ls /home -l：使用列表的形式列出home目录下的文件

![20210122133710](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image006.gif)

  首字母若为 d 则是目录

若为 - 则是文件；

若是 l 则表示为链接文档(link file)；

若是 b 则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；

若是 c 则表示为装置文件里面的串行端口设备，例如键盘(一次性读取装置)。

  接下来的字符中，以三个为一组，且均为 rwx 的三个参数的组合。其中， r 代表可读、 w 代表可写、 x 代表可执行。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号 - 而已。

![IMG_256](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image007.jpg)

  即每个文件的属性由左边第一部分的 10 个字符来确定

![IMG_256](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image009.gif)

ls /：列出根目录(/)下的所有目录

![20210120075000](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image011.gif)

ls --help:了解更多ls的信息

常用搭配：ls -a 列出目录所有文件，包含以.开始的隐藏文件

ls -A 列出除.及..的其它文件

ls -d 仅列出目录本身，而不是列出目录内的文件数据

ls -l 长数据串列出，包含文件的属性与权限等等数据

ls -r 反序排列

ls -t 以文件修改时间排序

ls -S 以文件大小排序

ls -h 以易读大小显示

  实例1：ls -lhrt（按易读方式按时间反序排序，并显示文件详细信息，不分顺序）

![20210122141459](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image013.gif)

  实例2：ls -l b*（列出当前目录中所有以"b"开头的目录的详细内容）

![20210122142101](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image015.gif)

  实例3：ls -al ~（将家目录下的所有文件列出来(含属性与隐藏档)）

![20210122142210](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image017.gif)

### 2. cd命令

  cd：切换当前工作目录

  语法：cd [dirName] 切换当前目录至dirName

  cd /：切换至根目录

  cd ~：切换至home目录

  cd -：切换至上一次工作路径

  cd ..：切换至上两层目录

![20210122144107](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image019.gif)

### 3. pwd命令

  Pwd：查看当前路径

![20210119111201](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image021.gif)

### 4. cat命令(从第一行开始显示内容)

  cat [-AbEnTv]> <filename> 创建一个新文件，并输入输入文件内容，以ctrl+d作为输入结束

  cat [-AbEnTv]<file1> <file2> > file 将file1和file2合并到file里

  cat [-AbEnTv] <filename> 一次显示所有文件

  -A ：相当於 -vET 的整合选项，可列出一些特殊字符而不是空白而已；

-b ：列出行号，仅针对非空白行做行号显示，空白行不标行号！

-E ：将结尾的断行字节 $ 显示出来；

-n ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；

-T ：将 [tab] 按键以 ^I 显示出来；

-v ：列出一些看不出来的特殊字符

![20210120101035](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image023.gif)

### 5. touch命令

### 6. tac命令（文件内容从最后一行开始显示）

实例：tac file

![20210122165347](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image025.gif)

### 7. nl命令（显示内容并显示行号）

  语法：nl [-bnw] 文件

  -b ：指定行号指定的方式，主要有两种：

-b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；

-b t ：如果有空行，空的那一行不要列出行号(默认值)；

-n ：列出行号表示的方法，主要有三种：

-n ln ：行号在荧幕的最左方显示；

-n rn ：行号在自己栏位的最右方显示，且不加 0 ；

-n rz ：行号在自己栏位的最右方显示，且加 0 ；

-w ：行号栏位的占用的位数。

![20210122165616](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image027.gif)

### 8. head命令（取出文件前面几行）

  语法：head [-n number] 文件 

  -n ：后面接数字，代表显示几行的意思（默认情况下，显示前面 10 行）

### 9. mkdir命令

  mkdir：创建一个目录

  -m: 对新建目录设置存取权限，也可以用 chmod 命令设置，不需要则默认权限 (umask)

 -p: 可以是一个路径名称。此时若路径中的某些目录尚不存在,系统将自动建立好那些不在的目录，即一次可以建立多个目录

 实例1：mkdir -p /tmp/test/t1/t （在 tmp 目录下创建路径为 test/t1/t 的目录，若不存在，则创建）

  权限不够sudo mkdir Code：创建一个代码Code的目录

![20210119112427](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image029.gif)

### 10. rmdir命令

  rmdir [-p] 目录名 （只能）删除空目录

  -p 选项用于递归删除空目录

### 11. rmdir (删除空的目录)

  -p ：连同上一级『空的』目录也一起删除

  实例：rmdir runoob/

![20210122150528](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image031.gif)

### 8、 rm 命令来删除非空目录

语法：rm [-fir] 文件或目录

-f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；

-i ：互动模式，在删除前会询问使用者是否动作

-r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！

### 9、 cp命令

语法：cp [-adfilprsu] 来源档(source) 目标档(destination)

   cp [options] source1 source2 source3 .... directory

   -a：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)

-d：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；

-f：强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；

-i：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)

-l：进行硬式连结(hard link)的连结档创建，而非复制文件本身；

-p：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；

-r：递归持续复制，用於目录的复制行为；(常用)

-s：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；

-u：若 destination 比 source 旧才升级 destination ！

![20210122154012](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image033.gif)

### 10、 mv (移动文件与目录，或修改名称)

   语法：mv [-fiu] source destination

​      mv [options] source1 source2 source3 .... directory

   -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；

-i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！

-u ：若目标文件已经存在，且 source 比较新，才会升级 (update)

实例1：mv -i ~/test/test1 test1

![20210122162318](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image035.gif)

![20210122162453](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image037.gif)

![20210122162628](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image039.gif)

实例2：mv test1 test2（将test1改名为test2）

![20210122163015](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image041.gif)

## 三、ping命令

用于检测主机是否运作正常

语法：ping [-dfnqrRv][-c<完成次数>][-i<间隔秒数>][-I<网络界面>][-l<前置载入>][-p<范本样式>][-s<数据包大小>][-t<存活数值>][主机名称或IP地址]

-d 使用Socket的SO_DEBUG功能。

-c<完成次数> 设置完成要求回应的次数。

-f 极限检测。

-i<间隔秒数> 指定收发信息的间隔时间。

-I<网络界面> 使用指定的网络接口送出数据包。

-l<前置载入> 设置在送出要求信息之前，先行发出的数据包。

-n 只输出数值。

-p<范本样式> 设置填满数据包的范本样式。

-q 不显示指令执行过程，开头和结尾的相关信息除外。

-r 忽略普通的Routing Table，直接将数据包送到远端主机上。

-R 记录路由过程。

-s<数据包大小> 设置数据包的大小。

-t<存活数值> 设置存活数值TTL的大小。

-v 详细显示指令的执行过程。

实例1：ping baidu.com(停止需手动Ctrl+C)

![20210119130548](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image043.gif)

实例2：ping -c 2 baidu.com(接收2次包后自动退出)

![20210119130903](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image045.gif)

实例3：ping -i 3 -s 1024 -t 255 baidu.com

​    发送周期为3s，发送包的大小为1024，设置TTL值为255（手打退出）

![20210119132632](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image047.gif)

## 四、PATH环境变量以及常用快捷键

### 1、echo $PATH(查看当前环境变量，显示冒号隔开的一组目录)

![20210119144058](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image049.gif)

### 2、Tab键可以补全命令

![20210119144725](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image051.gif)

### 3、临时设置环境变量，当前会话有效

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image052.gif)![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image053.gif)    export PATH=$PATH:/home/faker/bin (whereis命令)

​    永久有效：在文件最末添加~/.bashrc**？**

![20210119150159](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image055.gif)

### 4、whereis命令

​    whereis ls(查看指令“ls”的位置)

![20210119160436](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image057.gif)

## 五、用户和用户组

用户分成三大类：

root：超级用户，可以用来登录，操作系统任何文件和命令，拥有最高权限

虚拟用户：不具有登陆能力，系统本身拥有，不是后来添加的，但系统运行不可缺的

普通用户：有登陆能力，权限受到限制（sudo提高权限）

### 1、创建一个新用户

​    语法：useradd 选项 用户名

​    -c 指定一段注释性描述。

-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。

-g 用户组 指定用户所属的用户组。

-G 用户组，用户组 指定用户所属的附加组。

-s Shell文件 指定用户的登录Shell。

-u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

​    实例：sudo useradd -d /home/mengning -m mengning （创建一个mengning用户，放在home目录下）

![20210119162151](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image059.gif)

### 2、删除新用户

​    语法：userdel 选项 用户名(常用-r，把主目录一起删除)

​    实例：sudo userdel -r mengning

![20210119163031](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image061.gif)

### 3、whoami/groups

![20210119163435](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image063.gif)

### 4、修改账号

​    语法：usermod 选项 用户名

​    常用的选项包括-c, -d, -m, -g, -G, -s, -u以及-o等，这些选项的意义与useradd命令中的选项一样，可以为用户指定新的资源值。

另外，有些系统可以使用选项：-l 新用户名

### 5、增加一个用户组

​    语法：groupadd 选项 用户组

​    -g 指定新用户组的组标识号（GID）。

-o 一般与-g选项同时使用，表示新用户组的GID可以与已有用户组的GID相同

​    查看：grep 用户组 /etc/group 查看组信息

![20210120131351](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image065.gif)

### 6、删除用户组

​    语法：groupdel <用户组>

### 7、修改用户组

​    语法：groupmod 选项 用户组

​    -g GID 为用户组指定新的组标识号。

-o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。

-n新用户组 将用户组的名字改为新名字

​    实例：（sudo）groupmod –g 10000 -n group3 group2

​    将组group2的标识号改为10000，组名修改为group3

### 8、用户涉及到的系统配置文件

​    /etc/passwd ##用户身份信息文件

\#用户名称:用户密码:用户id：用户主组id:用户说明：用户家目录：用户默认shell

/etc/group ##组身份信息文件

\#组名称：组密码：组id：组的附加成员

/etc/skel/.* ##用户环境配置文件模板

/etc/shadow ##用户认证信息文件

/home/username ##用户家目录

### 9、将用户添加到用户组中

​    usermod -G group1 chenyu（将用户chenyu添加到用户组group1中）

​    ![20210120131249](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image067.gif)

### 10、修改所属用户或组

​    chown username filename

### 11、chown命令

​    用于设置文件所有者和文件关联组的命令（将指定文件的拥有者改为指定的用户或组）

​    语法：chown [-cfhvR] [--help] [--version] user[:group] file...

​    user : 新的文件拥有者的使用者 ID

group : 新的文件拥有者的使用者组(group)

-c : 显示更改的部分的信息

-f : 忽略错误信息

-h :修复符号链接

-v : 显示详细的处理信息

-R : 处理指定目录以及其子目录下的所有文件

--help : 显示辅助说明

--version : 显示版本

实例1：chown root file

![20210120142318](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image069.gif)

​    实例2：chown root.root chengyu(将chengyu目录使用者改成root组下的root用户)

![20210120151110](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image071.gif)    

​    实例3：sudo chown -R root.root chengyu（改变了chengyu及其子目录下的文件）

![20210120151804](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image073.gif)

### 12、chgrp命令

​    变更文件或目录的所属群组。

​    语法：chgrp [-cfhRv][--help][--version][所属群组][文件或目录...]

或chgrp [-cfhRv][--help][--reference=<参考文件或目录>][--version][文件或目录...]

​    -c或--changes 效果类似"-v"参数，但仅回报更改的部分。

-f或--quiet或--silent 　不显示错误信息。

-h或--no-dereference 只对符号连接的文件作修改，而不更动其他任何相关文件。

-R或--recursive 　递归处理，将指定目录下的所有文件及子目录一并处理。

-v或--verbose 显示指令执行过程。

--help 　在线帮助。

--reference=<参考文件或目录> 　把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同。

--version 　显示版本信息。

实例：sudo chgrp root file

![20210120143021](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image075.gif)

### 13、与用户账号有关的系统文件

  /etc/passwd文件是用户管理工作涉及的最重要的一个文件。

​    Linux系统中的每个用户都在/etc/passwd文件中有一个对应的记录行，它记录了这个用户的一些基本属性。

![20210125082040](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image077.gif)

​    其格式和具体含义如下：

​    用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell  

​    /etc/group用户组的所有信息都存放在/etc/group文件中 

![20210125082426](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image079.gif)  

​    格式为 组名:口令:组标识号:组内用户列表                                                                                                                                                                                                                                                                                

14、修改文件权限                                                                                                                                                                                  

​    Chomd [ugo] [=+-] [rwx] filename

​    u：user  g：group  o：other

​    =：赋予  +：增加  -：减少

​    r：可读 w：可写  x：可操作

​    实例1：sudo chmod u+x file（将file文件的user添加可操作）

![20210120155759](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image081.gif)    实例2：sudo chmod ugo=rwx file（将file文件的user，group，other都赋予可读，可       写，可操作）

![20210120160052](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image083.gif)

​    实例3：sudo chomd u=rwx,g=rx,o=r 文件名

![20210122140542](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image085.gif)

### 15、权限值

![20210120161041](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image087.gif)

rwx 7 rw- 6 r-x 5 r-- 4 -wx 3 -w- 2 --x 1

chmod 777 filename=chmod ugo=rwx filename

![20210120161719](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image089.gif)

### 16、find命令（搜索文件）

​    语法：find path -option [-print] [-exec -ok command] {} \

​     -mount, -xdev : 只检查和指定目录在同一个文件系统下的文件，避免列出其它文件系统中的文件

-amin n : 在过去 n 分钟内被读取过

-anewer file : 比文件 file 更晚被读取过的文件

-atime n : 在过去n天内被读取过的文件

-cmin n : 在过去 n 分钟内被修改过

-cnewer file :比文件 file 更新的文件

-ctime n : 在过去n天内被修改过的文件

-empty : 空的文件-gid n or -group name : gid 是 n 或是 group 名称是 name

-ipath p, -path p : 路径名称符合 p 的文件，ipath 会忽略大小写

-name name, -iname name : 文件名称符合 name 的文件。iname 会忽略大小写

-size n : 文件大小 是 n 单位，b 代表 512 位元组的区块，c 表示字元数，k 表示 kilo bytes，w 是二个位元组。

-type c : 文件类型是 c 的文件。

d: 目录

c: 字型装置文件

b: 区块装置文件

p: 具名贮列

f: 一般文件

l: 符号连结

s: socket

-pid n : process id 是 n 的文件

根据文件名搜索：

![20210120162933](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image091.gif)

​    实例1：sudo find ~ -name "terr*" -print（找到根目录下以”teer”开头的文件）

![20210120164920](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image093.gif)

根据权限（perm）类型（type）大小（size）搜索

![20210120165243](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image095.gif)

​    实例2：sudo find ~ -perm 777 -print（找到权限为777的文件）

![20210120165713](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image097.gif)

​    实例3：sudo find ~ ! -type d -print（找到根目录下非目录的文件）

### 17、grep命令（搜索行）

  语法：grep [-abcEFGhHilLnqrsvVwxy][-A<显示行数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]

  参数：-a 或 --text : 不要忽略二进制的数据。

-A<显示行数> 或 --after-context=<显示行数> : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。

-b 或 --byte-offset : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。

-B<显示行数> 或 --before-context=<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前的内容。

-c 或 --count : 计算符合样式的列数。

-C<显示行数> 或 --context=<显示行数>或-<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前后的内容。

-d <动作> 或 --directories=<动作> : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。

-e<范本样式> 或 --regexp=<范本样式> : 指定字符串做为查找文件内容的样式。

-E 或 --extended-regexp : 将样式为延伸的正则表达式来使用。

-f<规则文件> 或 --file=<规则文件> : 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。

-F 或 --fixed-regexp : 将样式视为固定字符串的列表。

-G 或 --basic-regexp : 将样式视为普通的表示法来使用。

-h 或 --no-filename : 在显示符合样式的那一行之前，不标示该行所属的文件名称。

-H 或 --with-filename : 在显示符合样式的那一行之前，表示该行所属的文件名称。

-i 或 --ignore-case : 忽略字符大小写的差别。

-l 或 --file-with-matches : 列出文件内容符合指定的样式的文件名称。

-L 或 --files-without-match : 列出文件内容不符合指定的样式的文件名称。

-n 或 --line-number : 在显示符合样式的那一行之前，标示出该行的列数编号。

-o 或 --only-matching : 只显示匹配PATTERN 部分。

-q 或 --quiet或--silent : 不显示任何信息。

-r 或 --recursive : 此参数的效果和指定"-d recurse"参数相同。

-s 或 --no-messages : 不显示错误信息。

-v 或 --invert-match : 显示不包含匹配文本的所有行。

-V 或 --version : 显示版本信息。

-w 或 --word-regexp : 只显示全字符合的列。

-x --line-regexp : 只显示全列符合的列。

-y : 此参数的效果和指定"-i"参数相同。

![20210121073758](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image099.gif)    实例1：grep 'test' d*

​        grep 'test' d* -r(递归处理，包括下一级的子文件夹)

![20210121074455](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image101.gif)

​    实例2：grep 'test' d* -r data/mysql（显示在data/mysql文件中匹配test的行）

![20210121082153](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image103.gif)

​    实例3：ls -l | grep '^d'

![20210121085330](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image105.gif)

![20210121085556](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image107.gif)

​    实例4：ls -l | grep 't..t' （搜索列出包含“t任意字符任意字符t”的行）

![20210121090509](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image109.gif)

​    实例5：ls -l | grep '[A-Z]' （搜索列出包含A-Z的行）

![20210121090840](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image111.gif)

​    实例6：ls -l | grep '[^a-z]'（不包含a-z）

![20210121091217](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image113.gif)

![20210121091352](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image115.gif)

### 18、tar命令（用来建立，还原备份文件，还可以加入，解开备份文件内的文件）

  语法：tar [-cxtzvfpPN] 文件 目录/文件

![20210121091655](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image117.gif)

​    实例1：tar -zcvf chengyu.tar.gz chengyu（压缩）

![20210121093108](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image119.gif)

​    实例2：tar -zxvf chengyu.tar.gz chengyu（解压缩）

![20210121093634](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image121.gif)

## 六、Linux 链接概念

  Linux 链接分两种，一种被称为硬链接（Hard Link），另一种被称为符号链接（Symbolic Link）。默认情况下，ln 命令产生硬链接。

### 1、硬连接

  硬连接指通过索引节点来进行连接。在 Linux 的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)。在 Linux 中，多个文件名指向同一索引节点是存在的。比如：A 是 B 的硬链接（A 和 B 都是文件名），则 A 的目录项中的 inode 节点号与 B 的目录项中的 inode 节点号相同，即一个 inode 节点对应两个不同的文件名，两个文件名指向同一个文件，A 和 B 对文件系统来说是完全平等的。删除其中任何一个都不会影响另外一个的访问。

硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。其原因如上所述，因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，文件真正删除的条件是与之相关的所有硬连接文件均被删除。

### 2、软连接

  另外一种连接称之为符号连接（Symbolic Link），也叫软连接。软链接文件有类似于 Windows 的快捷方式。它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。比如：A 是 B 的软链接（A 和 B 都是文件名），A 的目录项中的 inode 节点号与 B 的目录项中的 inode 节点号不相同，A 和 B 指向的是两个不同的 inode，继而指向两块不同的数据块。但是 A 的数据块中存放的只是 B 的路径名（可以根据这个找到 B 的目录项）。A 和 B 之间是“主从”关系，如果 B 被删除了，A 仍然存在（因为两个是不同的文件），但指向的是一个无效的链接。

![20210125075553](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image123.gif)

​    从上面的结果中可以看出，硬连接文件 f2 与原文件 f1 的 inode 节点相同，均为 9797648，然而符号连接文件的 inode 节点不同。

![20210125075710](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image125.gif)

​    通过上面的测试可以看出：当删除原始文件 f1 后，硬连接 f2 不受影响，但是符号连接 f1 文件无效

   总结：  

删除符号连接f3,对f1,f2无影响；

删除硬连接f2，对f1,f3也无影响；

删除原文件f1，对硬连接f2没有影响，导致符号连接f3失效；

同时删除原文件f1,硬连接f2，整个文件会真正的被删除。

## 七、磁盘管理

Linux磁盘管理常用三个命令为df、du和fdisk。

df：列出文件系统的整体磁盘使用量

du：检查磁盘空间使用量

fdisk：用于磁盘分区

### 1、df命令

​    语法：df [-ahikHTm] [目录或文件名]

​    -a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；

-k ：以 KBytes 的容量显示各文件系统；

-m ：以 MBytes 的容量显示各文件系统；

-h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；

-H ：以 M=1000K 取代 M=1024K 的进位方式；

-T ：显示文件系统类型, 连同该partition的filesystem名称 (例如 ext3) 也列出

-i ：不用硬盘容量，而以 inode 的数量来显示

​    如果 df 没有加任何选项，那么默认会将系统内所有的 (不含特殊内存内的文件系统与 swap) 都以 1 Kbytes 的容量来列出来！

### 2、du命令

​    是对文件和目录磁盘使用的空间的查看

​    语法：du [-ahskm] 文件或目录名称

​    -a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。

-h ：以人们较易读的容量格式 (G/M) 显示；

-s ：列出总量而已，而不列出每个各别的目录占用容量；

-S ：不包括子目录下的总计，与 -s 有点差别。

-k ：以 KBytes 列出容量显示；

-m ：以 MBytes 列出容量显示；

### 3、fdisk命令

​    fdisk 是 Linux 的磁盘分区表操作工具

​    语法：fdisk [-l] 装置名称

​     -l ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。

​    实例：找出你系统中的根目录所在磁盘，并查阅该硬盘内的相关信息

![20210125093112](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image127.gif)

​    输入 m 后，就会看到底下这些命令介绍

![20210125093157](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image129.gif)

​    离开 fdisk 时按下 q，那么所有的动作都不会生效！相反的， 按下w就是动作生效的意思。

![20210125093327](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image131.gif)

### 4、mkfs命令（格式化磁盘）

​    磁盘分割完毕后自然就是要进行文件系统的格式化

语法：mkfs [-t 文件系统格式] 装置文件名

​    -t ：可以接文件系统格式，例如 ext3, ext2, vfat 等(系统有支持才会生效)

### 5、fsck命令

​    用来检查和维护不一致的文件系统

​    语法：fsck [-t 文件系统] [-ACay] 装置名称

​    -t : 给定档案系统的型式，若在 /etc/fstab 中已有定义或 kernel 本身已支援的则不需加上此参数

-s : 依序一个一个地执行 fsck 的指令来检查

-A : 对/etc/fstab 中所有列出来的 分区（partition）做检查

-C : 显示完整的检查进度

-d : 打印出 e2fsck 的 debug 结果

-p : 同时有 -A 条件时，同时有多个 fsck 的检查一起执行

-R : 同时有 -A 条件时，省略 / 不检查

-V : 详细显示模式

-a : 如果检查有错则自动修复

-r : 如果检查有错则由使用者回答是否修复

-y : 选项指定检测每个文件是自动输入yes，在不确定那些是不正常的时候，可以执行 # fsck -y 全部检查修复。

### 6、磁盘的挂载 mount 命令

​    语法：mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n] 装置文件名 挂载点

​    语法：mount 查看已挂载设备

​    实例：mkdir /mnt/sda2(创建空目录)

​       mount /dev/sda2 /mnt/sda2(把/dev/sda2挂载到/mnt/sda2)

​       mount (查看以挂载设备信息)

![20210125101417](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image133.gif)

### 7、磁盘的卸载umount 语法

​    语法：umount [-fn] 装置文件名或挂载点

​    -f ：强制卸除！可用在类似网络文件系统 (NFS) 无法读取到的情况下；

-n ：不升级 /etc/mtab 情况下卸除。

​    实例：umount /mnt/sda2（卸载挂载点）

## 八、vim编辑器

![IMG_256](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image135.jpg)

  vim 共分为三种模式，分别是命令模式（Command mode），输入模式（Insert mode）和底线命令模式（Last line mode）。

命令模式：刚刚启动vim，便进入了命令模式。此状态下敲击键盘动作会被Vim识别为命令，而非输入字符。常用的几个命令：

i 切换到输入模式，以输入字符。

x 删除当前光标所在处的字符。

: 切换到底线命令模式，以在最底一行输入命令。

输入模式：在命令模式下按下i就进入了输入模式。

字符按键以及Shift组合，输入字符

ENTER，回车键，换行

BACK SPACE，退格键，删除光标前一个字符

DEL，删除键，删除光标后一个字符

方向键，在文本中移动光标

HOME/END，移动光标到行首/行尾

Page Up/Page Down，上/下翻页

Insert，切换光标为输入/替换模式，光标将变成竖线/下划线

ESC，退出输入模式，切换到命令模式

底线命令模式：在命令模式下按下:（英文冒号）就进入了底线命令模式。

q 退出程序

w 保存文件

按ESC键可随时退出底线命令模式

![IMG_256](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image137.jpg)

  批量添加注释：

  （替换命令）添加注释：

使用名命令格式： :起始行号,结束行号s/^/注释符/g（注意冒号）

（替换命令）取消注释

使用名命令格式： :起始行号,结束行号s/^注释符//g（注意冒号）。

常用按键：

![20210125105759](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image139.gif)

![20210125105822](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image141.gif)

![20210125105850](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image143.gif)

![20210125105924](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image145.gif)

![20210125110000](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image147.gif)

![20210125110037](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image149.gif)

![20210125110200](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image151.gif)

![20210125110237](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image153.gif)

![20210125110350](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image155.gif)

![20210125110447](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image157.gif)

![20210125110514](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image159.gif)

![20210125110618](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image161.gif)

## 九、shell  

### 1、shell的简介

ü Shell是外壳，界面，通常指的是shell脚本

ü 将多个程序组装成大型程序，而shell是最好的粘合剂

ü 优点：简单、高效、易维护、随写随用——>Linux越用越简单

ü Shell开发流程：需求分析——>问题建模——>伪码流程逻辑——>代码实施与运行——>总结

ü 编码规范

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image163.jpg)

ü Hello,world

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image165.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image167.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image169.jpg)

ü 注释（多行注释不常用）

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image171.jpg)

### 2、基本语法

ü 变量

l 可以改变的量

l 命名只能用字母、数字和下划线

l 首个字符不能以数字开头

l 中间不能有空格，可以使用下划线_

l 不能使用关键字

分为局部变量、环境变量（全局变量）、特殊变量

局部变量：只在当前shell有用；在脚本或命令中定义；函数内的变量加local

变量的定义和使用

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image173.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image175.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image177.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image179.jpg)

环境变量（全局变量）

所有程序都能访问环境变量；全部大写

env:找出当前的环境变量

export:导出环境变量

source与.：引用环境变量

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image181.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image183.jpg)

特殊变量

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image185.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image187.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image189.jpg)

### 3、基本运算

ü 算术运算

  ![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image191.jpg)

  ![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image193.jpg)

ü 关系运算

  ![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image195.jpg)  ![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image197.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image199.jpg)

ü 布尔与逻辑运算

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image201.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image203.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image205.jpg)

ü 文件测试运算

  ![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image207.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image209.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image211.jpg)

### 4、字符串（尽量不要用单引号）

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image213.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image215.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image217.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image219.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image221.jpg)

字符串运算符

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image223.jpg)

数组

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image225.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image227.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image229.jpg)

### 5、分支

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image231.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image233.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image235.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image237.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image239.jpg)

### 6、循环

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image241.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image243.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image245.jpg)

### 7、函数

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image247.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image249.jpg)

### 8、常用命令

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image251.jpg)

9、实战演练

  猜数字

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image253.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image255.jpg)

​    获取当前CPU使用率

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image257.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image259.jpg)

​    探测本地网络

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image261.jpg)

​    写日志

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image263.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image265.jpg)

## 十、Linux开发基础（编译、构建、调试）

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image267.jpg)

⒈预处理,生成.i的文件[ 预处理器cpp] gcc -E -o *.cpp *.c

⒉将预处理后的文件转换成汇编语言，生成文件.s[ 编译器egcs]

gcc -x cpp-output -S hello.s hello.cpp -o *.s *.cpp

或者直接转换成汇编：gcc -S -o *.s *.c

⒊由汇编变为目标代码（机器代码）生成.o的文件[ 汇编器as]

gcc -x assembler -c *.s -o *.o

gcc -c *.c -o *.o

as -o *.o *.s

⒋连接目标代码，生成可执行程序[ 链接器ld]

gcc -o * *.o

gcc -o * *.c

运行hello.c

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image269.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image271.jpg)

Makefile的规则

<target>：<prerequisites>

[tab]<commands>(任意的shell命令)

make如何工作

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image273.jpg)

目标（target）

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image275.jpg)

前置条件（prerequisites）

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image277.jpg)

命令（commands）

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image279.jpg)

makefile文件的语法

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image281.jpg)

模式匹配

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image283.jpg)

变量和赋值

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image285.jpg)

四个赋值运算符（=、:=、?=、+=）

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image287.jpg)

内置变量

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image289.jpg)

自动变量

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image291.jpg)

判断和循环

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image293.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image295.jpg)

函数

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image297.jpg)

Makefile的实例

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image299.jpg)

实例：

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image301.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image303.jpg)

GDB

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image305.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image307.jpg)

使用断点

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image309.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image311.jpg)

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image313.jpg)

变量跟踪

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image315.jpg)

堆栈跟踪

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image317.jpg)

实例：

（代码）

\#include <stdio.h>

\#include <stdlib.h>

 

int main(int argc, char **argv)

{

  int i;

  int a=0, b=0, c=0;

  double d;

  for (i=0; i<100; i++)

  {

​    a++;

​    if (i>97)

​      d = i / 2.0;

​    b++;

  }

  return 0;

}

## 十一、Linux网络基础

模型简介

![img](file:///C:/Users/Administrator/AppData/Local/Temp/msohtmlclip1/01/clip_image319.jpg)

## 十二、设置内核启动参数

默认cgroup v2，现设置为cgroup v1

将 /etc/default/grub 文件中的 GRUB_CMDLINE_LINUX 字段内容中添加 systemd.unified_cgroup_hierarchy=1，然后 sudo update-grub，重启电脑

GRUB_CMDLINE_LINUX="systemd.unified_cgroup_hierarchy=0"

改回cgroup v2，改回1

