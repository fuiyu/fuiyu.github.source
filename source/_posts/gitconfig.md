---
title: git config详解
date: 2017-03-06 21:31
tags: git,config
---

**git config 用于git的基础配置，可以通过修改配置文件或者使用git config命令更改配置**

### 一、概要格式
```c
$ git config [<file-option>] [type] [--show-origin] [-z|--null] name [value [value_regex]]
$ git config [<file-option>] [type] --add name value
$ git config [<file-option>] [type] --replace-all name value [value_regex]
$ git config [<file-option>] [type] [--show-origin] [-z|--null] --get name [value_regex]
$ git config [<file-option>] [type] [--show-origin] [-z|--null] --get-all name [value_regex]
$ git config [<file-option>] [type] [--show-origin] [-z|--null] [--name-only] --get-regexp name_regex [value_regex]
$ git config [<file-option>] [type] [-z|--null] --get-urlmatch name URL
$ git config [<file-option>] --unset name [value_regex]
$ git config [<file-option>] --unset-all name [value_regex]
$ git config [<file-option>] --rename-section old_name new_name
$ git config [<file-option>] --remove-section name
$ git config [<file-option>] [--show-origin] [-z|--null] [--name-only] -l | --list
$ git config [<file-option>] --get-color name [default]
$ git config [<file-option>] --get-colorbool name [stdout-is-tty]
$ git config [<file-option>] -e | --edit
```
你可以通过命令进行`query`（查询）/`set`（设置）/`replace`（替代）/`unset` （取消），`name`一般是指section“.”后面的key变量，如`git config user.name fuiyu`

结果
```c
[user]
    name = fuiyu
```
上文的values的值可以包括，
**boolean**，`true： yes | no | true | 1 , false: no | off | false | 0`
**integer**，整数型
**color**，支持normal, black, red, green, yellow, blue, magenta, cyan and white; 还支持属性 bold, dim, ul, blink and reverse或加no前缀（e.g. , nored ,noul ,etc）， 如果你的终端支持，也可以写RGB，16进制的颜色格式，其他详见git官网。
**pathname**，可以以"~/" 或 "~user/" 开头, 通常 ~/ 指向 $HOME,  ~user/ 指向用户的主目录.

### 二、基本用法
#### 一、 config配置文件的存储位置以及权重
linux 类系统下位置：
1./etc/gitconfig 文件：系统级配置文件，包含了适用于系统所有用户和所有仓库的值。如果你传递参数选项’--system’ 给 git config，它将明确的读和写这个文件。 
2.~/.gitconfig 文件 ：全局配置文件，具体到你的用户。你可以通过传递--global 选项使Git 读或写这个特定的文件。
3.位于git目录的config文件 (也就是 .git/config) ：仓库级配置文件，无论你当前在用的仓库是什么，特定指向该单一的仓库。每个级别重写前一个级别的值。因此，在.git/config中的值覆盖了在/etc/gitconfig中的同一个值。
window系统位置：
1.系统级配置文件，git安装目录下，文件名为gitconfig，如D:\Program Files\Git\mingw64\etc\.gitconfig
2.全局配置文件，在用户目录下，文件名为.gitconfig，如C:\users\fuiyu\.gitconfig
3.仓库级配置文件，git目录下的conifg文件

权重分配：
仓库>全局>系统

#### 二、用git config命令查看配置文件
命令参数--list |  -l
格式：git config [–local|–global|–system] -l
查看仓库级的config，即.git/.config，命令：git config –local -l
查看全局级的config，即C:\Users\zuoyu.ht\.gitconfig，命令：git config –global -l
查看系统级的config，即D:\Program Files\Git\etc\gitconfig，命令：git config –system -l
查看当前生效的配置，命令：git config -l

#### 三、配置用户信息
开始所提到的git config user.name fuiyu，还可以设置email，address。

四、 用git config命令编辑配置文件
命令参数为--edit | -e
格式：git config [–local|–global|–system] -e
查看仓库级的config，即.git/.config，命令：git config –local -e，与–list参数不同的是，git config -e默认是编辑仓库级的配置文件。
查看全局级的config，即C:\Users\zuoyu.ht\.gitconfig，命令：git config –global -e
查看系统级的config，即D:\Program Files\Git\etc\gitconfig，命令：git config –system -e
   执行这个命令的时候，git会用配置文件中设定的编辑器打开配置文件。
(git config --global core.editor vim用于配置编辑器)

#### 五、增加一个配置项
命令参数--add
用法类似于config section.key value，同上，也可以加上[–local|–global|–system]作为参数指向不同级的文件

#### 六、取消配置项
命令参数 --unset
用法也跟第五点差不多

 