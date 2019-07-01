# GS语言

## 域
* GS中的域(domain)扩展自handle，其中handle有一个Lock和Unlock的机制，而域主要应用了handle里面这个锁的机制。  

* domain类似于一个保护壳，每个对象都需要放在一个特定的保护壳里面保护起来。
域的出现主要是起到了一个保护对象数据安全的作用，它相当于一个内部实现的互斥锁。  

* 如果一个对象不是只读对象，一旦有一个协程调用到这个对象，它就会给对象加锁，这时候其它协程就不能去调用这个对象，
直到这个协程执行完毕，再给对象解锁，此时其它协程才可以调用这个对象。

* 绑定域方法
```             
function.call_in_domain(user, (){
    UserD.standard_user(user, standard);
});
```
or
```
function callback = (: handler.main :);
callback.bind_arg_domain(conn_or_user.get_domain());
callback.append_arg(conn_or_user);
callback.append_arg_arr(args);
return callback.call_domain();
```

## 对象
### RO对象
* RO对象中大部分为读操作，故默认方法是parallel的，由于是并行的，故调用方法的话，可以直接使用.符号调用，当RO中需要进行写操作时，且需要修改的数据类型为share_vale，此时便需要使用try_lock方法取锁，防止重复写的问题出现
### RW对象

### 个人心得
* 每个域中都有自己的对象及其数据，域主要用了锁的机制，保证了域内数据的安全，且不同域之间方法的调用需要进行跨域调用
* gs中有.?调用，如果该对象存在这个方法就调用，不存在就不调用且不报错，非常方便，如果是正常流程我们可能要在对象的父类中加入对应方法防止报错