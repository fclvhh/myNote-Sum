# 快速入门

## 单词速记

quit  : 退出
write/read  : 保存  
yank : (copy)的意思,因为copy这个命令被系统占用了
paste : 粘贴
delete
change
find
word
forward/backward : 前进/后退
up/down  : ctrl +u   翻页   ctrl+d  往下翻页
insert/append    插入
do/undo/redo  :  undo 撤销   redo 还原       撤销操作:u   还原造作:ctrl+R
replace



## 插入操作

1. insert  (在光标前面插入)     shift+i  =>到达行首  
2. append(在光标后面插入)    shift +a =>到达行尾
3. I   =>到达行首
4. A   =>到达行尾



## 翻页操作

1. ctrl D 下翻半页
2. ctrl U 上翻半页



## 删除 delete 和修改 change 操作

- delete/change word   删除/   单词      dd删除一行        db 删除前面的   delete  in ()   ==> d+i+(     
- delete/change in ()    ==> d+i+(    删除括号里面的
- delete at ()  ==>d+a+()       包括括号一起删除
- delete in/at {}   ==>d+a+{}   删除花括号里面的内容
- delete in tag   如: delete in div  ==>d+i+div   删除div里面的内容
- change in tag  



**任何操作前面加上数字,就代表这个操作运行几次!!!!**



## 四种模式

1. insert 模式（编辑模式）
2. 普通模式（normal模式）
3. 冒号模式（命令模式）
4. v 模式（编辑选区模式）

## 复制粘贴

1. 到达你想复制的开头
2. 按v，到达你想复制的结尾
3. 按y复制
4. 按p或P粘贴



# 入门

vim的官方中文文档:

1.     在命令行输入 vimtutor ，即可查看官方自带的中文教程。看完它。



# 移动光标

             ** 要移动光标，请依照说明分别按下 h、j、k、l 键。 **
    
             ^
             k              提示： h 的键位于左边，每次按下就会向左移动。
       < h       l >               l 的键位于右边，每次按下就会向右移动。
             j                     j 键看起来很象一支尖端方向朝下的箭头。
             v


# vim的进入和退出

  1. 按<ESC>键(这是为了确保您处在正常模式)。
  2. 然后输入：                 :q! <回车>
     这种方式的退出编辑器会丢弃您进入编辑器以来所做的改动。
  3. 如果您看到了命令行提示符，请输入能够带您回到本教程的命令，那就是：
     vimtutor <回车>
   4. 如果您自信已经牢牢记住了这些步骤的话，请从步骤1执行到步骤3退出，然
   5. 后再次进入编辑器。

提示： :q! <回车> 会丢弃您所做的任何改动。几讲之后您将学会如何保存改动到文件。



# 文本编辑之删除

在正常(Normal)模式下，可以按下 x 键来删除光标所在位置的字符



# 文本编辑之插入[^insert]

在正常模式下，可以按下 i 键来插入文本。



# 文本编辑之添加[^add]

 ** 按 A 键以添加文本。 **



# 编辑文件

                    ** 使用 :wq 以保存文件并退出。 **



# 第三节

## 置入类命令

** 输入 p 将最后一次删除的内容置入光标之后。 **



## 替换类命令

​          ** 输入 r 和一个字符替换光标所在位置的字符。**



# 第四节

## 定位及文件状态

  ** 输入 CTRL-G 显示当前编辑文件中当前光标所在行位置以及文件状态信息。
     输入大写 G 则直接跳转到文件中的某一指定行。**

 输入您曾停留的行号，然后输入大写 G。这样就可以返回到您第一次按下
     CTRL-G 时所在的行了。

