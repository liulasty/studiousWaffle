# MQTT 常见面试问题

## 基础概念

1. **什么是 MQTT 协议？它的主要特点是什么？**
   - MQTT (Message Queuing Telemetry Transport) 是一种轻量级的发布/订阅消息传输协议
   - 特点：轻量级、低带宽、低功耗、适合不稳定网络、支持 QoS 等级

2. **MQTT 协议的设计目标是什么？**
   - 为低带宽、高延迟或不稳定的网络环境提供可靠的网络服务
   - 实现物联网设备的轻量级通信
   - 最小化协议开销和网络流量

3. **MQTT 协议与 HTTP 协议的主要区别是什么？**
   - MQTT 是发布/订阅模式，HTTP 是请求/响应模式
   - MQTT 头部开销小 (2 字节)，HTTP 头部大
   - MQTT 保持长连接，HTTP 通常短连接
   - MQTT 支持消息推送，HTTP 需要轮询

## 协议细节

4. **解释 MQTT 中的发布/订阅模式**
   - 发布者 (Publishers) 向主题 (Topic) 发布消息
   - 订阅者 (Subscribers) 订阅感兴趣的主题
   - 代理服务器 (Broker) 负责消息路由

5. **MQTT 中的 Topic 是什么？它的层级结构是怎样的？**
   - Topic 是消息的分类标识符
   - 层级用"/"分隔，如"home/livingroom/temperature"
   - 支持通配符：+表示单级通配， #表示多级通配

6. **MQTT 支持哪些 QoS 等级？它们有什么区别？**
   - QoS 0：最多一次 (At most once) - 消息可能丢失
   - QoS 1：至少一次 (At least once) - 消息可能重复
   - QoS 2：恰好一次 (Exactly once) - 消息确保到达且不重复

7. **解释 MQTT 的保留消息 (Retained Message) 概念**
   - Broker 会保存每个主题最后一条保留消息
   - 新订阅者会立即收到该主题的最后一条保留消息
   - 用于获取设备最新状态

8. **什么是 MQTT 的遗嘱消息 (Will Message)？**
   - 客户端在连接时设置的意外断开时发送的消息
   - 当客户端非正常断开时，Broker 会发布该消息
   - 用于通知其他客户端该设备离线

## 实现与安全

1. **MQTT 协议如何保证安全性？**
   - 支持 TLS/SSL 加密
   - 客户端认证 (用户名/密码)
   - 客户端证书认证
   - 网络隔离或 VPN

2. **常见的 MQTT Broker 有哪些？**
    - Eclipse Mosquitto
    - EMQ X
    - HiveMQ
    - AWS IoT Core
    - Azure IoT Hub
    - IBM Watson IoT Platform

3. **MQTT 协议中 CONNECT 报文包含哪些重要信息？**
    - Client ID
    - Clean Session 标志
    - 用户名/密码
    - Will Message 设置
    - Keep Alive 时间

4. **MQTT 的 Keep Alive 机制是如何工作的？**
    - 客户端指定心跳间隔 (秒)
    - 如果在此期间没有数据交换，客户端必须发送 PINGREQ
    - Broker 响应 PINGRESP
    - 超时未通信则 Broker 认为连接断开

## 高级问题

1. **MQTT 5.0 相比 3.1.1 有哪些主要改进？**
    - 会话和消息过期
    - 原因码和属性
    - 共享订阅
    - 流量控制
    - 用户属性
    - 增强的认证机制

2. **如何处理 MQTT 消息的顺序性问题？**
    - 使用 QoS 2 确保消息顺序
    - 在消息中添加序列号
    - 客户端实现消息排序逻辑
    - 单连接单线程发布

3. **MQTT 在大规模物联网场景中可能遇到哪些挑战？**
    - Broker 的性能瓶颈
    - 海量连接管理
    - 消息路由效率
    - 安全威胁增加
    - 设备异构性问题

4. **如何实现 MQTT Broker 的高可用性？**
    - 集群部署
    - 负载均衡
    - 持久化会话存储
    - 消息桥接
    - 故障自动转移

5. **解释 MQTT 的共享订阅 (Shared Subscription) 概念**
    - 多个订阅者共享一个订阅
    - Broker 以负载均衡方式分发消息
    - 格式：$share/{group}/{topic}
    - 用于工作队列模式

## 实际应用

6. **在物联网项目中，你是如何设计 MQTT 主题结构的？**
    - 考虑因素：设备类型、位置、功能、权限等
    - 示例：factory/area 1/machine 1/temperature
    - 避免过多层级
    - 考虑通配符订阅需求

7. **如何监控 MQTT 系统的健康状况？**
    - Broker 提供的监控接口
    - 系统级指标：CPU、内存、连接数
    - 消息吞吐量统计
    - 客户端活动监控
    - 异常断开报警

8. **MQTT 协议如何支持离线消息？**
    - Clean Session=false 时 Broker 保存会话状态
    - QoS 1 或 2 的消息会被存储
    - 客户端重新连接后接收未确认消息
    - 注意消息过期时间 (MQTT 5.0)