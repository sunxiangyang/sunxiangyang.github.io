---
layout:     post   				    # 使用的布局（不需要改）
title:      KotLin中stream的用法				# 标题 
date:       2021-09-27 # 时间
author:     stq 					# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:		KotLin						#标签
   
---
#Kotlin中Stream的一些用法
```kotlin
val list = listOf("aa","aa", "ab","dd",'ee')
//foreach
list.forEach{
    println(it)
}

//filter
 val filterResult = list.filter { it.startWith("a") }
 
//groupBy
val groupByResult = list.groupBy { it.toString() }
```