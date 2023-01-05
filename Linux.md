# Linux Maintaining

Linux 系统维护使用及常见问题辑录

[TOC]

---

## 虚拟终端

`ctrl+alt+[F2~F6]`  
进入虚拟终端

`ctrl+alt+F1`  
返回图形界面

## root 权限

```
sudo su root
[password]:
```

进入 root 权限

`exit`  
退出root

`sudo passwd root`  
修改密码

**注：一般情况下不要在 root 权限下安装软件，因为会安装到`/`下而非`/home`下，如果需要权限可以添加`sudo`，输入密码后短时间内无需重复输入**

## ZSH

zsh 是一款高效好用的 shell，与 bash 同类；一般配合 oh-my-zsh 插件使用

- 安装
  `echo $SHELL`  
  查看当前使用的 shell

  `cat /etc/shells`  
  查看当前安装的 shells

  `sudo apt install zsh`  
  安装 zsh

  `chsh -s /bin/zsh`  
  更换默认 shell

  使用插件`./Collections/zsh_install.sh`执行 `sudo sh zsh_install.sh` 或直接 `git clone` 安装 oh-my-zsh

- 插件与主题

  在`~/.oh-my-zsh/plugins`目录下下载插件，此处推荐插件

  >zsh-autosuggestions
  >
  >zsh-syntax-highlighting

  `vim ～/.zshrc`

  在`plugins=(git)`中添加所需插件名

  在`ZSH_THEME=`后修改主题，此处推荐 [powerlevel10k](https://hub.fastgit.org/romkatv/powerlevel10k)

  `source ~/.zshrc` 应用配置

  ***装完之后千万千万千万不要用`exit`退出终端！！！***

## zsh-autosuggestions 重启丢失记忆

`sudo chmod 777 ~/.zsh_history`

## SSH 是密钥

- 下载与配置

  `sudo apt install openssh-server`

  `ssh-keygen`  
  生成 ssh 密钥

- 获取 ssh 密钥

  `cat ~/.ssh/id_rsa.pub`

## 中文字体

Ubuntu 自带的中文字体不太正常，具体可以以“门”“复”等字试验。以下方法转换正常的简体中文

`sudo vim /etc/fonts/conf.d/64-language-selector-prefer.conf`  
打开字体配置文件  
将三处含`SC`（简体中文）的行剪切到序列最上  
保存退出，重启

## 华为安装 Ubuntu 提示

开机过程中按（Fn+）F2 进入 BIOS， 设置`HDD Windows ...`为Enable，`secure boot`为 Enable。安装完后不用修改回去  
挂在分配完后一定选择默认在`Ubuntu efi`位置启动

## Ubuntu 与 Windows 系统时间不一致

`timedatectl set-local-rtc 1 --adjust-system-clock`，设置正确时间并重启

## 开机引导倒计时

`sudo vim /etc/default/grub`  
修改`GRUB_TIMEOUT`  
`sudo update-grub`  
若要将引导时间设为 0，还需`sudo vim /etc/grub.d/30_os-prober`，找到`timeout`变量，注释其检查逻辑

## 关机过慢

`sudo vim /etc/systemd/system.comf`
将 `#DefaultTimeoutStopSec=90s` 改为 `DefaultTimeoutStopSec=5s`

## QQ 闪退

原因是缓存过多，执行如下命令清除QQ缓存  
`rm -r ~/.config/tencent-qq`

## Gnome Tweak Tools

用于 Gnome 桌面自定义设置

```
sudo add-apt-repository ppa:ricotz/testing
sudo apt-get update
sudo apt-get install gnome-tweak-tool
```

[Gnome 主题](http://gnome-look.org/)
[参考教程](https://www.cnblogs.com/yaoxing/archive/2013/02/11/customize-gnome.html)

---

## *奇怪的技巧合集*

- 光标选中一段代码后在另一处按下鼠标中键可以快速复制粘贴（但是好像有最大字符数限制）
- 如果 github 的 clone 速度过慢可以先在 gitee 中创建新仓库并 import github的仓库地址，然后从 gitee clone
- [github 镜像站](hub.fastgit.org)

---

&copy; 2020 NWPU WMJ Vision group. All rights reserved.