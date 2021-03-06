---
title: Ubuntu zsh配置
date: 2017-10-24 10:46:08
tags: [Ubuntu,vim]
copyright: true
categories: "Ubuntu"
---


# 1.安装
>sudo apt-get install zsh

# 2.配置
>wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
zsh

# 3.zsh配置文件
```
#color{{{
autoload colors zsh/terminfo
if [[ "$terminfo[colors]" -ge 8 ]]; then
colors
fi
for color in RED GREEN YELLOW BLUE MAGENTA CYAN WHITE; do
eval _$color='%{$terminfo[bold]$fg[${(L)color}]%}'
eval $color='%{$fg[${(L)color}]%}'
(( count = $count + 1 ))
done
FINISH="%{$terminfo[sgr0]%}"
# }}}

# 命令提示符 {{{
precmd () {
local count_db_wth_char=${# ${${(%):-%/}//[[:ascii:]]/}}
local leftsize=${# ${(%):-%M%/}}+$count_db_wth_char
local rightsize=${# ${(%):-%D %T }}
HBAR=" -"

FILLBAR="\${(l.(($COLUMNS - ($leftsize + $rightsize +2)))..${HBAR}.)}"

RPROMPT=$(echo "%(?..$RED%?$FINISH)")
PROMPT=$(echo "$BLUE%M$GREEN%/ $WHITE${(e)FILLBAR} $MAGENTA%D %T$FINISH
$CYAN%n $_YELLOW>>>$FINISH ")

# 在 Emacs终端 中使用 Zsh 的一些设置
if [[ "$TERM" == "dumb" ]]; then
setopt No_zle
PROMPT='%n@%M %/
>>'
alias ls='ls -F'
fi
}
# }}}

# 标题栏、任务栏样式{{{
case $TERM in (*xterm*|*rxvt*|(dt|k|E)term)
preexec () { print -Pn "\e]0;%n@%M//%/\ $1\a" }
;;
esac
# }}}

# 关于历史纪录的配置 {{{
# 历史纪录条目数量
export HISTSIZE=1000
# 注销后保存的历史纪录条目数量
export SAVEHIST=1000
# 历史纪录文件
export HISTFILE=~/.zhistory
# 以附加的方式写入历史纪录
setopt INC_APPEND_HISTORY
# 如果连续输入的命令相同，历史纪录中只保留一个
setopt HIST_IGNORE_DUPS
# 为历史纪录中的命令添加时间戳
setopt EXTENDED_HISTORY

# 启用 cd 命令的历史纪录，cd -[TAB]进入历史路径
setopt AUTO_PUSHD
# 相同的历史路径只保留一个
setopt PUSHD_IGNORE_DUPS

# 在命令前添加空格，不将此命令添加到纪录文件中
# setopt HIST_IGNORE_SPACE
# }}}


# 杂项 {{{
# 允许在交互模式中使用注释 例如：
# cmd # 这是注释
setopt INTERACTIVE_COMMENTS

# 启用自动 cd，输入目录名回车进入目录
# 稍微有点混乱，不如 cd 补全实用
# setopt AUTO_CD

# 扩展路径
# /v/c/p/p => /var/cache/pacman/pkg
setopt complete_in_word

# 禁用 core dumps
limit coredumpsize 0

# Emacs风格 键绑定
bindkey -e
# 设置 [DEL]键 为向后删除
bindkey "\e[3~" delete-char

# 以下字符视为单词的一部分
WORDCHARS='*?_-[]~=&;!# $%^(){}<>'
# }}}

# 自动补全功能 {{{
setopt AUTO_LIST
setopt AUTO_MENU
# 开启此选项，补全时会直接选中菜单项
# setopt MENU_COMPLETE

autoload -U compinit
compinit

# 自动补全缓存
# zstyle ':completion::complete:*' use-cache on
# zstyle ':completion::complete:*' cache-path .zcache
# zstyle ':completion:*:cd:*' ignore-parents parent pwd

# 自动补全选项
zstyle ':completion:*' verbose yes
zstyle ':completion:*' menu select
zstyle ':completion:*:*:default' force-list always
zstyle ':completion:*' select-prompt '%SSelect: lines: %L matches: %M [%p]'

zstyle ':completion:*:match:*' original only
zstyle ':completion::prefix-1:*' completer _complete
zstyle ':completion:predict:*' completer _complete
zstyle ':completion:incremental:*' completer _complete _correct
zstyle ':completion:*' completer _complete _prefix _correct _prefix _match _approximate

# 路径补全
zstyle ':completion:*' expand 'yes'
zstyle ':completion:*' squeeze-slashes 'yes'
zstyle ':completion::complete:*' '\\'

# 彩色补全菜单
eval $(dircolors -b)
export ZLSCOLORS="${LS_COLORS}"
zmodload zsh/complist
zstyle ':completion:*' list-colors ${(s.:.)LS_COLORS}
zstyle ':completion:*:*:kill:*:processes' list-colors '=(# b) # ([0-9]# )*=0=01;31'

# 修正大小写
zstyle ':completion:*' matcher-list '' 'm:{a-zA-Z}={A-Za-z}'
# 错误校正
zstyle ':completion:*' completer _complete _match _approximate
zstyle ':completion:*:match:*' original only
zstyle ':completion:*:approximate:*' max-errors 1 numeric

# kill 命令补全
compdef pkill=killall
zstyle ':completion:*:*:kill:*' menu yes select
zstyle ':completion:*:*:*:*:processes' force-list always
zstyle ':completion:*:processes' command 'ps -au$USER'

# 补全类型提示分组
zstyle ':completion:*:matches' group 'yes'
zstyle ':completion:*' group-name ''
zstyle ':completion:*:options' description 'yes'
zstyle ':completion:*:options' auto-description '%d'
zstyle ':completion:*:descriptions' format $'\e[01;33m -- %d --\e[0m'
zstyle ':completion:*:messages' format $'\e[01;35m -- %d --\e[0m'
zstyle ':completion:*:warnings' format $'\e[01;31m -- No Matches Found --\e[0m'
zstyle ':completion:*:corrections' format $'\e[01;32m -- %d (errors: %e) --\e[0m'

#  cd ~ 补全顺序
zstyle ':completion:*:-tilde-:*' group-order 'named-directories' 'path-directories' 'users' 'expand'
# }}}

# # 行编辑高亮模式 {{{
#  Ctrl+@ 设置标记，标记和光标点之间为 region
zle_highlight=(region:bg=magenta # 选中区域
special:bold # 特殊字符
isearch:underline)# 搜索时使用的关键字
# }}}

# # 空行(光标在行首)补全 "cd " {{{
user-complete(){
case $BUFFER in
"" ) #  空行填入 "cd "
BUFFER="cd "
zle end-of-line
zle expand-or-complete
;;
"cd " ) #  TAB + 空格 替换为 "cd ~"
BUFFER="cd ~"
zle end-of-line
zle expand-or-complete
;;
" " )
BUFFER="!?"
zle end-of-line
;;
"cd --" ) #  "cd --" 替换为 "cd +"
BUFFER="cd +"
zle end-of-line
zle expand-or-complete
;;
"cd +-" ) #  "cd +-" 替换为 "cd -"
BUFFER="cd -"
zle end-of-line
zle expand-or-complete
;;
* )
zle expand-or-complete
;;
esac
}
zle -N user-complete
bindkey "\t" user-complete

# 显示 path-directories ，避免候选项唯一时直接选中
cdpath="/home"
# }}}

# # 在命令前插入 sudo {{{
# 定义功能
sudo-command-line() {
[[ -z $BUFFER ]] && zle up-history
[[ $BUFFER != sudo\ * ]] && BUFFER="sudo $BUFFER"
zle end-of-line # 光标移动到行末
}
zle -N sudo-command-line
# 定义快捷键为： [Esc] [Esc]
bindkey "\e\e" sudo-command-line
# }}}

# 命令别名 {{{

alias -g ls='ls -F --color=auto'
alias -g ll='ls -l'
alias -g la='ls -a'
alias -g l='ls'
alias -g grep='grep --color=auto'
# alias -g history='history -fi'
alias -g ai='sudo apt-get install'
alias -g aar='sudo apt-get autoremove'
alias -g ap='sudo apt-get purge'
alias -g aud='sudo apt-get update'
alias -g aug='sudo apt-get upgrade'
alias -g adu='sudo apt-get dist-upgrade'

# [Esc][h] man 当前命令时，显示简短说明
alias run-help >&/dev/null && unalias run-help
autoload run-help

# 历史命令 top10
# alias top10='print -l ${(o)history%% *} | uniq -c | sort -nr | head -n 10'
# }}}

# 路径别名 {{{
# 进入相应的路径时只要 cd ~xxx
hash -d HIST="$HISTDIR"
# }}}

# {{{自定义补全
# 补全 ping
zstyle ':completion:*:ping:*' hosts g.cn facebook.com
# 补全 ssh scp sftp 等
my_accounts=(
{ly50247,osily,lg50247,root}@{192.168.1.1,192.168.0.1}
osily@localhost
)
zstyle ':completion:*:my-accounts' users-hosts $my_accounts

# def pacman-color completion as pacman
# compdef pacman-color=pacman
# }}}

# {{{ F1 计算器
arith-eval-echo() {
LBUFFER="${LBUFFER}echo \$(( "
RBUFFER=" ))$RBUFFER"
}
zle -N arith-eval-echo
bindkey "^[[11~" arith-eval-echo
# }}}

# # # # {{{
# function timeconv { date -d @$1 +"%Y-%m-%d %T" }

#  }}}

# # # # {{{
function command_not_found_handler() {
python /usr/lib/command-not-found $1
return 0
}
#  }}}

# #  END OF FILE # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #################
# vim:filetype=zsh foldmethod=marker autoindent expandtab shiftwidth=4


export http_proxy=http://192.168.187.145:80
#export JAVA_HOME="/usr/lib/jvm/java-6-sun"
#export JRE_HOME="/usr/lib/jvm/java-6-sun/jre"
export PATH="$PATH:/home/osily/program/bin"
#exportCLASSPATH="$CLASSPATH:.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:/home/osily/program/tomcat620/lib:/home/osily/program/tomcat620/lib/servlet-api.jar"
alias upg="sudo apt-get update && sudo apt-get upgrade"
alias qq="nohup google-chrome --no-proxy-server --app=http://web.qq.com >/dev/null 2>/dev/null &"
#alias tomstart="sudo ~/program/tomcat620/bin/startup.sh"
#alias tomshut="sudo ~/program/tomcat620/bin/shutdown.sh"
#alias js2="rhino"
alias gmusic="google-chrome --no-proxy-server --app=http://g.top100.cn/12174704/html/player.html#loaded"
alias apa="dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P"

```