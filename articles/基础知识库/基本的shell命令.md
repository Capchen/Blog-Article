### 文件操作

```shell
ls #查看目录下文件
ls -a #查看隐藏文件
ls -F #在文件夹后面加了／，在可执行文件后面加了*
ls -R #递归遍历子文件夹
ls -l #显示长列表形式的文件信息


touch filename # 创建一个空文件
cp source destination # 将源文件复制成一个新文件并且以 destination命名

# 制表键自动补全文件名

```

#### 链接文件
如果需要在系统中维护同一文件的两份或者多分副本，除了保存多分单独的物理文件副本之外。还可以采用保存一份物理副本和多个虚拟副本的方法，这些虚拟副本被称为链接。链接是目录中指向文件真是位置的占位符。在linux中有两种不同类型的链接
##### 符号链接
符号链接就是一个实实在在的文件，它指向存放在虚拟目录结构中的某个地方的一个文件，这两个通过符号链接在一起的文件，彼此的内容并不相同

```shell
ln -s data_file rl_data_file 
#以data_file文件创建一个rl_data_file符号链接，注意rl_data_file只是个占位符

# demo
wangcc:chenjian oukawa$ ln -s chenj rl_chenj
wangcc:chenjian oukawa$ ls -l chenj
-rw-r--r--@ 1 oukawa  staff  11 10 30 22:07 chenj
wangcc:chenjian oukawa$ ls -l *chenj
-rw-r--r--@ 1 oukawa  staff  11 10 30 22:07 chenj
lrwxr-xr-x  1 oukawa  staff   5 10 30 22:08 rl_chenj -> chenj
wangcc:chenjian oukawa$ ls -i *chenj
8082497 chenj		8082525 rl_chenj
wangcc:chenjian oukawa$ 


```

##### 硬链接
硬链接会创建独立的虚拟文件，其中包含原始文件的信息及位置。引用硬链接文件等同于引用了原始文件。

> 只能对处于同一存储媒体的文件创建硬链接，要想在不同的存储媒体之间创建链接，只能是软链接。

### 重命名文件

```shell
mv fall fzll # 重命名文件称为移动 ，表示移动fall文件为fzll
```

### 删除文件

```shell
rm -i fall #删除fall文件，并且会提示
rm -f fall #强制删除，不提示
```
### 
### 处理目录

```shell
mkdir newDir #创建新目录
rmdir newDir #删除空目录
rm -rf newDir #全部删除，不提示
```

### 查看文件

```shell
file my_file #查看文件是什么类型
cat -n test1 #查看文件并加上行号
#cat的缺陷是一旦运行就无法控制后面的操作
more
```

