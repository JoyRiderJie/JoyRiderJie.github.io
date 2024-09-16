在Ubuntu中创建一个shell脚本，用于实时监控某些目录下的文件更改，并将更新后的文件上传到服务器。

## 安装必要的工具

首先，确保你的系统中安装了以下工具：

- `inotify-tools`：用于监控文件系统事件。
- `bypy` 或 `git`：用于上传文件到服务器。

安装命令示例

```bash
# 安装 inotify-tools
sudo apt install inotify-tools

# 安装 bypy（如果它在官方仓库中不可用，可能需要使用 pip 或其他方式安装）
sudo apt install bypy  # 这可能不会工作，因为 bypy 可能不在官方仓库中

# 安装 git
sudo apt install git
```

如果 `bypy` 不在你的系统的官方仓库中，你可能需要使用 Python 的包管理器 `pip` 来安装它。以下是使用 `pip` 安装 `bypy` 的命令：

```bash
# 确保 pip 是最新的
sudo apt install python3-pip

# 使用 pip 安装 bypy
pip3 install bypy
```

## 编写脚本

### 使用git实现

下面是一个简单的示例脚本，它使用`inotify-tools`来监控当前目录下的文件更改，并使用`git`命令来上传更新后的文件：

```bash
#!/bin/bash

# 监控的目录
WATCHED_DIR=$(pwd)

# Git仓库的远程地址
REMOTE_REPO="your_remote_repo_url"

# 监控文件更改
inotifywait -m -r -e modify -e move -e create -e delete --format '%w%f' "$WATCHED_DIR" | while read FILE
do
  echo "Detected change in $FILE"

  # 添加更改到Git
  git add "$FILE"

  # 提交更改
  git commit -m "Auto-update: $FILE"

  # 推送到远程仓库
  git push "$REMOTE_REPO" master

  echo "Uploaded $FILE to $REMOTE_REPO"
done
```

这种方法可以实现检测与上传，但是需要在git的配置文件中保存自己的账号与密码。否则每次提交时，命令行会要求输入密码：

```bash
Username for 'https://gitee.com': 123@test.com
Password for 'https://123@test.com@gitee.com': 
```

如果不想保存或输入密码也可以使用个人访问令牌（Personal Access Token, PAT），确保个人访问令牌具有推送代码的权限。

### 使用bypy实现

使用脚本监控 `./results` 和 `./runs_new` 两个目录，并上传到百度云的 `New_data/Adversarial_camou` 目录，脚本为

1. **定义监控目录**：监控 `./results` 和 `./runs_new` 两个目录。
2. **定义百度云目标目录**：上传文件到百度云的 `New_data/Adversarial_camou` 目录。
3. **监控文件更改**：使用 `inotifywait` 命令监控文件的修改、移动、创建和删除事件。
4. **构建百度云路径**：将本地文件路径转换为百度云中的相对路径。
5. **上传文件**：使用 `bypy upload` 命令上传文件到百度云。

```bash
#!/bin/bash

# 定义要监控的目录列表
WATCHED_DIRS=("./results" "./runs_new")

# 百度云的目标目录
BAIDU_YUN_DIR="New_data/Adversarial_camou"

# 监控文件更改
for DIR in "${WATCHED_DIRS[@]}"; do
  inotifywait -m -r -e modify -e move -e create -e delete --format '%w%f' "$DIR" | while read FILE
  do
    echo "Detected change in $FILE"

    # 构建百度云中的完整路径
    # 去除本地路径的开头部分，只保留相对路径
    LOCAL_RELATIVE_PATH="${FILE#$DIR/}"

    # 构建百度云中的完整路径
    CLOUD_PATH="${BAIDU_YUN_DIR}/$LOCAL_RELATIVE_PATH"

    # 使用bypy上传文件到百度云指定目录
    bypy upload "$FILE" "$CLOUD_PATH"

    echo "Uploaded $FILE to Baidu Yun directory: $CLOUD_PATH"
  done
done
```

## 运行和监控脚本

在运行脚本之前，确保给予它执行权限：

```bash
chmod +x your_script_name.sh
```

然后，可以在后台运行这个脚本：

```bash
./your_script_name.sh & # 命令后字符 & 表示这条命令会放入后台执行
```

这样，脚本就会开始监控当前目录下的文件更改，并将更新后的文件上传到服务器。

## 终止脚本运行

在Linux系统中，可以使用多种方法来终止或杀死进程。以下是一些常用的命令和方法：

1. **使用`kill`命令**：
   
   - 首先，需要找到进程的进程ID（PID）。可以使用`ps`命令或`top`命令来查找。
   - 然后，使用`kill`命令加上PID来发送信号终止进程。
     ```bash
     kill PID
     ```
   - 如果进程不响应普通的终止信号，可以尝试发送更强制的信号，如`SIGKILL`（信号值为9）。
     ```bash
     kill -9 PID
     ```
   
2. **使用`pkill`命令**：
   - 可以使用`pkill`命令根据进程名称或其他属性来杀死进程。
     ```bash
     pkill -f process_name
     ```
   - `-f`选项表示按照完整的命令行来匹配进程。

3. **使用`killall`命令**：
   - 类似于`pkill`，`killall`命令也可以根据进程名称来杀死进程。
     ```bash
     killall process_name
     ```

4. **使用`top`或`htop`命令**：
   - 在`top`或`htop`界面中，可以找到进程的PID，并发送信号来杀死进程。
   - 在`top`中，按`k`键，然后输入PID和信号值（默认是9）。
   - 在`htop`中，选中进程后按`F9`，然后选择信号值。

5. **使用`pgrep`和`kill`组合**：
   - 可以使用`pgrep`命令来查找特定进程的PID，然后使用`kill`命令来杀死进程。
     ```bash
     pgrep process_name
     kill -9 $(pgrep process_name)
     ```

6. **使用`systemctl`命令**：
   - 如果进程是一个系统服务，可以使用`systemctl`命令来停止服务。
     ```bash
     systemctl stop service_name
     ```

7. **使用`killtree`命令**：
   - `killtree`可以杀死一个进程及其所有子进程。
     ```bash
     killtree PID
     ```

请记住，在使用强制杀死进程的方法（如`kill -9`）之前，最好先尝试使用默认的终止信号（如`kill PID`），因为强制方法不会给进程清理资源的机会，可能会导致数据丢失或其他问题。

在使用这些命令时，确保有足够的权限来终止目标进程。如果没有，可能需要使用`sudo`来获取必要的权限。