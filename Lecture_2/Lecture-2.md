foo=bar 在bash中为变量赋值的语法（注意：使用空格foo = bar是不能正常工作的）
Bash 中的字符串通过 ' 和 " 分隔符来定义，但是它们的含义并不相同。以 ' 定义的字符串为原义字符串，其中的变量不会被转义，而 " 定义的字符串会将变量值进行替换
```
foo=bar
echo "$foo"
#打印bar
echo '$foo'
打印$foo
```
bash 支持函数，它可以接受参数并基于参数进行操作。下面这个函数是一个例子，它会创建一个文件夹并使用 cd 进入该文件夹。
```
mcd(){
	mkdir -p "$1"
	cd "$1"
}
```
退出码可以搭配 &&（与操作符）和 ||（或操作符）使用，用来进行条件判断，决定是否执行其他程序。它们都属于短路 运算符（short-circuiting） 同一行的多个命令可以用 ; 分隔。程序 true 的返回码永远是 0，false 的返回码永远是 1
```
false || echo "Oops,fail"
#Oops,fail

true || echo "Will not be printed"
#

true && echo "Thing went well"
#Things went well

false && echo "Will not be printed"
#

false ; echo "This will always run"
#This will always run
```
下面这个例子展示了一部分上面提到的特性。这段脚本会遍历我们提供的参数，使用 grep 搜索字符串 foobar，如果没有找到，则将其作为注释追加到文件中
```
#!/bin/bash
echo "Starting program at $(date)" #date会被替换成日期和时间
echo "Running program $0 with $# arguments with pid $$"
for file in "$@"; do
	grep foobar "$file" > /dev/null 2> /dev/null
	#如果模式没有找到，则grep退出状态为1
	#我们将标准输出流和标准错误流重定向到Null，因为我们并不关心这些信息
	if [[ $? -ne 0]]; then
		echo "File $file does not have any foobar,adding one"
		echo "# foobar" >> "$file"
	fi
done
```
- 通配符 - 当你想要利用通配符进行匹配时，你可以分别使用 ? 和 * 来匹配一个或任意个字符。例如，对于文件 foo, foo1, foo2, foo10 和 bar, rm foo? 这条命令会删除 foo1 和 foo2 ，而 rm foo* 则会删除除了 bar 之外的所有文件。
- 花括号 {} - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令。这在批量移动或转换文件时非常方便。
```
convert image.{png,jpg}
# 会展开为
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# 会展开为
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# 也可以结合通配使用
mv *{.py,.sh} folder
# 会移动所有 *.py 和 *.sh 文件

mkdir foo bar

# 下面命令会创建 foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h 这些文件
touch {foo,bar}/{a..h}
touch foo/x bar/y
# 比较文件夹 foo 和 bar 中包含文件的不同
diff <(ls foo) <(ls bar)
# 输出
# < x
# ---
# > y
```
注意，脚本并不一定只有用 bash 写才能在终端里调用。比如说，这是一段 Python 脚本，作用是将输入的参数倒序输出：
```
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```
程序员们面对的最常见的重复任务就是查找文件或目录。所有的类 UNIX 系统都包含一个名为 find 的工具，它是 shell 上用于查找文件的绝佳工具。find 命令会递归地搜索符合条件的文件
```
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path '*/test/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'
```
除了列出所寻找的文件之外，find 还能对所有查找到的文件进行操作。这能极大地简化一些单调的任务
```
# 删除全部扩展名为.tmp 的文件
find . -name '*.tmp' -exec rm {} \;
# 查找全部的 PNG 文件并将其转换为 JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```


作业
```
ls -a 显示所有文集（包括隐藏文件）
ls -t 文件以最近访问顺序排序
ls -h 文件以人们熟悉的格式输出
```
编写两个 bash 函数 marco 和 polo
```
marco(){
     echo "$(pwd)" > $HOME/marco_history.log
     echo "save pwd $(pwd)"
 }
 polo(){
     cd "$(cat "$HOME/marco_history.log")"
 }
```
或者
```
marco() {
     export MARCO=$(pwd)
 }
 polo() {
     cd "$MARCO"
 }
```
编写一段 bash 脚本，运行如下的脚本直到它出错，将它的标准输出和标准错误流记录到文件，并在最后输出所有内容
```
n=$(( RANDOM % 100 ))

 if [[ n -eq 42 ]]; then
     echo "Something went wrong"
     >&2 echo "The error was using magic numbers"
     exit 1
 fi

 echo "Everything went according to plan"
```
使用 while 循环
```
count=0
 echo > out.log

 while true
 do
     ./buggy.sh &>> out.log
     if [[ $? -ne 0 ]]; then
         cat out.log
         echo "failed after $count times"
         break
     fi
     ((count++))

 done
```
使用 for 循环
```
echo > out.log
 for ((count=0;;count++))
 do
     ./buggy.sh &>> out.log
     if [[ $? -ne 0 ]]; then
         echo "failed after $count times"
         break

     fi
 done
```
使用 until 
```
count=0
 ./buggy.sh &>> out.log
 until [[ "$?" -ne 0 ]];
 do
     count=$((count+1))
     ./buggy.sh &>> out.log
 done

 echo "failed after $count runs"
```
执行测试脚本 debug.sh, 并验证脚本结果的正确性
```
./debug.sh
cat out.log | grep Everything | wc -l
```
find命令
```
 mkdir html_root
  cd html_root
  touch {1..10}.html
  mkdir html
  cd html
  touch xxxx.html
find . -type f -name "*.html" | xargs -d '\n'  tar -cvzf html.zip
```