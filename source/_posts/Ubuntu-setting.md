---
title: Ubuntu必备软件及其设置
date: 2017-10-24 08:29:54
tags: [Ubuntu,Software]
copyright: true
categories: "Ubuntu"
---


#### 设置启动器放在底部
>gsettings set com.canonical.Unity.Launcher launcher-position Bottom

#### 删除LibreOffice
>sudo apt-get remove LibreOffice* 

#### 删除Amazon的图标
>sudo apt-get remove unity-webapps-common

#### 删除不必要的软件

>sudo apt-get remove unity-webapps-common   
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot  
sudo apt-get remove gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install   
sudo apt-get remove rhythmbox evolution bittorrent empathy    
sudo apt-get remove onboard deja-dup 

#### 更新源，并安装更新

>sudo apt-get update
sudo apt-get upgrade  
sudo apt-get autoremove 


#### 安装Unity Tweak Tool
>sudo apt-get install unity-tweak-tool  gnome-tweak-tool

#### 安装aptitude,使得自动解决依赖问题
>sudo apt-get install aptitude shutter  flashplugin-installer 

####github程序猿必备。
>sudo apt-get install git

#### ubuntu自带的字体不太好看，所以采用文泉译微米黑字体替代，效果会比较好
>sudo apt-get install fonts-wqy-microhei

#### 安装Numix主题

>sudo add-apt-repository ppa:numix/ppa
sudo apt-get update
sudo apt-get install numix-gtk-theme  numix-icon-theme-circle


#### 安装 Slingscold
>sudo add-apt-repository ppa:noobslab/macbuntu
sudo apt-get update
sudo apt-get install slingscold  albert

#### 开启WiFi(kde5-nm-connection-editor在plasma-nm包内） 
`sudo apt-get install plasma-nm`   

#### 安装arc-theme
>sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update 
sudo apt-get install arc-theme
  

#### Flatabulous主题是一款ubuntu下扁平化主题，执行以下命令安装Flatabulous主题：

>sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install flatabulous-theme

  
#### 该主题有配套的图标，安装方式如下：

>sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ultra-flat-icons


#### 安装GTK主题

>sudo add-apt-repository ppa:numix/ppa
sudo apt-get update && sudo apt-get install numix-gtk-theme


#### 安装conky，curl系统监控软件

>sudo apt-get install conky conky-all   curl


#### 安装压缩类软件  

>sudo apt-get install unace unrar zip unzip p7zip-full p7zip-rar sharutils rar 

 
#### 安装互联网常用工具  
>sudo apt-get install filezilla  iptux 


 
#### 安装系统工具  

>sudo apt-get install  yakuake htop lrzsz sysstat sshpass curl wget nmap nload tree lynx iptraf


#### 最喜欢的图标主题 Faenza 1.3

>sudo add-apt-repository ppa:tiheum/equinox
sudo apt-get update 
 sudo apt-get install faenza-icon-theme


#### square 图标主题(非必须）

>sudo add-apt-repository ppa:noobslab/icons2
sudo apt-get update
sudo apt-get install square-icons


#### 终端采用zsh和oh-my-zsh，既美观又简单易用，主要是能提高你的逼格

>sudo apt-get install zsh  
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

#### 启动器上苹果Logo

>wget -O launcher_bfb.png http://drive.noobslab.com/data/Mac/launcher-logo/apple/launcher_bfb.png
sudo mv launcher_bfb.png /usr/share/unity/icons/


#### 恢复默认

>wget -O launcher_bfb.png http://drive.noobslab.com/data/Mac/launcher-logo/ubuntu/launcher_bfb.png
mv launcher_bfb.png /usr/share/unity/icons/


#### 配置 Mac 字体：

>wget -O mac-fonts.zip http://drive.noobslab.com/data/Mac/macfonts.zip
sudo unzip mac-fonts.zip -d /usr/share/fonts; rm mac-fonts.zip
sudo fc-cache -f -v


#### 安装 MacBuntu OS Y Theme、Icons 和 cursors
>sudo add-apt-repository ppa:noobslab/macbuntu
sudo apt-get update
sudo apt-get install macbuntu-os-icons-lts-v7   
sudo apt-get install macbuntu-os-ithemes-lts-v7
