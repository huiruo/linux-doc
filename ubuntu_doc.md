## 最开始的安装db和设置git
sudo apt install git 
git config --global user.name "chenhuiruo" 

git config --global user.email "chenhuiruo@sugo.io"
sudo dpkg -i xxx

### github 配置
```
ssh-keygen -t rsa -C '2196411859@qq.com' -f ~/.ssh/github_id_rsa
 
Your identification has been saved in /c/Users/Administrator/.ssh/github_id_rsa
Your public key has been saved in /c/Users/Administrator/.ssh/github_id_rsa.pub
 
ssh-add ~/.ssh/github_id_rsa 
```

### 安装 neovim
sudo apt install neovim
```
mkdir ~/.config/nvim
touch ~/.config/nvim/init.vim

目录：
/home/ruome/.config/nvim/init.vim

curl -fLo ~/.config/nvim/autoload/plug.vim https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

安装插件管理工具vim-plug:
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
:PlugInstall to install plugins.

插件位置：
/home/ruome/.vim/plugged
配置位置：
/home/ruome/.config/nvim/init.vim
```

## 卸载内核
```
查看自己已经安装的内核有那些
dpkg --get-selections | grep linux 

sudo apt remove linux-headers-5.13.0-23
sudo apt remove linux-headers-5.13.0-23-generic	

sudo apt remove linux-image-5.13.0-23-generic
sudo apt remove linux-modules-5.13.0-23-generic
sudo apt remove linux-modules-extra-5.13.0-23-generic

更新启动引导
sudo update-grub

处于deinstall状态，移除:
sudo dpkg -P linux-headers-5.13.0-23  
sudo dpkg -P linux-image-3.5.0-51-generic  
```

## vim 和剪切板
```
1、进入vim，进入底行模式输入:echo has('clipboard') 如果输出为“0”则需要安装vim-gnome，在终端中输入 sudo apt-get install vim-gnome
2、在终端输入sudo vim /etc/vim/vimrc编辑配置文件，在文件底部添加:set clipboard=unnamedplus保存退出
3、完成上面两部之后在命令模式下输入yy即可将vim下的内容复制到系统剪贴板
```

## 商店
```
sudo snap install snap-store

商店不能搜索：
一种解决方法是重新安装旧的Gnome软件，这是Ubuntu先前版本中的默认设置，并且具有完全相同的用户界面。
sudo apt install gnome-software
3.安装后，可通过软件更新程序或通过运行命令来刷新系统程序包缓存：
sudo apt update
```

## node 环境
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
当前是 16.x 的版本。 这里我使用了 LTS 版本。

也可以使用最新版本:
curl -fsSL https://deb.nodesource.com/setup_current.x | sudo -E bash -

可以默认同意安装加上 -y:
sudo apt install -y nodejs
### node 卸载
sudo apt remove --purge npm
sudo apt remove --purge nodejs
sudo apt remove --purge nodejs-legacy
sudo apt autoremove

### yarn
npm install -g yarn
sudo npm install -g umi
查看：umi -v

## ubuntu安装中州韵rime输入法
https://www.freesion.com/article/4710839210/
https://blog.csdn.net/qq_44700366/article/details/121446162
```
桌面找到 语言支持 或者 设置-区域与语言-管理已安装语言 中，键盘输入法系统框架为 IBus 。当然，rime也有fcitx框架的版本，但是已经停更，并且fcitx下，为什么不使用搜狗或者百度呢。
25.3.2 终端安装
桌面找到 语言支持 或者 设置-区域与语言-管理已安装语言 中，键盘输入法系统框架为 IBus 。当然，rime也有fcitx框架的版本，但是已经停更，并且fcitx下，为什么不使用搜狗或者百度呢。
25.3.2 终端安装
sudo apt-get install ibus-rime

2.打开设置，在终端中输入
ibus-setup

3.光标不跟随解决
在用Rime打字的时候发现输入框不跟随光标，这个是ibus的问题，是框架没装全，下面命令运行后重启
sudo apt-get install ibus-gtk ibus-gtk3

4.这几个配置有用：
cd /home/ruome/.config/ibus/rime
4.1增加配置文件default.custom.yaml
  配置小鹤双拼和简化字模式
patch:
    schema_list:
        - schema: luna_pinyin_simp # 简化字模式
        - schema: double_pinyin_flypy # 小鹤双拼  

  * 设置文字横排/home/user/.config/ibus/rime 下增加配置文件ibus_rime.yaml:
style:
    horizontal: true
```

sudo vi default.custom.yaml
设置后选词的个数
```
patch:
    menu:
        page_size: 9
switches: 
  - name: ascii_mode
    reset: 1 # 1为默认英文状态，0为默认中文状态
```

sudo vi ibus_rime.yaml
```
style:
    horizontal: true
```

sudo vi luna_pinyin.custom.yaml
```
patch:                      
  switches:                   # 注意縮進
    - name: ascii_mode
      reset: 0                # reset 0 的作用是當從其他輸入方案切換到本方案時，
      states: [ 中文, 西文 ]  # 重設爲指定的狀態，而不保留在前一個方案中設定的狀態。
    - name: full_shape        # 選擇輸入方案後通常需要立即輸入中文，故重設 ascii_mode = 0；
      states: [ 半角, 全角 ]  # 而全／半角則可沿用之前方案中的用法。
    - name: simplification
      reset: 0                # 增加這一行：默認啓用「繁→簡」轉換。
      states: [ 漢字, 汉字 ]
```

## markdown vim 配置
```
"安装插件
Plug 'godlygeek/tabular' "必要插件，安装在vim-markdown前面
Plug 'plasticboy/vim-markdown'

查看所有配置建议
:help vim-markdwon
禁用折叠：
let g:vim_markdown_folding_disabled = 1
[[ "跳转上一个标题
]] "跳转下一个标题
]c "跳转到当前标题
]u "跳转到副标题
zr "打开下一级折叠
zR "打开所有折叠
zm "折叠当前段落
zM "折叠所有段落
:Toc "显示目录
```

## 钉钉修改
```
/opt/apps/com.alibabainc.dingtalk/files/Elevator.sh #!/bin/sh 
export GTK_IM_MODULE="ibus" 
export QT_IM_MODULE="ibus"
export XMODIFIERS="@im=ibus" 
export QT_QPA_PLATFORM=xcb
```

## mysql 安装
通过apt 安装MySQL服务（会安装最新版）
log:
```text
done!
update-alternatives: 使用 /var/lib/mecab/dic/ipadic-utf8 来在自动模式中提供 /var
/lib/mecab/dic/debian (mecab-dictionary)
正在设置 mysql-server-8.0 (8.0.27-0ubuntu0.21.10.1) ...
update-alternatives: 使用 /etc/mysql/mysql.cnf 来在自动模式中提供 /etc/mysql/my.
cnf (my.cnf)
Renaming removed key_buffer and myisam-recover options (if present)
mysqld will log errors to /var/log/mysql/error.log
mysqld is running as pid 42590
Created symlink /etc/systemd/system/multi-user.target.wants/mysql.service → /lib
/systemd/system/mysql.service.
```

```text
# 更新源
sudo apt update
# 安装mysql服务
sudo apt install mysql-server

安装验证插件
sudo mysql_sercure_installation

查看数据库状态
systemctl status mysql

编辑 非必需:注释掉bind-address = 127.0.0.1
/etc/mysql/mysql.conf.d/mysqld.cnf

创建新用户
create user '用户名'@'访问主机' identified by '密码';
CREATE USER 'admin'@'localhost' IDENTIFIED BY '你要设置的密码'; 
对新增的用户更改加密方式和密码:
ALTER USER 'admin'@'localhost' IDENTIFIED WITH mysql_native_password BY 'admin';
```

#### 配置文件

修改密码：
这个方法测试不行：
```
cat /etc/mysql/debian.cnf

mysql -u debian-sys-maint -p

然后就是修改密码了:
连接到mysql数据库
1)、use mysql;

2)、
8.0这个不行：
update mysql.user set authentication_string=password('xxxx') where user='root' and Host ='localhost';

用这个命令：
ALTER USER 'root'@'localhost' IDENTIFIED BY 'xxx';
update user set  plugin="mysql_native_password";

3)、update user set  plugin="mysql_native_password";     
4)、flush privileges;
5)、quit; 

最后测试：
sudo service mysql restart
```

这个修改密码并开启远程链接可以：
```
1.mysql -u root -p
2.enter 进入mysql
3.ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'xxxxx';

或则：
1.输入mysql进入服务mysql: mysql
2.如果进入不了，可能是mysql服务没有开，开启mysql
service mysql status
service mysql start
3.进去mysql之后先设置root密码，因为8.0版本已经没有passwd函数了:
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
4.FLUSH PRIVILEGES;

5.添加远程访问使用root访问数据库的权限并刷新权限
update user set host='%' where user='root';
FLUSH PRIVILEGES;

6.修改mysql配置文件
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
在里面把bind-address =127.0.0.1注释掉

7.防火墙打开3306端口并刷新防火墙设置
sudo ufw allow 3306
sudo ufw reload
sudo ufw enable
```

```text
service mysql status
# 启动mysql service
sudo service mysql start 
mysql -u root -p;
show databases;
```
