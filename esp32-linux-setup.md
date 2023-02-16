详细安装步骤
=========================

请根据下方详细步骤，完成安装过程。

**设置开发环境**


以下是为 ESP32设置 ESP-IDF 的具体步骤。

* 第一步：安装准备
* 第二步：获取 ESP-IDF
* 第三步：获取工具
* 第四步：设置环境变量
* 第五步：开始使用 RSO-IDF吧

第一步：安装准备
=============================

为了在 ESP32中使用 ESP-IDF，需要根据操作系统安装一些软件包。以下安装指南可协助您安装 Linux 和 macOS 的系统上所有需要的软件包。

**Linux 用户**


编译 ESP-IDF 需要以下软件包。请根据使用的 Linux 发行版本，选择合适的安装命令。

- Ubuntu 和 Debian:

    ```zsh
    sudo apt-get install git wget flex bison gperf python3 python3-venv python3-setuptools cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
    ```

- CentOS 7 & 8:

    ```zsh
    sudo yum -y update && sudo yum install git wget flex bison gperf python3 python3-setuptools cmake ninja-build ccache dfu-util libusbx
    ```
    
    

目前仍然支持 CentOS 7，但为了更好的用户体验，建议使用 CentOS 8。

- Arch:

    ```zsh
    sudo pacman -S --needed gcc git make flex bison gperf python cmake ninja ccache dfu-util libusb
    ```
    

**备注**


- 使用 ESP-IDF 需要 CMake 3.16 或以上版本。较早的 Linux 发行版可能需要升级自身的软件源仓库，或开启 backports 套件库，或安装 "cmake3" 软件包（不是安装 "cmake"）。
- 如果上述列表中没有您使用的系统，请参考您所用系统的相关文档，查看安装软件包所用的命令。

**macOS 用户**

ESP-IDF 将使用 macOS 上默认安装的 Python 版本。

- 安装 CMake 和 Ninja 编译工具：

    - 若有 HomeBrew_，您可以运行:

        ```zsh
        brew install cmake ninja dfu-util
        ```

    - 若有 MacPorts_，您可以运行::

        ```zsh
        sudo port install cmake ninja dfu-util
        ```

    - 若以上均不适用，请访问 CMake_ 和 Ninja_ 主页，查询有关 macOS 平台的下载安装问题。

- 强烈建议同时安装 ccache_ 以获得更快的编译速度。如有 HomeBrew_，可通过 MacPorts_ 上的 ``brew install ccache`` 或 ``sudo port install ccache`` 完成安装。

**备注**
    如您在上述任何步骤中遇到以下错误:

```zsh
 xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

则必须安装 XCode 命令行工具，可运行 ``xcode-select --install`` 命令进行安装。

**Apple M1 用户**

如果您使用的是 Apple M1 系列且看到如下错误提示:

```zsh
  WARNING: directory for tool xtensa-esp32-elf version esp-2021r2-patch3-8.4.0 is present, but tool was not found
  ERROR: tool xtensa-esp32-elf has no installed versions. Please run 'install.sh' to install it.
```

或者:

```zsh
  zsh: bad CPU type in executable: ~/.espressif/tools/xtensa-esp32-elf/esp-2021r2-patch3-8.4.0/xtensa-esp32-elf/bin/xtensa-esp32-elf-gcc
```

您需要运行如下命令来安装 Apple Rosetta 2：

```zsh
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
```

**安装 Python 3**

Catalina 10.15 发布说明中表示不推荐使用 Python 2.7 版本，在未来的 macOS 版本中也不会默认包含 Python 2.7。执行以下命令来检查您当前使用的 Python 版本:

```zsh
  python --version
```

如果输出结果是 ``Python 2.7.17``，则代表您的默认解析器是 Python 2.7。这时需要您运行以下命令检查电脑上是否已经安装过 Python 3::

```zsh
  python3 --version
```

如果运行上述命令出现错误，则代表电脑上没有安装 Python 3。

请根据以下步骤安装 Python 3：

  - 使用 HomeBrew_ 进行安装的方法如下::
      ```zsh
      brew install python3
      ```
  - 使用 MacPorts_ 进行安装的方法如下::
      ```zsh
      sudo port install python38
      ```

第二步：获取 ESP-IDF
=================================

在围绕 ESP32构建应用程序之前，请先获取乐鑫提供的软件库文件 `ESP-IDF 仓库`。

获取 ESP-IDF 的本地副本：打开终端，切换到您要保存 ESP-IDF 的工作目录，使用 ``git clone`` 命令克隆远程仓库。针对不同操作系统的详细步骤，请见下文。

打开终端，运行以下命令：
```zsh
mkdir -p ~/esp
cd ~/esp
git clone --recursive https://github.com/espressif/esp-idf.git
```
ESP-IDF 将下载至 ``~/esp/esp-idf``。

第三步：设置工具
=================================

除了 ESP-IDF 本身，您还需要为支持 ESP32的项目安装 ESP-IDF 使用的各种工具，比如编译器、调试器、Python 包等。

```zsh
cd ~/esp/esp-idf
./install.sh esp32
```

或使用 Fish shell：

```zsh
cd ~/esp/esp-idf
./install.fish esp32
```

上述命令仅仅为 ESP32安装所需工具。如果需要为多个目标芯片开发项目，则可以一次性指定多个目标，如下所示：

```zsh
cd ~/esp/esp-idf
./install.sh esp32,esp32s2
```

或使用 Fish shell：

```zsh
cd ~/esp/esp-idf
./install.fish esp32,esp32s2
```

如果需要一次性为所有支持的目标芯片安装工具，可以运行如下命令：

```bash
cd ~/esp/esp-idf
./install.sh all
```

或使用 Fish shell：

```bash
cd ~/esp/esp-idf
./install.fish all
```

**备注**   

 对于 macOS 用户，如您在上述任何步骤中遇到以下错误::

```bash
 <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:xxx)
```

可运行您电脑 Python 文件夹中的 ``Install Certificates.command`` 安装证书。了解更多信息，请参考 `安装 ESP-IDF 工具时出现的下载错误 <https://github.com/espressif/esp-idf/issues/4775>`_。

**下载工具备选方案**

ESP-IDF 工具安装器会下载 Github 发布版本中附带的一些工具，如果访问 Github 较为缓慢，可以设置一个环境变量，从而优先选择 Espressif 的下载服务器进行 Github 资源下载。

**备注**

 该设置只影响从 Github 发布版本中下载的单个工具，它并不会改变访问任何 Git 仓库的 URL。

要在安装工具时优先选择 Espressif 下载服务器，请在运行 ``install.sh`` 时使用以下命令：

```bash
cd ~/esp/esp-idf
export IDF_GITHUB_ASSETS="dl.espressif.com/github_assets"
./install.sh
```

**自定义工具安装路径**

本步骤中介绍的脚本将 ESP-IDF 所需的编译工具默认安装在用户的根目录中，即 Linux 系统中的 ``$HOME/.espressif`` 目录。您可以选择将工具安装到其他目录中，但请在运行安装脚本前，重新设置环境变量 ``IDF_TOOLS_PATH``。注意，请确保您的用户账号已经具备了读写该路径的权限。

如果修改了 ``IDF_TOOLS_PATH`` 变量，请确保该变量在每次执行安装脚本 (``install.bat``、``install.ps1`` 或 ``install.sh``) 和导出脚本 (``export.bat``、``export.ps1`` 或 ``export.sh``) 均保持一致。

第四步：设置环境变量
=======================================

此时，您刚刚安装的工具尚未添加至 PATH 环境变量，无法通过“命令窗口”使用这些工具。因此，必须设置一些环境变量。这可以通过 ESP-IDF 提供的另一个脚本进行设置。

请在需要运行 ESP-IDF 的终端窗口运行以下命令：

```zsh
. $HOME/esp/esp-idf/export.sh
```

对于 fish shell（仅支持 fish 3.0.0 及以上版本），请运行以下命令：

```zsh
. $HOME/esp/esp-idf/export.fish
```

注意，命令开始的 "." 与路径之间应有一个空格！

如果您需要经常运行 ESP-IDF，您可以为执行 ``export.sh`` 创建一个别名，具体步骤如下：

1. 复制并粘贴以下命令到 shell 配置文件中（``.profile``、``.bashrc``、``.zprofile`` 、``.zshrc``等）

   ```zsh
 alias get_idf='. $HOME/esp/esp-idf/export.sh'
   ```

2. 通过重启终端窗口或运行 ``source [path to profile]``，如 ``source ~/.bashrc`` 来刷新配置文件。

现在您可以在任何终端窗口中运行 ``get_idf`` 来设置或刷新 esp-idf 环境。

不建议直接将 ``export.sh`` 添加到 shell 的配置文件。这样做会导致在每个终端会话中都激活 IDF 虚拟环境（包括无需使用 IDF 的会话）。这违背了使用虚拟环境的目的，还可能影响其他软件的使用。

实例:

```shell
export GOPATH=/home/marces/go
export IDF_PATH=/home/marces/esp/esp-idf
export PATH=$PATH:/home/marces/esp/esp-idf/tools

alias get_idf='. $HOME/esp/esp-idf/export.sh'
```

第五步：开始使用 ESP-IDF 吧
========================================



