# Spring Boot 集成 MQTT 的细节

## 1. 添加依赖

首先需要在 `pom.xml` 中添加 MQTT 相关依赖：

```xml
<dependency>
    <groupId>org.springframework.integration</groupId>
    <artifactId>spring-integration-mqtt</artifactId>
</dependency>
<dependency>
    <groupId>org.eclipse.paho</groupId>
    <artifactId>org.eclipse.paho.client.mqttv3</artifactId>
    <version>1.2.5</version>
</dependency>
```

## 2. 配置 MQTT 连接

在 `application.properties` 或 `application.yml` 中配置 MQTT 连接参数：

```properties
# MQTT配置
mqtt.broker-url=tcp://localhost:1883
mqtt.client-id=spring-boot-client
mqtt.username=admin
mqtt.password=password
mqtt.default-topic=test/topic
mqtt.completion-timeout=5000
mqtt.keep-alive-interval=60
mqtt.clean-session=true
```

## 3. 创建 MQTT 配置类

```java
@Configuration
public class MqttConfig {
    
    @Value("${mqtt.broker-url}")
    private String brokerUrl;
    
    @Value("${mqtt.client-id}")
    private String clientId;
    
    @Value("${mqtt.username}")
    private String username;
    
    @Value("${mqtt.password}")
    private String password;
    
    @Value("${mqtt.keep-alive-interval}")
    private int keepAliveInterval;
    
    @Bean
    public MqttConnectOptions mqttConnectOptions() {
        MqttConnectOptions options = new MqttConnectOptions();
        options.setServerURIs(new String[]{brokerUrl});
        options.setUserName(username);
        options.setPassword(password.toCharArray());
        options.setCleanSession(true);
        options.setAutomaticReconnect(true);
        options.setConnectionTimeout(10);
        options.setKeepAliveInterval(keepAliveInterval);
        return options;
    }
    
    @Bean
    public MqttPahoClientFactory mqttClientFactory() {
        DefaultMqttPahoClientFactory factory = new DefaultMqttPahoClientFactory();
        factory.setConnectionOptions(mqttConnectOptions());
        return factory;
    }
}
```

## 4. 消息发布配置

```java
@Configuration
public class MqttOutboundConfig {
    
    @Autowired
    private MqttPahoClientFactory mqttClientFactory;
    
    @Value("${mqtt.default-topic}")
    private String defaultTopic;
    
    @Bean
    @ServiceActivator(inputChannel = "mqttOutboundChannel")
    public MessageHandler mqttOutbound() {
        MqttPahoMessageHandler messageHandler = 
            new MqttPahoMessageHandler("publisherClient", mqttClientFactory);
        messageHandler.setAsync(true);
        messageHandler.setDefaultTopic(defaultTopic);
        messageHandler.setDefaultQos(1);
        return messageHandler;
    }
    
    @Bean
    public MessageChannel mqttOutboundChannel() {
        return new DirectChannel();
    }
}
```

## 5. 消息订阅配置

```java
@Configuration
public class MqttInboundConfig {
    
    @Autowired
    private MqttPahoClientFactory mqttClientFactory;
    
    @Value("${mqtt.default-topic}")
    private String[] topics;
    
    @Bean
    public MessageProducer inbound() {
        MqttPahoMessageDrivenChannelAdapter adapter = 
            new MqttPahoMessageDrivenChannelAdapter("subscriberClient", 
                mqttClientFactory, topics);
        adapter.setCompletionTimeout(5000);
        adapter.setQos(1);
        adapter.setOutputChannel(mqttInputChannel());
        return adapter;
    }
    
    @Bean
    public MessageChannel mqttInputChannel() {
        return new DirectChannel();
    }
    
    @Bean
    @ServiceActivator(inputChannel = "mqttInputChannel")
    public MessageHandler handler() {
        return message -> {
            String topic = (String) message.getHeaders().get("mqtt_receivedTopic");
            String payload = (String) message.getPayload();
            System.out.println("Received message from topic: " + topic + ", payload: " + payload);
            // 处理消息的业务逻辑
        };
    }
}
```

## 6. 使用 MQTT 服务

### 消息发布服务

```java
@Service
public class MqttPublisherService {
    
    @Autowired
    private MessageChannel mqttOutboundChannel;
    
    public void sendMessage(String topic, String message) {
        mqttOutboundChannel.send(MessageBuilder.withPayload(message)
            .setHeader(MqttHeaders.TOPIC, topic)
            .setHeader(MqttHeaders.QOS, 1)
            .build());
    }
    
    public void sendMessage(String message) {
        mqttOutboundChannel.send(MessageBuilder.withPayload(message).build());
    }
}
```

### 消息订阅服务

消息订阅已经在配置类中通过 `@ServiceActivator` 注解配置了处理器，会自动处理收到的消息。

## 7. 高级配置

### 自定义消息转换器

```java
@Bean
public MessageConverter messageConverter() {
    return new DefaultPahoMessageConverter();
}
```

### 配置多个主题订阅

```java
@Bean
public MessageProducer inbound() {
    MqttPahoMessageDrivenChannelAdapter adapter = 
        new MqttPahoMessageDrivenChannelAdapter("subscriberClient", 
            mqttClientFactory, "topic1", "topic2", "topic3");
    // 其他配置...
    return adapter;
}
```

### 通配符订阅

```java
@Bean
public MessageProducer inbound() {
    MqttPahoMessageDrivenChannelAdapter adapter = 
        new MqttPahoMessageDrivenChannelAdapter("subscriberClient", 
            mqttClientFactory, "sensors/+/temperature");
    // 其他配置...
    return adapter;
}
```

## 8. 异常处理

```java
@Bean
public MessageProducer inbound() {
    MqttPahoMessageDrivenChannelAdapter adapter = 
        new MqttPahoMessageDrivenChannelAdapter(...);
    adapter.setErrorChannel(errorChannel());
    return adapter;
}

@Bean
public MessageChannel errorChannel() {
    return new DirectChannel();
}

@Bean
@ServiceActivator(inputChannel = "errorChannel")
public MessageHandler errorHandler() {
    return message -> {
        Throwable cause = (Throwable) message.getPayload();
        // 处理MQTT连接或消息处理中的异常
        System.err.println("MQTT Error: " + cause.getMessage());
    };
}
```

## 9. 集成测试

```java
@SpringBootTest
public class MqttIntegrationTest {
    
    @Autowired
    private MqttPublisherService publisherService;
    
    @Test
    public void testMqttCommunication() throws InterruptedException {
        String testTopic = "test/topic";
        String testMessage = "Hello MQTT from Spring Boot";
        
        publisherService.sendMessage(testTopic, testMessage);
        
        // 等待消息被处理
        Thread.sleep(2000);
        
        // 可以通过日志或其他方式验证消息是否被正确接收和处理
    }
}
```

## 10. 生产环境注意事项

1. **客户端 ID 管理**：确保每个实例有唯一的客户端 ID，避免冲突
2. **连接重试**：配置自动重连机制
3. **QoS 选择**：根据业务需求选择合适的 QoS 级别
4. **资源清理**：确保应用关闭时正确断开 MQTT 连接
5. **性能监控**：监控消息吞吐量和延迟
6. **安全配置**：生产环境应使用 TLS 加密连接
7. **消息积压**：处理消息积压情况，避免内存溢出

通过以上配置，Spring Boot 应用可以完整地实现 MQTT 的发布订阅功能，并具有良好的可扩展性和可维护性。