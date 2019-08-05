---
layout:     post
title:      Workerman笔记
subtitle:   Workerman笔记
date:       2019-06-26
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - PHP
---
## 序言
[Workerman]: http://doc3.workerman.net/640361
[官网]: https://www.workerman.net/
[Workerman]是一款纯PHP开发的开源高性能的PHP socket 服务框架。[官网]

### 简单实例
```swift
<?php
    use Workerman\Worker;
    require_once __DIR__ . '/Workerman/Autoloader.php';
    
    // 创建一个Worker监听2345端口，使用http协议通讯
    $http_worker = new Worker("http://0.0.0.0:2345");
    
    // 启动4个进程对外提供服务
    $http_worker->count = 4;
    
    // 接收到浏览器发送的数据时回复hello world给浏览器
    $http_worker->onMessage = function($connection, $data)
    {
        // 向浏览器发送hello world
        $connection->send('hello world');
    };
    
    // 运行worker
    Worker::runAll();
```
命令行运行
`php http_test.php`

## Worker类
### 属性
#### count
>int Worker::$count

设置当前Worker实例启动多少个进程，不设置时默认为1。

#### name
>string Worker::$name

设置当前Worker实例的名称，方便运行status命令时识别进程。不设置时默认为none。

#### pidFile
>static string Worker::$pidFile

如果无特殊需要，建议不要设置此属性

此属性为全局静态属性，用来设置WorkerMan进程的pid文件路径。

#### logFile
>static string Worker::$logFile

用来指定workerman日志文件位置。

此文件记录了workerman自身相关的日志，包括启动、停止等。

### 回调属性
#### onWorkerStart
>callback Worker::$onWorkerStart

设置Worker子进程启动时的回调函数，每个子进程启动时都会执行。
#### onConnect
>callback Worker::$onConnect

当客户端与Workerman建立连接时(TCP三次握手完成后)触发的回调函数。每个连接只会触发一次`onConnect`回调。

#### onMessage
>callback Worker::$onMessage

当客户端通过连接发来数据时(Workerman收到数据时)触发的回调函数

#### onClose
>callback Worker::$onClose

当客户端连接与Workerman断开时触发的回调函数。不管连接是如何断开的，只要断开就会触发`onClose`。每个连接只会触发一次onClose。