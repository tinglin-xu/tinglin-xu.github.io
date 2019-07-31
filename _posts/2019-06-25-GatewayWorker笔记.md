---
layout:     post
title:      GatewayWorker笔记
subtitle:   GatewayWorker笔记
date:       2019-06-25
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - PHP
---
## 序言
[GatewayWorker]: https://www.kancloud.cn/walkor/gateway-worker/642185
[GatewayWorker]使用经典的Gateway和Worker进程模型。Gateway进程负责维持客户端连接，并转发客户端的数据给BusinessWorker进程处理，BusinessWorker进程负责处理实际的业务逻辑（默认调用Events.php处理业务），并将结果推送给对应的客户端。Gateway服务和BusinessWorker服务可以分开部署在不同的服务器上，实现分布式集群。


## Register类的使用
Register类其实也是基于基础的Worker开发的。Gateway进程和BusinessWorker进程启动后分别向Register进程注册自己的通讯地址，Gateway进程和BusinessWorker通过Register进程得到通讯地址后，就可以建立起连接并通讯了。

## Gateway类的使用
Gateway类用于初始化Gateway进程。Gateway进程是暴露给客户端的让其连接的进程。所有客户端的请求都是由Gateway接收然后分发给BusinessWorker处理，同样BusinessWorker也会将要发给客户端的响应通过Gateway转发出去。

## BusinessWorker类的使用
BusinessWorker类其实也是基于基础的Worker开发的。BusinessWorker是运行业务逻辑的进程，BusinessWorker收到Gateway转发来的事件及请求时会默认调用Events.php中的onConnect onMessage onClose方法处理事件及数据，开发者正是通过实现这些回调控制业务及流程。

## Events类的回调属性
### Events::onConnect
>void Events::onConnect(string $client_id);
```swift
    /**
     * 当客户端连接时触发
     * 如果业务不需此回调可以删除onConnect
     * 
     * @param int $client_id 连接id
     */
    public static function onConnect($client_id)
    {
        Gateway::sendToClient($client_id, json_encode(array(
            'type'      => 'init',
            'client_id' => $client_id
        )));
    }
```

### Events::onMessage
>void Events::onMessage(string $client_id, mixed $recv_data);

### Events::onClose
>void Events::onClose(string $client_id);

## Lib\Gateway类提供的接口
### Gateway::joinGroup
>void Gateway::joinGroup(string $client_id, mixed $group);
```swift
    public static function onMessage($client_id, $message)
    {
        // $message = '{"type":"join","group":"xxxxx"}'
        $req_data = json_decode($message, true);
        Gateway::joinGroup($client_id, $req_data['group']);
    }
```

### Gateway::leaveGroup
>void Gateway::leaveGroup(string $client_id, mixed $group);

### Gateway::sendToGroup
>void Gateway::sendToGroup(mixed $group, string $message [, array $exclude_client_id = null [, bool $raw = false]]);

## 启动与停止
### 启动
以debug（调试）方式启动

`php start.php start`

以daemon（守护进程）方式启动

`php start.php start -d`

### 停止
`php start.php stop`

### 重启
`php start.php restart`

### 平滑重启
`php start.php reload`

### 查看状态
`php start.php status`
