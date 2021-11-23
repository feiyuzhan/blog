
# item 12. copy 动作中的自我赋值

## Question：
- 自我赋值
- 异常检测


## Solution：

### Pass by reference

对于指针的赋值来说，自我赋值中需要注意的是删除旧指针的时机，如果在一开始就删除了旧指针，则当自我赋值时会出现指向空指针的错误。

可以利用 if(this == &rhs) 来检测自我赋值，但是不能解决异常的处理

这个时候可以利用临时变量来保存旧指针，再赋值完成后再删除旧指针即可，此时会仅仅存在旧指针的copy副本。

```c++
Bitmap* tmp = pb;
pb = new Bitmap(*rhs.pb); // pb指向待拷贝的一个副本，如果是自我赋值，得到自身的一个副本
delete tmp; //如果是自我赋值，这时候原指针已经产出copy，可以正常删除
return *this;
```

另一种方式就是 swap，原理是：确定副本被正常创建完毕再swap，达到要么swap全部，要么不发生swap的效果

```c++
Widgt& Widgt::operator=(const Widgt& rhs){ //传入的是引用，区别下面的pass by value
    Widgt tmp(rhs); // 和上面的同理，创建一个临时副本
    swap(tmp);  //交换value
    return *this;
}
```

### Pass by value
更加巧妙但可读性差
```c++
Widgt& Widgt::operator=(const Widgt rhs){ //传入的是值，已经是一个临时副本
    // Widgt tmp(rhs); 不再需要副本
    swap(rhs);  //同上
    return *this;
}
```
