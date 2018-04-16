# json_pickle
writer
****************json，重要！！！*****************
如果我们要在不同的编程语言之间传递对象，就必须把对象序列化为标准格式，比如xml，
但更好的方法是序列化为json，应为json表示出来就是一个字符串，就可以被所有语言读取，
也方便存储到磁盘或者通过网络传输。json不仅是标准格式，而且比xml更快，可以直接在web页面中读取，非常方便
json：可以用于任何语言之间的数据传递，但不能传递函数、类
pickle：几乎所有的数据类型都可以传递，包括函数、类等。但是只限于python语言之间
import json
dic={'name':'alex'}
i=8
s='hello'
l=[11,22,33,'asz',[1,2,'3']]
data=json.dumps(dic)
print(type(data))        #<class 'str'>
print(data)              #{"name": "alex"}

data=json.dumps(i)
print(type(data))        #<class 'str'>
print(data)              #8

data=json.dumps(s)
print(type(data))        #<class 'str'>
print(data)              #"hello"

data=json.dumps(l)
print(type(data))        #<class 'str'>
print(data)              #[11, 22, 33, "asz", [1, 2, "3"]]

*****序列化*****
把对象(变量)从内存中编程可存储或可传输的过程就叫序列化，在python中叫pickling。
反过来，将变量内容从序列化的对象重新写入到内存中叫反序列化，unpickling

json.dumps(value)
将value（普通的数据类型）转化为json字符串（序列化过程）。value中的元素如果有单引号，全部转化为双引号。
                            双引号保持不变，结果的类型全部都是json字符串。
value——json.dumps()——>json字符串，进行到存到文件或者传输等其他操作

json.loads(value1)
将value1（json字符串）转化为普通的数据类型（dumps之前的数据，即value）
value1——json.loads()——>普通的数据类型，进行其他操作

import json
dic={"name":'alex'}
f=open("json","w",encoding="utf-8")
a=json.dumps(dic)
f.write(a)
f.close()

*******dumps和dump之间的等价*********
a=json.dumps(dic)
f.write(a)
            等价于a=json.dump(dic,f)，dump只能用于文件操作，推荐使用前者

*******loads和load之间的等价*********
b=f.read()
B=json.loads(b)
            等价于B=json.load(f)，load只能用于文件操作，推荐使用前者

dic={"name":'alex'}
f=open("json1","w+",encoding="utf-8")
a=json.dumps(dic)
f.write(a)
b=f.read()
B=json.loads(b)    #这一步会报错，why？
print(B)
f.close()

import json
f=open("json","r",encoding="utf-8")
b=f.read()
B=json.loads(b)
print(type(B))      #<class 'dict'>
print(B)            #{'name': 'alex'}
print(f.closed)
f.close()
print(f.closed)

******只要符合json字符串的字符串都可以用load方法*******
import json
f=open("json_test","r",encoding="utf-8")
c=f.read()
C=json.loads(c)
print(C)
f.close()

文件中：{"name":'lili'}       结果会报错
文件中：{"name":"lili"}       正确结果

@@@@@@@@@@@@@@@@@@@@@@@@pickle模块@@@@@@@@@@@@@@@@@@@@@@@@@@@@
pickle模块：在使用上，pickle和json完全一样，只是序列化后不一样
import pickle
dic={"name":"alex"}
f=open("pickle","wb")
a=pickle.dumps(dic)    #———>序列化后是bytes类型
print(type(a))          #<class 'bytes'>
f.write(a)              #文件打开后内容是乱的。。。。。，也就是文件没有可读性
print(f.readable())     #False
f.close()



@@@@@@@@@@@@@@@@@@@@@@@@shelve模块@@@@@@@@@@@@@@@@@@@@@@@@@@@@
shelve模块比pickle模块简单，只有一个open函数，返回类似字典的对象，可读可写；
key必须为字符串，而值可以是python所支持的数据类型
shelve模块：和json，pickle模块类似，作传输使用，不能跨语言用，不常用
import shelve
f=shelve.open(r'shelve1')    #目的：把字典放入到文件中去
f['aa']={"name":"alex","age":19}
f["bb"]={"name":"alvin","age":190}
f.close()

按照字典的格式取存的就按照字典的格式去取
import shelve
f=shelve.open(r'shelve1')
print(f.get("aa")["age"])        #19
print(f.get("bb")["name"])       #alvin


其实shelve内部封装了pickle！！！
