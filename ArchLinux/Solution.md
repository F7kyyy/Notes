## 主要记录一些使用arch过程中的遇到配置环境问题的解决方法

### 1. opengl

#### 配置task.json

系统本身就安装了最新的GCC,不需要安装g++等

arch自带的有opengl,glut,glfw等包，没必要再装freeglut

在vscode的task.json中添加参数即可

```
"-lGL",
"-lGLU",
"-lglut"
```

类似于

![image-20210810231448877](https://gitee.com/Fantastic-Feng/picgo/raw/master/20210810231459.png)

#### 使用cmake

添加cmakeList.txt,内容如下

```cmake
cmake_minimum_required(VERSION 3.14)
project(CG)

find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIR})

find_package(GLUT REQUIRED)
include_directories(${GLUT_INCLUDE_DIR})

set(CMAKE_CXX_STANDARD 14)

add_executable(CG arcball.h arcball.cpp main.cpp)
target_link_libraries(CG ${GLUT_LIBRARY} ${OPENGL_LIBRARY})
```

### 2. miniconda的安装与配置

#### 安装

可以直接在terminal中使用yay安装,按照提示操作添加环境变量 如果是zsh可以添加到.zshrc中

```shell
yay -S miniconda3
```

中

#### 修改镜像源

第一种方法

```shell
#添加清华大学镜像源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --set show_channel_urls yes
#添加第三方源
Conda Forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

msys2
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/

bioconda
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/

menpo
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/

pytorch
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/


```

第二种方法

执行 conda config,在/home/$username$/目录下产生.condarc文件编辑文件

```shell
channels:
  - defaults
show_channel_urls: true
channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
#执行清除缓存
conda clean -i 
```

#### 基本操作

创建虚拟环境

```shell
#tensorflow 改为虚拟环境名字，python版本自己设置
conda create --name tensorflow python=3.7
```

激活与退出

```shell
conda activate "虚拟环境名"
conda deactivate 
```

查看已有环境的包

```shell
conda list
```

删除虚拟环境

```shell
conda remove -n "虚拟环境名" --all
```

查看所有虚拟环境名

```shell
conda-env list
```

#### 安装pytorch

创建虚拟环境

````shell
conda create --name pytorch python=3.7
````

进入pytorch官网 https://pytorch.org/

选择想要的版本

![image-20210810231540098](https://gitee.com/Fantastic-Feng/picgo/raw/master/20210810231540.png)

激活虚拟环境

```shell
conda activate pytorch 
```

安装

```shell
# -c pytorch指向pytorch官网，配置过conda镜像就不需要加pytorch ,-c nvidia指向NVIDIA官网
# 安装11.1需要加上nvidia 10.2不需要，无论是pytorch官网还是nvidia官网都是需要魔法上网的否则几乎不可能下载成功
conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch -c nvidia
```

#### 安装tensorflow

cuda python tensorflow版本依赖关系

![image-20210819025650588](https://gitee.com/Fantastic-Feng/picgo/raw/master/20210819025651.png)

1. 创建虚拟环境

   ```shell
   conda create --name tensorflow python=3.8
   conda activate tensorflow
   ```

2. 安装conda-cuda

   ```shell
   conda install cudatoolkit=11.0 
   conda install cudnn=8.0
   ```

3. 安装tensorflow，清华源中没有tensorflow-gpu 2.4.0版本，所以从pip安装

   ```shell
    pip install -i https://pypi.doubanio.com/simple/ tensorflow-gpu==2.4.0
   ```

   

### 3. typora 使用gitte图云

#### 1. 安装node依赖

```shell
# 安装node
yay -S node
# 安装npm
yay -S npm
```

查看安装情况

![image-20210810235348419](https://gitee.com/Fantastic-Feng/picgo/raw/master/20210810235348.png)

#### 2. 安装picgo及必要插件

```shell
1. 使用npm安装picgo
sudo npm install picgo -g
#2. 安装插件
# 上传gitee插件
picgo install gitee-uploader
# 时间戳重名
install super-prefix
```

#### 3. 配置picgo config

打开 ~/.picgo/config.json文件

粘贴一下配置

```shell
{
  "picBed": {
    "uploader": "gitee",
    "gitee": {
      "owner": "Fantastic-Feng",
      "repo": "Fantastic-Feng/picgo",
      "token": "1658a5e6c966bd9e89c0e4303b535360",
      "path": "",
      "customUrl": "",
      "branch": "master"
    }
  },
  "picgoPlugins": {
    "picgo-plugin-gitee-uploader": true,
    "picgo-plugin-super-prefix": true
  },
  "picgo-plugin-super-prefix": {
    "fileFormat": "YYYYMMDDHHmmss"
  },
}
```

token:

- Github

  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

  cdn: https://cdn.jsdelivr.net/gh/ Fantastic-Feng/picture@main

- gitee

  1658a5e6c966bd9e89c0e4303b535360

#### 4.  配置Typora

查询node.picgo位置

![image-20210811000044353](https://gitee.com/Fantastic-Feng/picgo/raw/master/20210811000044.png)

typora 文件->偏好设置->图像 按以下方案进行配置

![image-20210811000229243](https://gitee.com/Fantastic-Feng/picgo/raw/master/20210811000229.png)

### 4. 配置vscode 

#### 1. 配置默认终端

添加终端配置，需要注意的是下方的配置是在windows下的，在linux下需要将windows变为linux，并且shell位置要设置为自己的zsh,bash的位置linux下一般在 "/usr/bin/"目录下

"terminal.integrated.profiles.windows"->"terminal.integrated.profiles.linux"

```shell
 "terminal.integrated.profiles.windows": {
        "PowerShell-7": {
            "path": "C:\\Program Files\\Environment\\PowerShell\\7\\pwsh.exe",
            "color":"terminal.ansiMagenta",
            "icon": "terminal",
            "args": [
                "-NoLogo"
            ]
        },
        "PowerShell": {
            "source": "PowerShell",
            "color": "terminal.ansiBlue",
            "args": [
                "-NoLogo"
            ]
        },
        "Git-Bash": {
            "path": "C:\\Program Files\\Environment\\Git\\bin\\bash.exe",
            "color":"terminal.ansiYellow",
            "icon": "terminal-bash",
        }
    },
```

选择默认终端

```shell
"terminal.integrated.defaultProfile.windows": "Git-Bash",
```

控制终端光标的样式

```shell
"terminal.integrated.cursorStyle": "line",
```

