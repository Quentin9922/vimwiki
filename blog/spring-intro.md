# Spring 简介
对于最原始的 `Spring` 而言，`Spring` 往往指的是能够对对象进行管理的框架，`IoC` 和 `AOP` 是其两大核心思想。
而在最初的 `Spring` 中，多数是使用 `xml` 文件进行配置，不过在目前看来使用 `xml` 进行配置的方式已经不再流行，
而是使用 `Java` 配置的方式 (即通过 `Java` 注解的方式进行配置)，这样也更好的与 `Spring-Boot` 相互配合。
因此本文只是介绍一些常用的 `Spring` 注解，以及 `Spring` 的一些基本概念。

# 什么是 Bean
`Bean` 可以理解为 `Spring` 容器中的对象，`Spring` 容器负责创建 `Bean`，并将 `Bean` 管理起来。

再简单一点，一个 `Bean` 就是一个 `Java` 中的对象。

## Bean 与 IoC
在不使用 `Spring` 进行开发的环境中，如果需要创建一个对象，通常是通过 `new` 关键字来创建对象。这样的话，
对象的创建和对象的销毁都是由程序员来控制的。

而 `IoC` 则指的是控制反转，即对象的创建和对象的销毁不再由程序员来控制。最简单的 `IoC` 的例子是使用
`setter` 进行依赖注入。

NOTE: 在 `Spring` 中将一个类的成员成为其依赖，所谓的依赖注入指的就是为每个成员变量设置对应的值。

通过使用 `setter` 的方式进行依赖注入的好处是可以根据用户的配置文件中的不同配置决定注入的具体对象，而如果是
使用 `new` 进行对象的创建，那么对象的创建就是固定的了。

而 `Spring` 所做的一切通过读取这个配置文件来决定创建哪个对象，以及创建对象的时候需要注入哪些对象，这样就
实现了对象的创建和对象的销毁不再由程序员来控制，也就完成了 `IoC`。

## Spring Bean 作用域介绍
在介绍 `Spring` 注解之前，首先需要了解 `Spring` 的 `Bean` 作用域，`Spring` 的 `Bean` 作用域有以下几种:

| 作用域 | 描述 |
| ---    | --- |
| `singleton` | `Spring` 的默认值，一个 `IoC` 容器只有一个 `Bean` 的实例，为所有对象提供共享的 `Bean` 实例 |
| `prototype` | 每次请求都会创建一个新的 `Bean` 实例 |
