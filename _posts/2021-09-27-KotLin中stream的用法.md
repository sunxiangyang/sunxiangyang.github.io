---
layout:     post   				    # 使用的布局（不需要改）
title:      KotLin中stream的用法				# 标题 
date:       2021-09-27 # 时间
author:     stq 					# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:		KotLin Stream						#标签
   
---
#Kotlin中Stream的一些用法

```kotlin

val list  = listOf("aa", "ac", "ab", "dd", "ee")
val list2 = listOf("aa", "aa", "ag", "dd", "dd")
//foreach
list.forEach {
    println("list中的元素:$it")
}

//stream 中迭代对象引用默认为it ,但可以重新指定 例：
list.forEach { a1 ->
    list2.forEach { a2 ->
        if (a1.equals(a2)) {
            print("重复的元素:$a1")
        }
    }
}

//filter
list.filter { it.startsWith("a") }.forEach { println("list中以a开头的元素$it") }

//groupBy 输出结果为{aa=[aa, aa], ag=[ag], dd=[dd, dd]}
println(list2.groupBy { it })


//orderBy 正序
println("正: ${list.sorted()}")


//倒序
println("倒: ${list.sortedDescending()}")
```