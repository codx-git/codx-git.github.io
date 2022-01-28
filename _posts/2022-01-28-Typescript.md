---
layout: post
title: "Typescript学习笔记"
date: 2021-01-28 
description: "TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6,由微软开发的自由和开源的编程语言。TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上"
tag: Typescript,TS
---   

## TypeScript是强类型语言，JavaScript是弱类型语言

1. 强类型语言的特点：变量、形参、函数都要声明类型
2. Ts对属性和方法成员定义三种访问修饰符：public、private、protected<br>
    - 访问修饰符的特殊用法
    ```typescript
            //方法一：
            class Emp{
                private age:number
                construcor(age){
                    this.age = age
                }
            }
            //方法二：
            class Emp{
                construcor(ptivate age:number){
                }
            }
    ```
3. 面对对象编程核心概念——class(类)和interface(接口)
    - 接口：是一种特殊的类，规范“要求某个class必须具备XXX方法”，如管道类必须提供transform方法
    ```typescript
        //接口
        interface Runnable{
            start()
            stop()
        }
        //继承接口的类来实现方法
        class Car implements Runnable{
            start(){}
            stop(){}
        }
    ```
4. 装饰器,如`@Compenent`、`@Pipe`等