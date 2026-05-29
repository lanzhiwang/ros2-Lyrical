# Ubuntu (deb packages)

* https://docs.ros.org/en/lyrical/Installation/Ubuntu-Install-Debs.html

Deb packages for ROS 2 Lyrical Luth are currently available for Ubuntu Resolute (26.04). The Rolling Ridley distribution will change target platforms from time to time as new platforms are selected for development. The target platforms are defined in [REP 2000](https://reps.openrobotics.org/rep-2000/). Most people will want to use a stable ROS distribution.
目前, 适用于 Ubuntu Resolute (26.04) 的 ROS 2 Lyrical Luth 的 deb 软件包可用. Rolling Ridley 发行版会随着新平台的开发而不时更改目标平台. 目标平台定义在 [REP 2000](https://reps.openrobotics.org/rep-2000/) 中. 大多数用户都希望使用稳定的 ROS 发行版.

## Resources

- Status Page:

  - ROS 2 Lyrical (Ubuntu Resolute Raccoon 26.04): [amd64](http://repo.ros2.org/status_page/ros_lyrical_default.html), [arm64](http://repo.ros2.org/status_page/ros_lyrical_unv8.html)

- [Jenkins Instance](http://build.ros2.org/)

- [Repositories](http://repo.ros2.org/)


## System setup

### Set locale

Make sure you have a locale which supports `UTF-8`. If you are in a minimal environment (such as a docker container), the locale may be something minimal like `POSIX`. We test with the following settings. However, it should be fine if you’re using a different UTF-8 supported locale.
请确保您使用的语言环境支持 `UTF-8` 编码. 如果您使用的是最小化环境(例如 Docker 容器), 则语言环境可能是类似 `POSIX` 这样的最小化设置. 我们使用以下设置进行测试. 但是, 如果您使用其他支持 UTF-8 编码的语言环境, 应该也不会有问题.

```bash
locale # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale # verify settings
```

### Enable required repositories
启用所需存储库

You will need to add the ROS 2 apt repository to your system.
您需要将 ROS 2 apt 软件仓库添加到您的系统中.

First ensure that the [Ubuntu Universe repository](https://help.ubuntu.com/community/Repositories/Ubuntu) is enabled.
首先确保已启用 [Ubuntu Universe 软件仓库](https://help.ubuntu.com/community/Repositories/Ubuntu).

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

The [ros-apt-source](https://github.com/ros-infrastructure/ros-apt-source/) packages provide keys and apt source configuration for the various ROS repositories.
[ros-apt-source](https://github.com/ros-infrastructure/ros-apt-source/) 软件包为各种 ROS 存储库提供密钥和 apt 源配置.

Installing the ros2-apt-source package will configure ROS 2 repositories for your system. Updates to repository configuration will occur automatically when new versions of this package are released to the ROS repositories.
安装 ros2-apt-source 软件包将为您的系统配置 ROS 2 软件仓库. 当 ROS 软件仓库发布此软件包的新版本时, 软件仓库配置将自动更新.

```bash
sudo apt update && sudo apt install curl -y

export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')

curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"

sudo dpkg -i /tmp/ros2-apt-source.deb
```

### Install development tools (optional)
安装开发工具(可选)

If you are going to build ROS packages or otherwise do development, you can also install the development tools:
如果您打算构建 ROS 软件包或进行其他开发工作, 您还可以安装开发工具:

```bash
sudo apt update && sudo apt install ros-dev-tools
```

## Install ROS 2

Update your apt repository caches after setting up the repositories.
设置完软件源后, 请更新 apt 软件源缓存.

```bash
sudo apt update
```

ROS 2 packages are built on frequently updated Ubuntu systems. It is always recommended that you ensure your system is up to date before installing new packages.
ROS 2 软件包构建于频繁更新的 Ubuntu 系统之上. 因此, 我们始终建议您在安装新软件包之前确保系统已更新至最新版本.

```bash
sudo apt upgrade
```

Desktop Install (Recommended): ROS, RViz, demos, tutorials.
桌面安装(推荐): ROS、RViz、演示、教程.

```bash
sudo apt install ros-lyrical-desktop
```

ROS-Base Install (Bare Bones): Communication libraries, message packages, command line tools. No GUI tools.
ROS 基础安装(精简版): 包含通信库、消息包和命令行工具. 不包含图形用户界面工具.

```bash
sudo apt install ros-lyrical-ros-base
```

### Install additional RMW implementations (optional)
安装其他 RMW 实现(可选)

The default middleware that ROS 2 uses is `Fast DDS`, but the middleware (RMW) can be replaced at runtime. See the [guide](https://docs.ros.org/en/lyrical/How-To-Guides/Working-with-multiple-RMW-implementations.html) on how to work with multiple RMWs.
ROS 2 默认使用的中间件是 `Fast DDS`, 但可以在运行时替换中间件 (RMW). 请参阅[指南](https://docs.ros.org/en/lyrical/How-To-Guides/Working-with-multiple-RMW-implementations.html)了解如何使用多个 RMW.

## Setup environment

Set up your environment by sourcing the following file.
通过运行以下文件来设置您的环境.

```bash
source /opt/ros/lyrical/setup.bash
```

> Note
> Replace `.bash` with your shell if you’re not using bash. Possible values are: `setup.bash`, `setup.sh`, `setup.zsh`.
> 如果您不使用 bash, 请将 `.bash` 替换为您的 shell. 可选值有: `setup.bash`、`setup.sh`、`setup.zsh`.

## Try some examples
尝试一些例子

If you installed `ros-lyrical-desktop` above you can try some examples.
如果您已安装上述 `ros-lyrical-desktop` 则可以尝试一些示例.

In one terminal, source the setup file and then run a C++ `talker`:
在一个终端中, 运行 setup 文件, 然后运行一个 C++ `talker`:

```bash
source /opt/ros/lyrical/setup.bash
ros2 run demo_nodes_cpp talker
```

In another terminal source the setup file and then run a Python `listener`:
在另一个终端中运行 setup 文件, 然后运行 ​​Python `listener`:

```bash
source /opt/ros/lyrical/setup.bash
ros2 run demo_nodes_py listener
```

You should see the `talker` saying that it’s `Publishing` messages and the `listener` saying `I heard` those messages. This verifies both the C++ and Python APIs are working properly. Hooray!
你应该看到 `talker` 显示正在 `Publishing` 消息, `listener` 显示 `I heard` 这些消息. 这验证了 C++ 和 Python API 都正常工作. 太棒了!

If you want to use other RMW implementations, you can check the [guide](https://docs.ros.org/en/lyrical/Installation/RMW-Implementations.html).
如果您想使用其他 RMW 实现, 可以查看[指南](https://docs.ros.org/en/lyrical/Installation/RMW-Implementations.html).

## Next steps

Continue with the [tutorials and demos](https://docs.ros.org/en/lyrical/Tutorials.html) to configure your environment, create your own workspace and packages, and learn ROS 2 core concepts.
继续学习[教程和演示](https://docs.ros.org/en/lyrical/Tutorials.html), 配置您的环境, 创建您自己的工作区和软件包, 并学习 ROS 2 核心概念.

## Troubleshoot
故障排除

Troubleshooting techniques can be found [here](https://docs.ros.org/en/lyrical/How-To-Guides/Installation-Troubleshooting.html).
故障排除技巧请点击[此处](https://docs.ros.org/en/lyrical/How-To-Guides/Installation-Troubleshooting.html)查看.

## Uninstall

If you need to uninstall ROS 2 or switch to a source-based install once you have already installed from binaries, run the following command:
如果您需要卸载 ROS 2 或在已通过二进制文件安装后切换到基于源代码的安装方式, 请运行以下命令:

```bash
sudo apt remove '~nros-lyrical-*' && sudo apt autoremove
```

You may also want to remove the repository:
您可能还需要删除该存储库:

```bash
sudo apt remove ros2-apt-source
sudo apt update
sudo apt autoremove
sudo apt upgrade # Consider upgrading for packages previously shadowed.
```
