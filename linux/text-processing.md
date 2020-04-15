# Linux 文本处理
## sed
awk和sed像一对兄妹，一个出现，就会问起另一个。现在，都来了。

sed基本参数解释
sed是stream editor的简称，擅长对文件进行各种正则操作、插入操作、替换操作和删除操作，可以全局，可以指定特定范围的行或者特定特征的行。

s/pat/replace/: 正则替换

前插行i, 后插行a, 替换行c, 删除行d, 输出行p

N: 读入下一行，同时存储；n:读入下一行，抛弃当前行


## 练习：找出空行
```
#创建测试文件
cat <<EOF > test.txt
freeaihub-1$
freeaihub-2$
freeaihub-3$
$
freeaihub-$

  $

freeaihub-5$

$

freeaihub-6$
EOF



egrep -n "^ *$" test.txt 
egrep -n  "^$|^ +$" test.txt 
```
## 练习：读取/etc/passwd第一列
```
cat /etc/passwd
egrep "^[^:]+" /etc/passwd  -o 
sed -r 's#(^.*)(:x.*:)(.*)#\3\2\1#g' /etc/passwd
```

来源注明：
- 1. Linux学习 - SED操作，awk的姊妹篇
 生信宝典 陈同 https://mp.weixin.qq.com/s/cywkIeRbhkYTZvkwTeIVSA?

<code title="Linux 文本处理" description="a" keyword="a">