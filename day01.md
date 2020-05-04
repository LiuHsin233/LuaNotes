# 关于table的长度
> 总结: 判断table是否为空使用next(myTable)~=nil这种语句,慎用#运算符求table长度
## 关于判断table是否为空
1. myTable=={}或者myTable~={}
> {}是一个空表,myTable和{}比较,相当于比较这两个table的地址,最终的结果一定是不相等.
> 如果你想舍弃一个表,写了myTable=nil这种语句的话,后续判断是可以使用if myTable == nil这种语句的
2. #myTable==0
> 关于#运算符,用来求一个table的长度,但是要求key值是有序的并且从1开始(就是key值是1,2,3..n这种)
```Lua
    -- 可以用这个代码测试一下#计算的方式
    myTable={
        test="myTest",
        1,2,3,4,5,
        name="myTable",
        date="2019-12-19"
    }
    print(#myTable)
    myTable[7]=7
    print(#myTable)
    myTable[6]=6
    print(#myTable)
```
> 使用循环遍历table求出table的长度
```Lua
    local count =0
    for _,conf in pairs(myTable) do
        count=count+1
    end
    print(count)
```
## 关于lua的真假
> 在lua中只有nil和false视为假,数字0和其他都视为真,所以在和其他语言交互的时候尤其注意
