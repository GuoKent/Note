### Vim

##### vim 打开文件

```python
vim filename.txt
vim -R filename.txt  # 只读
```

##### vim 查看文档

```python
# 光标跳转
shift + g  # 跳转到底部
gg  # 跳转到首行
shift + 4  # 跳转到当前行最后一个字符
0  # 跳转到当前行第一个字符

# 翻页
ctrl + f  # 往后翻一页 forward
ctrl + b  # 往前翻一页 backward
ctrl + d  # 往后翻半页 down
ctrl + u  # 往前翻半页 up
```

##### vim 编辑文档

```python
i  # insert 模式
Esc  # 退出至正常模式
:  # 命令行模式 shift + ;
w  # 保存
q  # 退出文档
wq  # 保存并退出
!  # 强制操作 wq!强制保存退出
```

