### yield 语法

```python
def foo():
    print("starting...")
    while True:
        res = yield 4
        print("res:",res)
g = foo()
print(next(g))
print("*"*20)
print(next(g))

'''
输出:
starting...
4
********************
res: None
4
'''
```

1. 程序开始执行以后，因为foo函数中有yield关键字，所以foo函数并不会真的执行，而是先得到一个生成器g(相当于一个对象)

2. 直到调用`next`方法，foo函数正式开始执行，先执行foo函数中的print方法，然后进入while循环

3. 程序遇到yield关键字，然后把yield想想成return, return了一个4之后，程序停止，**并没有执行赋值给res操作**，此时`next(g)`语句执行完成，所以输出的前两行（第一个是while上面的print的结果,第二个是return出的结果）是执行print(next(g))的结果，

4. 程序执行print("*"*20)，输出20个*

5. 又开始执行下面的`print(next(g))`, 这个时候和上面那个差不多，不过不同的是，这个时候是从刚才那个next 程序停止的地方开始执行的，也就是要执行 res 的赋值操作，这时候要注意，**这个时候赋值操作的右边是没有值的（因为刚才那个是return出去了，并没有给赋值操作的左边传参数）**，所以这个时候res赋值是None,所以接着下面的输出就是res: None,

6. 程序会继续在 while 里执行，又一次碰到 yield ,这个时候同样 return 出 4，然后程序停止，print 函数输出的4就是这次 return 出的 4.

```python
def foo():
    print("starting...")
    while True:
        res = yield 4
        print("res:",res)
g = foo()
print(next(g))
print("*"*20)
print(g.send(7))

'''
输出:
starting...
4
********************
res: 7
4
'''
```

先大致说一下 send 函数的概念：send 是发送一个参数给 res 的，因为上面讲到，return 的时候，并没有把 4 赋值给 res，下次执行的时候只好继续执行赋值操作，只好赋值为 None 了，而如果用 send 的话，开始执行的时候，先接着上一次（return 4之后）执行，先把 7 赋值给了 res, 然后执行 next 的作用，遇见下一回的 yield，return 出结果后结束

5. 程序执行 `g.send(7)`，程序会从 yield 关键字那一行继续向下运行，send 会把 7 这个值赋值给 res 变量

6. 由于 send 方法中包含 `next() `方法，所以程序会继续向下运行执行 print 方法，然后再次进入 while 循环

7. 程序执行再次遇到 yield 关键字，yield 会返回后面的值后，程序再次暂停，直到再次调用 next 方法或 send 方法



##### 为什么要用 yield

yield 生成一个迭代起，迭代器每次只生成一个元素，不会一次性将所有元素都加载到内存中，而是在需要时逐个生成和返回元素。这在处理大型数据集或无限序列时非常有用，可以节省内存并提高性能。

```python
def foo(num):
    print("starting...")
    while num <5  :
        num = num+1
        yield num
for n in foo(0):
    print(n)
'''
输出:
1 2 3 4 5
'''
nums = [1, 2, 3, 4, 5]
for num in nums:
  print(num)
```

与下面的 for 循环相比，for 循环会把 nums 一次性加载到内存中，而 foo 迭代器则是按需生成逐个返回，在处理大数据集时可以节省内存并提高性能