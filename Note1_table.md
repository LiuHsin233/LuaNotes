# 关于table
> 这里的代码测试可以使用 https://tool.lu/coderunner/ 来进行测试
## table作用
1. table是lua中的一种数据结构,使用key-value的方式来存放各种数据.
> 你可以想象你书桌上那多个抽屉,然后给每个抽屉一个标识,编个号1,2,3,或者取个名字小白小红什么的都可以,但是要确保这个标识是唯一的.那么你只要说出这个标识,就一定可以找到这个抽屉.这个标识就是所谓的key值,而你要存放在抽屉的东西就是所谓的value.通过建立key和value,你可以直接从table中存储或者取得数据.
2. key值,也称为索引,在table中不同的索引值必须不一样,或者说,如果要同时存放两个不同的数据,必须给定不同的索引值.key允许是一个整数,或者是一个字符串
```Lua
    tb={
        [1]=233,    -- 使用整数作为key值
        ["id"]=123, -- 使用字符串作为key值
        age=23,     -- 这里实际上等同于["age"]=23,也是用字符串作为key值
    }
```
3. value,使用key值从table中取出,可以是任意类型(整数,小数,字符串,另外一个table都可以)
```Lua
    -- 取得value的几种方式
    tb[1]      -- 使用 表名[key] 的形式取出value
    tb["id"]   -- 同上,但是这里的id需要加上双引号
    tb.id      -- 这里是一个语法糖,直接使用 表名.变量名的形式 等同于tb.["id"]

    -- 打印对应的value值
    print(tb["id"])
    print(tb.id)


    --[[
        冷知识: 虽然使用的时候允许使用"1"这样的key值,但是只能使用tb["1"]而不能使用tb.1这种形式.
        tb["1"]和tb[1]两个key值不同.
    ]]
```
## table和for循环
> 如果想要取得table中的全部内容,可以搭配循环来使用,这里主要介绍for循环和相关的搭配
> 因为table允许存放不同类型的value值,所以这里给出一个简单的table作为演示,复杂的table后续补充
```Lua
    tb={
        [1]=1,
        [2]=1,
        [3]=2,
        [4]=3,
        [5]=5,
        [7]=13,
    }
```
### 1. for..in搭配pairs
    ```Lua
        for k,v in pairs(tb) do
            print(k..v)-- 对应的操作 这里的k对应key值,v对应value
        end
    ```
    >测试打印结果,你会发现,虽然能够打印出全部的数据,但是打印的结果并不是按照key有序的顺序来的
### 2. for..in搭配ipairs
    ```Lua
        for k,v in ipairs(tb) do
            print(k..v)-- 对应的操作 这里的k对应key值,v对应value
        end 
    ```
    > 测试结果中,按照key值顺序来打印出,但是并不是打印出所有的数据,实际上,ipairs从1作为开始,循环打印里面的元素,直到遇到第一个nil.也就是说依次打印出tb[1]~tb[5],当遇到tb[6]的时候(没有赋值,为nil),就结束这个循环.如果tb中有使用字符串的key值,使用ipairs也是没法打印出来的,打印从tb[1]开始,所以就算有tb[0]也是不会打印的
### 3. for搭配#
    > lua中#运算符用于计算字符串或者table的长度,所以对于某些表格也可以使用#搭配for进行循环
    > 因为lua中的#并不总是能计算出table中有效元素的个数(这里不具体讨论,有兴趣可参考https://cloud.tencent.com/developer/article/1559492),所以在搭配for循环的时候,只给出可用的示例,其余情况请慎用#

    ```Lua
        tb={
            [1]=1,
            [2]=1,
            [3]=2,
            [4]=3,
            [5]=5,
            [6]=13,
        } --key值连续并且没有中断
        for i=1,#tb do  -- 从1开始 到#tb
         print(tb[i])
        end

        tb1={
            [0]=0,
            [1]=1,
            [2]=1,
            [3]=2,
            [4]=3,
            [5]=5,
            [6]=13,
        } --key值连续并且没有中断
        for i=0,#tb1-1 do  -- 从0开始 这里最后一个key值是#tb1-1
         print(tb1[i])
        end
    ```
## 在table中存放不同的数据
1. 因为table中可以存放各种类型的数据,包括table,甚至于函数,所以在使用的时候,需要分清楚table中的数据用法
```Lua
    local tb={} --空表
    --[[
        因为在插入数据的时候,后续可能不清楚到底插入了哪些数据或者数据的格式,因此可以在注释中补充表的格式以及作用
        tb={
            count   --记录个数
            record={ --存储记录
                [rank]={    --每条记录的格式
                    id
                    name
                    grade
                }
            }
        }
    ]]

    local function addRecord(id,name,grade)
        if not tb.record then tb.record={} end --使用前最好检查要使用的内容是不是nil
        local record={
            id=id,          -- 这里其实等价 ["id"]=id  后面的id是传入的参数
            name=name,      -- 如果感觉不容易区分,换参数名就行
            grade=grade
        }
        tb.record[tb.count or 0]=record --对于数值,如果没有赋值默认为nil
        --使用的时候可以使用or运算符   tb.count or 0 如果之前没有定义过tb.count,那么就用0,当然,使用其他值  比如tb.count or 1也是可以的
        tb.count =(tb.count or 0)+1
        
        --[[
            补充: 这里的count用的是默认值0,所以后续循环的话可以从0开始
            选中其他默认值也是可以的,没有一定的规则
        ]]
    end
```