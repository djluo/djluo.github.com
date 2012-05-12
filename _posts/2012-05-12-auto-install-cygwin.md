---
layout: post
title: "auto install cygwin"
description: ""
category: Cygwin
tags: []
---
{% include JB/setup %}

### 自动安装/更新Cygwin

* 新建相应的目录``D:\cygwin`` ``D:\Downloads`` 
* 下载cygwin的安装程序 [setup.exe](http://cygwin.com/setup.exe) 至 ``D:\cygwin`` 下
* 保存下面的BAT代码至 ``D:\cygwin\auto-install.bat``
        cd /D D:\cygwin

        set OPT=--no-shortcuts --quiet-mode --download --local-install --package-manager
        set SITE=--only-site --site http://mirrors.163.com/cygwin/
        set DIR=--local-package-dir D:\Downloads\Cygwin --root d:\cygwin

        set PKG=p7zip,pbzip2,zip,unzip,gcc4,gcc4-g++,make
        set PKG=%PKG%,git,git-completion,git-svn
        set PKG=%PKG%,curl,lftp,netcat,openssh,rsync
        set PKG=%PKG%,gperf,wget,libcharset1,libiconv
        set PKG=%PKG%,perl,python,ruby,bc,autossh,bind
        set PKG=%PKG%,whois,bash-completion,ping,screen
        set PKG=%PKG%,autoconf,automake,autopoint,bison
        set PKG=%PKG%,openssl,openssl-devel,ctags,mlcscope

        setup.exe %OPT% --packages %PKG% %DIR% %SITE%
* 双击执行``auto-install.bat``,一路Next就好, 因为软件包已自动选中了。
* 保存下面的代码至``D:\cygwin\init.sh`` 需要修改一下``USER``和``authorized_keys``：
{% highlight bash %}
#!/bin/sh

USER="username" # 你当前windows的账号
mkdir -p /home/${USER}/.ssh
cat<<EOF >/home/${USER}/.ssh/authorized_keys
ssh-rsa AAAAB3N...... # 你的公钥
EOF

cygserver-config -y
ssh-host-config -y

cp /etc/sshd_config{,bak}
cat<<EOF >/etc/sshd_config
Port 223
Protocol 2
ListenAddress 127.0.0.1
PermitRootLogin no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
UsePAM no
UseDNS no
X11Forwarding no
SyslogFacility AUTHPRIV
GSSAPIAuthentication no
GSSAPICleanupCredentials yes
Subsystem   sftp    /usr/sbin/sftp-server
EOF
cat<<\EOF >/etc/ssh_config
Host *
        Port 223
        ForwardAgent yes
EOF

cygrunsrv -S sshd

cat<<\EOF >/home/${USER}/.bashrc
[[ "$-" != *i* ]] && return
[[ -f /etc/bash_completion ]] && . /etc/bash_completion
shopt -s histappend
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias grep='grep --color'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias ls='ls -h --file-type --color=tty --time-style=long-iso'
alias ll='ls -l'
alias la='ls -A'
alias l='ls -C --file-type'
HISTFILESIZE=50000
HISTSIZE=10000
HISTTIMEFORMAT="<%F %T>: "
PATH="/bin:/usr/bin:/sbin:$PATH"
export HISTTIMEFORMAT HISTSIZE HISTFILESIZE PATH
PS1='\[\e]0;\w\a\]\[\e[32m\]\t@\h \[\e[33m\]\W\[\e[0m\]\$ '
EOF
sed -i '/^PS1=/d' /etc/bash.bashrc
{% endhighlight %}
* 双击执行``D:\cygwin\Cygwin.bat`` 会弹出cmd窗口,输入 ``cd /;sh init.sh`` 完成sshd配置。
* 配置Xshell,使用IP 127.0.0.1,端口223,认证方式 Public Key ,即可登录Cygwin
* 相关解释:
    1. cmd窗口太丑陋了,建议使用sshd + Xshell终端,能享用tab补齐,ssh-agent等等。 
    2. 建议使用 [Xshell](http://www.netsarang.com/products/xsh_overview.html), 虽然SecureCRT也可以,不过这是收费软件,强烈建议不要用盗版的。
    3. 如需更新，再次双击执行``auto-install.bat`` 并一路Next即可。

<!--
 vim: expandtab tabstop=4 shiftwidth=4 softtabstop=4
-->
