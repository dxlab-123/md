现在您已经具备了使用 ESP-IDF 的所有条件，接下来将介绍如何开始您的第一个工程。

本指南将帮助您完成使用 ESP-IDF 的第一步。按照本指南，您将使用 esp32 创建第一个工程，并构建、烧录和监控设备输出。

开始创建工程
================

现在，您可以准备开发ESP32应用程序了。您可以从 ESP-IDF 中 :idf:`examples` 目录下的 :example:`get-started/hello_world` 工程开始。

**重要**

ESP-IDF 编译系统不支持 ESP-IDF 路径或其工程路径中带有空格。

将 :example:`get-started/hello_world` 工程复制至您本地的 ``~/esp`` 目录下：

```zsh
cd ~/esp
cp -r $IDF_PATH/examples/get-started/hello_world .
```

**备注**

ESP-IDF 的 :idf:`examples` 目录下有一系列示例工程，您可以按照上述方法复制并运行其中的任何示例，也可以直接编译示例，无需进行复制。

连接设备
==============

现在，请将您的ESP32开发板连接到 PC，并查看开发板使用的串口。

通常，串口在不同操作系统下显示的名称有所不同：

- **Linux 操作系统：** 以 ``/dev/tty`` 开头
- **macOS 操作系统：** 以 ``/dev/cu.`` 开头

有关如何查看串口名称的详细信息，请见 :doc:`establish-serial-connection`。

**备注**

请记住串口名，您会在后续步骤中使用。

配置工程
=============

请进入 ``hello_world`` 目录，设置ESP32为目标芯片，然后运行工程配置工具 ``menuconfig``。

```zsh
cd ~/esp/hello_world
idf.py set-target {IDF_TARGET_PATH_NAME}
idf.py menuconfig
```

打开一个新工程后，应首先使用 ``idf.py set-target {IDF_TARGET_PATH_NAME}`` 设置“目标”芯片。注意，此操作将清除并初始化项目之前的编译和配置（如有）。您也可以直接将“目标”配置为环境变量（此时可跳过该步骤）。更多信息，请见 :ref:`selecting-idf-target`。

正确操作上述步骤后，系统将显示以下菜单：

https://github.com/espressif/esp-idf/blob/236fa5e6698a262aed534d2a46c78711204b903a/docs/_static/project-configuration.png

工程配置 — 主窗口

您可以通过此菜单设置项目的具体变量，包括 Wi-Fi 网络名称、密码和处理器速度等。``hello_world`` 示例项目会以默认配置运行，因此在这一项目中，可以跳过使用 ``menuconfig`` 进行项目配置这一步骤。

**注意**

​    如果您使用的是 ESP32-DevKitC（板载 ESP32-SOLO-1 模组）或 ESP32-DevKitM-1（板载 ESP32-MINI-1(1U) 模组），请在烧写示例程序前，前往 ``menuconfig`` 中使能单核模式（:ref:`CONFIG_FREERTOS_UNICORE`）。

**备注**

您终端窗口中显示出的菜单颜色可能会与上图不同。您可以通过选项 ``--style`` 来改变外观。请运行 ``idf.py menuconfig --help`` 命令，获取更多信息。
