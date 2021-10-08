## 让arch更好用的方法

### 1. 设置触摸板

系统设置

<img src="https://gitee.com/Fantastic-Feng/picgo/raw/master/20210816212903.png" alt="image-20210816212902130" style="zoom: 67%;" />

安装额外的包设置触摸板手势

```shekll
yay -S libinput-gestures
```

编辑~/.config/libinput-gestures.conf ,自定义手势支持滑、涅，以及手指数量。比如swipe up就是上滑，swipe up 3就是三指上滑

配置为

```shell
# 2指捏: 缩小
gesture pinch in 2 xdotool key ctrl+minus
# 2指张: 放大
gesture pinch out 2 xdotool key ctrl+plus

# 3指捏: 多桌面切换任务
# gesture pinch in 3 xdotool key ctrl+super+d
# 3指张: 多桌面切换任务
# gesture pinch in 3 xdotool key ctrl+super+d

# 3指上划: 多桌面切换任务
gesture swipe up 3 xdotool key ctrl+super+d
# 3指下划: 多桌面切换任务
gesture swipe down 3 xdotool key ctrl+super+d

# 4指左划: 切换到左侧桌面
gesture swipe left 4 xdotool key super+ctrl+Right
# 4指右划: 切换到右侧桌面
gesture swipe right 4 xdotool key super+ctrl+Left

# 4指上划: 所有窗口
gesture swipe up 4 xdotool key ctrl+F10
# 4指下划: 桌面
gesture swipe down 4 xdotool key super+d

```

命令行启动，重启，自动启动 设置

```shell
libinput-gestures-setup start|stop|restart|autostart|autostop|status
```

### 2. 主题推荐

前往网页下载速度非常快,需要先安装cur-url包

```shell
yay -S ocs-url
```

安装kvantum;

```shell
sudo pacman -S kvantum-qt5
```

其中在125%的缩放条件下的dolphin的透明效果看脸，在使用核显和自动选择显卡时更是如此

因为kde 125%的缩放有未知bug，所以可以在100%缩放条件下调整字体dpi和图标大小

字体dpi，默认为96，调整为120，图表大小全部增大一级，效果与全局缩放125%近似

![20210821110327](https://cdn.jsdelivr.net/gh/Fantastic-Feng/picture@main/img/20210821110327.png)

- 深色主题：

应用程序风格：kvantum 使用layan

plasma样式：layan

窗口装饰元素：Sweet-Dark-transparent  Arc-OSX-Black-Transparent(调小边框)

颜色: layan

字体: harmonyos-sans-git

图标: Fluent green ; Tela blue; Paprius

光标:  whitesur Cursors

konsole: Moe-dark

效果图：

<img src="https://cdn.jsdelivr.net/gh/Fantastic-Feng/picture@main/img/20210817193908.png" alt="20210817193908" style="zoom: 50%;" />




应用程序风格：kvantum 使用 Fluent-light

plasma样式：Arch

窗口装饰元素：Arc OSX White Transparent (调小边框)  Breezemite

颜色: WhiteSur

字体: harmonyos-sans-git

图标: Fluent green ; Tela blue; Paprius

光标:  whitesur Cursors

konsole: Moe

效果图:

<img src="https://gitee.com/Fantastic-Feng/picgo/raw/master/20210817192119.png" alt="image-20210817192117878" style="zoom:50%;" />

### 3. 包管理系统的常用命令

1. pacman

```shell
sudo pacman -S package_name        # 安装软件包
sudo pacman -Syyu                  # 升级系统 yy标记强制刷新 u标记升级动作
sudo pacman -R package_name        # 删除软件包，保留依赖关系
sudo pacman -Rs package_name       # 删除软件包，及其所有没有被其他已安装软件包使用的依赖包
sudo pacman -Qdt                   # 找出孤立包 Q为查询本地软件包数据库 d标记依赖包 t标记不需要的包 dt合并标记孤立包
sudo pacman -Rs $(pacman -Qtdq)    # 删除孤立软件包 递归进行，小心使用
sudo pacman -Fy                    # 更新命令查询文件列表数据库
sudo pacman -F xxx                 # 当不知道某个命令属于哪个包时，用来查询某个xxx命令属于哪个包
sudo pacman -Ss package            # 包数据库中查询包 
sudo pacman -Q [package]           # 本地仓库包查询
sudo pacman -Qs [package]          # 已安装的包查询 
sudo pacman -U local_package_name  # 其扩展名为pkg.tar.gz或pkg.tar.xz
sudo pacman -U url                 # 不在 pacman 配置的源里面
sudo pacman -Sc                    # 清理软件包缓存
sudo pacman -Scc                   # 清理所有缓存
```

2. yay

```shell
yay -S package        # 从 AUR 安装软件包
yay -Rns package      # yay删除包
yay -Syu              # 升级所有已安装的包
yay -Ps               # 打印系统统计信息
yay -Qi package       # 检查安装的版本
```

### 4. 系统备份

1. #### 安装timeshift

```shell
yay -S timeshift
```

2. #### 拍摄快照 

   设置备份位置，和备份文件，备份完成后如图所示

<img src="https://gitee.com/Fantastic-Feng/picgo/raw/master/20210821012614.png" alt="image-20210821012613217" style="zoom:80%;" />

3. #### 恢复快照

   - 可进入桌面，直接在GUI界面点击恢复

   - 命令行恢复(系统崩溃,可进入命令行）

     一般系统崩溃后不能进入桌面，但是能够进入登录界面，现象就是在登录界面输入密码后不会进入桌面，那么就要通过命令行的方式进行还原。

     1. 通过Ctrl+Alt+F1（一般是F1-F6都可）进入tty终端

     2. 输入用户和密码登录

     3. 执行以下命令，查看快照列表

        ```shell
        sudo timeshift --list
        ```

        执行结果如下

        ![image-20210821013240434](https://gitee.com/Fantastic-Feng/picgo/raw/master/20210821013240.png)

        选择节点还原，在弹出选项中输入Enter ,y 即可

        ```shell
        sudo timeshift --restore --snapshot '2019-07-16_16-35-42' --skip-grub
        # –skip-grub 选项为跳过grub安装，一般来说grub不需要重新安装，除非bios启动无法找到正确的grub启动项，才需要安装
        ```

   - 无法进入系统，通过优盘进入live cd,安装timeshift,进行命令行恢复即可

4. #### 更新系统

   ```shell
   yay -Syu
   ```

   

