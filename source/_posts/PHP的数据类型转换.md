title: PHP的数据类型转换
date: 2019-08-17 13:18:00
categories: 技术
tags: [PHP]
---
* PHP的数据类型转换属于强制转换，允许转换的PHP数据类型有：

 -  (int)/(integer)：转换成整形
 -  (float）/(double)/(real)：转换成浮点型
 -  (string）：转换成字符串
 - （bool）/（boolean）：转换成布尔类型
 - （array）：转换成数组
 - （object）：转换成对象

* PHP数据类型有三种转换方式：
  * 在要转换的变量之前加上用括号括起来的目标类型
```c
<?php   
$num1=3.14;   
$num2=(int)$num1;   
var_dump($num1); //输出float(3.14)   
var_dump($num2); //输出int(3)   
?>  
```

  * 使用3个具体类型的转换函数，intval()、floatval()、strval()
```c
<?php   
$str="123.9abc";   
$int=intval($str);     //转换后数值：123   
$float=floatval($str); //转换后数值：123.9   
$str=strval($float);   //转换后字符串："123.9"    
?>  
```
  * 使用通用类型转换函数settype(mixed var,string type)
```c
<?php   
$num4=12.8;   
$flg=settype($num4,"int");   
var_dump($flg);  //输出bool(true)   
var_dump($num4); //输出int(12)   
?>
```
