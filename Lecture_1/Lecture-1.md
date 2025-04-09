### 课程内容
 - date
 - echo执行参数
 - echo hello
 - $PATH环境变量
 - which echo绕过PATH环境变量
 - pwd显示当前工作目录
 - cd用来切换命令
 - 用“/”是绝对路径，不用就是相对路径
 - ls -l /home home这里可以写其它目录，表示显示/后面的目录下的信息
 - man ls显示程序参数、输入输出的信息，还有工作方式等
 - echo hello > hello.txt重定向输出内容
 - cat < hello.txt > hello2.txt 将hello.txt的内容重定向到hello2.txt

### 作业内容
 - 码
```
cd /tmp
mkdir missing
ls
man touch
touch semester
echo '#!/bin/sh' > semester
echo curl --head --silent https://missing.csail.mit.edu > semester
cat semester
echo '#!/bin/sh' > semester
echo curl --head --silent https://missing.csail.mit.edu >> semester
cat semester
./semester
ls -l
chmod 777 semester
./semester | grep last-modified > ~/last-modified.txt
cat ~/last-modified.txt
```