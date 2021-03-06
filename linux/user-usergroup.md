# Linux 用户和用户组

Linux系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

在Linux中，有三种用户：

- Root 用户：也称为超级用户，对系统拥有完全的控制权限。超级用户可以不受限制的运行任何命令。Root 用户可以看做是系统管理员。
- 系统用户：系统用户是Linux运行某些程序所必须的用户，例如 mail 用户、sshd 用户等。系统用户通常为系统功能所必须的，不建议修改这些用户。
- 普通用户：一般用户都是普通用户，这些用户对系统文件的访问受限，不能执行全部Linux命令。

Linux支持用户组，用户组就是具有相同特征的用户的集合。一个组可以包含多个用户，每个用户也可以属于不同的组。用户组在Linux中扮演着重要的角色，方便管理员对用户进行集中管理。

## 与用户和组有关的系统文件

与用户和组有关的系统文件：

| 系统文件    | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| /etc/passwd | 保存用户名和密码等信息，Linux系统中的每个用户都在/etc/passwd文件中有一个对应的记录行。这个文件对所有用户都是可读的。 |
| /etc/shadow | /etc/shadow中的记录行和/etc/passwd中的相对应，他由pwconv命令根据/etc/passwd中的数据自动产生，它的格式和/etc/passwd类似，只是对密码进行了加密。并不是所有的系统都支持这个文件。 |
| /etc/group  | 以记录行的形式保存了用户组的所有信息。                       |

用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。

每个用户账号都拥有一个唯一的用户名和各自的口令。

用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。

实现用户账号的管理，要完成的工作主要有如下几个方面：

- 用户账号的添加、删除与修改。
- 用户口令的管理。
- 用户组的管理。


## 与用户账号有关的系统文件
### 用户列表文件:`/etc/passwd`
Linux系统中的每个用户都在`/etc/passwd`文件中有一个对应的记录行，它记录了这个用户的一些基本属性。

这个文件对所有用户都是可读的。使用`cat`命令查看:
```
cat /etc/passwd
```

`/etc/passwd`这个文件中一行记录对应着一个用户，每行记录又被冒号(:)分隔为7个字段，其格式和具体含义如下：

对每个字段的说明：

| 字段       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| 用户名     | 用户名是惟一的，长度根据不同的linux系统而定，一般是8位。     |
| 密码       | 由于系统中还有一个/etc/shadow文件用于存放加密后的口令，所以在这里这一项是“x”来表示，如果用户没有设置口令，则该项为空。如果passwd字段中的第一个字符是“*”的话，那么，就表示该账号被查封了，系统不允许持有该账号的用户登录。 |
| 用户ID     | 系统内部根据用户ID而不是用户名来识别不同的用户，用户ID有以下几种：0代表系统管理员，如果你想建立一个系统管理员的话，可以建立一个普通帐户，然后将该账户的用户ID改为0即可。1~500系统预留的ID。500以上是普通用户使用。 |
| 组ID       | 其实这个和用户ID差不多，用来管理群组，与/etc/group文件相关。 |
| 描述信息   | 这个字段几乎没有什么用，只是用来解释这个账号的意义。在不同的Linux系统中，这个字段的 格式并没有统一。在许多Linux系统中，这个字段存放的是一段任意的注释性描述文字，用做finger命令的输出。 |
| 用户主目录 | 用户登录系统的起始目录。用户登录系统后将首先进入该目录。root用户默认是/，普通用户是/home/username。 |
| 用户Shell  | 用户登录系统时使用的Shell。                                  |

### 用户密码对应文件 `/etc/passwd`
```
cat /etc/shadow
```
/etc/passwd文件文件是用户管理工作涉及的最重要的一个文件，它是所有用户都可读的，如果用户的密码太简单或规律比较明显的话，将很容易破解。因此对安全性要求较高的Linux系统都把加密后的口令字分离出来，单独存放在一个文件中，这个文件是/etc/shadow文件。 有超级用户才拥有该文件读权限，用来保证了用户密码的安全性。

/etc/shadow中的记录行与/etc/passwd中的一一对应，它由pwconv命令根据/etc/passwd中的数据自动产生，它的文件格式与/etc/passwd类似，由若干个字段组成，字段之间用":"隔开。这些字段是。

格式及解读如下：
登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志


### 用户组对应文件:`/etc/group`
```
cat /etc/group
```

将用户分组是Linux 系统中对用户进行管理及控制访问权限的一种手段。
用户组的所有信息都存放在/etc/group文件中，每个用户都属于某个用户组；一个组中可以有多个用户，一个用户也可以属于不同的组。

当一个用户同时是多个组中的成员时，在/etc/passwd文件中记录的是用户所属的主组，也就是登录时所属的默认组，而其他组称为附加组。

用户要访问属于附加组的文件时，必须首先使用newgrp命令使自己成为所要访问的组中的成员。

用户组的所有信息都存放在/etc/group文件中。此文件的格式也类似于/etc/passwd文件，由冒号(:)隔开若干个字段，这些字段有：

组名:口令:组标识号:组内用户列表

"组名"是用户组的名称，由字母或数字构成。与/etc/passwd中的登录名一样，组名不应重复。

"口令"字段存放的是用户组加密后的口令字。一般Linux 系统的用户组都没有口令，即这个字段一般为空，或者是*。

"组标识号"与用户标识号类似，也是一个整数，被系统内部用来标识组。

"组内用户列表"是属于这个组的所有用户的列表/b]，不同用户之间用逗号(,)分隔。这个用户组可能是用户的主组，也可能是附加组。


## 用户与用户组相关命令
用户账号的管理工作主要涉及到用户账号的添加、修改和删除。

添加用户账号就是在系统中创建一个新账号，然后为新账号分配用户号、用户组、主目录和登录Shell等资源。刚添加的账号是被锁定的，无法使用。

| 命令     | 说明             |
| -------- | ---------------- |
| useradd  | 添加用户。       |
| usermod  | 修改用户信息。   |
| userdel  | 删除用户。       |
| groupadd | 添加用户组。     |
| groupmod | 修改用户组信息。 |
| groupdel | 删除用户组。     |

### 新建用户`useradd`
命令格式: `useradd 选项 用户名`
参数说明：
- 选项:
  - -c comment 指定一段注释性描述。
  - -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
  - -g 用户组 指定用户所属的用户组。
  - -G 用户组，用户组 指定用户所属的附加组。
  - -s Shell文件 指定用户的登录Shell。
  - -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。
- 用户名:
  指定新账号的登录名。

#### 练习
```
#1.创建一个用户名为test的用户，并指定其的主目录们于/home/test
useradd -d /home/test test

#2.创建一个用户名为test1的用户，并指定其登录后的Shell是/bin/sh这个Shell
useradd -s /bin/sh test1
```

### 修改帐号`usermod`


### 删除用户`userdel`
命令格式: `userdel 选项 用户名`

如果一个用户的账号不再使用，可以从系统中删除。删除用户账号就是要将/etc/passwd等系统文件中的该用户记录删除，必要时还删除用户的主目录。

#### 练习
```
#1.删除一个用户名为test的用户
userdel  test
#查看该用户是否在存在于/etc/passwd中
cat /etc/passwd

#2.创建一个用户名为test1的用户，并指定其登录后的Shell是/bin/sh这个Shell
useradd -s /bin/sh test1
su test1 #切换用户并观察Shell的不同
```

### 变更用户的密码 `passwd`
```
passwd 选项 用户名
```
用户管理的一项重要内容是用户口令的管理。用户账号刚创建时没有口令，但是被系统锁定，无法使用，必须为其指定口令后才可以使用，即使是指定空口令。

指定和修改用户口令的Shell命令是passwd。超级用户可以为自己和其他用户指定口令，普通用户只能用它修改自己的口令。

#### 练习:更改用户test的密码
```
passwd test
```


## Linux系统用户组的管理
每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对`/etc/group`文件的更新。

### 添加用户组`groupadd`
命令格式: `groupadd 选项 用户组名`

### 删除用户组`groupdel`
```
groupdel -g 102 group2
```

### 修改用户组的属性`groupmod`
```
groupmod -g 102 group2
```

#### 练习：创建一个用户并进行将其的用户组改为root组


## 其它用户相关的命令
### 切换用户命令:
 `su` 

### 查看用户命令:
`users` `who` `w` 



## 本节作业
### 1.创建一个用户jone，密码为16238后，并将其置于用户组wather中后。使用该用户成功登陆，使用相关命令查看该用户信息，并查看`/etc/passwd`和`/etc/group`文件进行验证。

### 1.更改用户jone的主目录为/home/jone-bak，并将密码更改为21435
