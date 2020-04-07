# ActiveMQ
```
2020/3/9 fanfuni
```
## ActiveMQ是什么
ActiveMQ 是由 Apache 出品的一款开源消息中间件，旨在为应用程序提供高效、可扩展、稳定、安全的企业级消息通信。

面向消息的中间件（message-oriented middleware），是指利用高效可靠的消息传递机制与平台无关的数据交流，并基于数据通信来进行分布式系统的集成。
通过提供**消息传递**和**消息排队**模型在分布式环境下提供应用解耦，弹性伸缩，冗余存储、流量削峰，异步通信，数据同步等功能

---

## 为什么要引入MQ
我们举一个例子会比较好理解，假设现在有学生、老师两个对象。学生像老师询问问题，因为老师一次只能回答一个学生的问题，学生没被回答问题也不会离开，所以会导致学生的排队。这样就会导致几个问题：每一个学生都要亲自面对面问老师，耦合度很高。老师需要一直回答学生的问题，容易疲惫。学生需要被回答问题才能做其他事，效率低。

所以我们引入班长这个角色。同学按照一定的格式，将问题写在纸上。再统一由班长交给老师。这样上述的几个问题就有了改善。首先学生不会亲自问老师，实现了**解耦**。老师也不用一时间回答所有学生的问题，实现了**消减峰值流量**。同时学生也不用一直等待老师回答，实现了**异步**  MQ就在系统里实现了班长的作用。

## 安装ActiveMQ（Linux）
- 官网下包并解压
- 进入bin目录 ./activemq start (需要java环境 建议使用yum安装)启动active

默认端口号是61616

AcitveMQ使用61616 提供服务，8161提供管理控制台

## ActiveMQ控制台
- 首先要确保linux 和 Win 能互相ping通
```
systemctl status firewalld.service 查看linux防火墙状态
systemctl stop firewalld.service 关闭linux防火墙
```
确认能互相ping通后 win端浏览器输入http://linux网络地址:8161/admin（http://192.168.80.129:8161/）

## Active模式
**点对点：**
（1）每个消息只能有一个消费者，类似于1对1的关系。好比个人快递自己领自己的。

（2）消息的生产者和消费者之间没有时间上的相关性。无论消费者在生产者发送消息的时候是否处于运行状态，消费者都可以提取消息。好比我们的发送短信，发送者发送后不见得接收者会即收即看。

（3）消息被消费后队列中不会再存储，所以消费者不会消费到已经被消费掉的消息。

**发布/订阅：**
（1）生产者将消息发布到topic中，每个消息可以有多个消费者，属于1：N的关系；

（2）生产者和消费者之间有时间上的相关性。订阅某一个主题的消费者只能消费自它订阅之后发布的消息。

（3）生产者生产时，topic不保存消息它是无状态的不落地，假如无人订阅就去生产，那就是一条废消息，所以，一般先启动消费者再启动生产者。


## Java编码实现ActiveMQ通信
1. 创建一个maven工程

2. 编写pom文件
```
  <!-- https://mvnrepository.com/artifact/org.apache.activemq/activemq-all -->
        <dependency>
            <groupId>org.apache.activemq</groupId>
            <artifactId>activemq-all</artifactId>
            <version>5.15.11</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.xbean/xbean-spring -->
        <dependency>
            <groupId>org.apache.xbean</groupId>
            <artifactId>xbean-spring</artifactId>
            <version>4.16</version>
        </dependency>
```
3. 编写代码
```
public class test01 {
    //链接地址
    private static String URL = "tcp://192.168.80.129:61616";
    private static String QUEUE_NAME = "test01";

    public static void main(String[] args) throws JMSException {
        // 创建链接工厂 这里使用默认用户名/密码 admin
        ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);

        // 创建链接
        Connection connection = factory.createConnection();
        connection.start();

        // 创建会话session  这里的两个参数 一个是事务 一个是签收 我们暂时不用管
        Session session = connection.createSession(false,Session.AUTO_ACKNOWLEDGE);

        // 使用点对点模式 创建队列
        Queue queue = session.createQueue(QUEUE_NAME);

        // 创建消息生产者
        MessageProducer producer = session.createProducer(queue);

        //创建消息
        TextMessage textMessage = session.createTextMessage("Hello,ActiveMQ");

        //传入消息
        producer.send(textMessage);

        // 关闭资源
        producer.close();
        session.close();
        connection.close();

        //打印结束信息
        System.out.println("消息传入完成");

    }
}
```
4. 上述是消息生产者，接着我们创建消费者案例
```
public class test_consumer {
    //链接地址
    private static String URL = "tcp://192.168.80.129:61616";
    private static String QUEUE_NAME = "test01";

    public static void main(String[] args) throws JMSException {
        // 创建链接工厂 这里使用默认用户名/密码 admin
        ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);

        // 创建链接
        Connection connection = factory.createConnection();
        connection.start();

        // 创建会话session  这里的两个参数 一个是事务 一个是签收 我们暂时不用管
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        // 使用点对点模式 创建队列
        Queue queue = session.createQueue(QUEUE_NAME);

        // 创建消费者
        MessageConsumer consumer = session.createConsumer(queue);

        try {
            // 获得消息，因为我们发送的是TextMessage类型 所以我们获取也是TextMessage类型。同步阻塞
            TextMessage receive = (TextMessage) consumer.receive();

            //
            if(receive != null){
                System.out.println("消费者接收到消息"+receive.getText());
            }else{
                System.out.println("消费者没有消息可消费");
            }

        }finally{
            consumer.close();
            session.close();
            connection.close();


        }
    }
}
```
这里我们可以法相 我们使用的是receive获得消息，如果不设定时间，就会一直等待，这样很明显是不符合效率要求的，现在我们用另一种方式来获得消息
```
public class test_consumer {
    //链接地址
    private static String URL = "tcp://192.168.80.129:61616";
    private static String QUEUE_NAME = "test01";

    public static void main(String[] args) throws JMSException {
        // 创建链接工厂 这里使用默认用户名/密码 admin
        ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);

        // 创建链接
        Connection connection = factory.createConnection();
        connection.start();

        // 创建会话session  这里的两个参数 一个是事务 一个是签收 我们暂时不用管
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        // 使用点对点模式 创建队列
        Queue queue = session.createQueue(QUEUE_NAME);

        // 创建消费者
        MessageConsumer consumer = session.createConsumer(queue);
        
        //使用监听的模式来获取消息 异步非阻塞
        consumer.setMessageListener(new MessageListener() {
            @Override
            public void onMessage(Message message) {
                if(message != null && message instanceof TextMessage){
                    TextMessage tmessage = (TextMessage) message;
                    try {
                        System.out.println("消费者接收到消息"+ tmessage.getText());
                    } catch (JMSException e) {
                        e.printStackTrace();
                    }
                }else{
                    System.out.println("消费者没有消息可消费");
                }

            }
        });
            // 持续监听
            System.in.read();
            consumer.close();
            session.close();
            connection.close();

    }
}
```
我们上面使用的是queue，接下来我们使用topic模式，其实topic十分简单只需要改几行代码便可以
```
public class test_productor {
    //链接地址
    private static String URL = "tcp://192.168.80.129:61616";
    private static String TOPIC_NAME = "topic";

    public static void main(String[] args) throws JMSException {
        // 创建链接工厂 这里使用默认用户名/密码 admin
        ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);

        // 创建链接
        Connection connection = factory.createConnection();
        connection.start();

        // 创建会话session  这里的两个参数 一个是事务 一个是签收 我们暂时不用管
        Session session = connection.createSession(false,Session.AUTO_ACKNOWLEDGE);

        // 使用点对点模式 创建队列
        Topic topic = session.createTopic(TOPIC_NAME);

        // 创建消息生产者
        MessageProducer producer = session.createProducer(topic);

        //创建消息
        TextMessage textMessage = session.createTextMessage("Hello,ActiveMQ");

        //传入消息
        producer.send(textMessage);

        // 关闭资源
        producer.close();
        session.close();
        connection.close();

        //打印结束信息
        System.out.println("消息传入完成");

    }
}

```
```
public class test_consumer {
    //链接地址
    private static String URL = "tcp://192.168.80.129:61616";
    private static String TOPIC_NAME = "topic";

    public static void main(String[] args) throws JMSException, IOException {
        // 创建链接工厂 这里使用默认用户名/密码 admin
        ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);

        // 创建链接
        Connection connection = factory.createConnection();
        connection.start();

        // 创建会话session  这里的两个参数 一个是事务 一个是签收 我们暂时不用管
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        // 使用点对点模式 创建队列
        Topic topic = session.createTopic(TOPIC_NAME);

        // 创建消费者
        MessageConsumer consumer = session.createConsumer(topic);

        //使用监听的模式来获取消息
        consumer.setMessageListener(new MessageListener() {
            @Override
            public void onMessage(Message message) {
                if(message != null && message instanceof TextMessage){
                    TextMessage tmessage = (TextMessage) message;
                    try {
                        System.out.println("消费者接收到消息"+ tmessage.getText());
                    } catch (JMSException e) {
                        e.printStackTrace();
                    }
                }else{
                    System.out.println("消费者没有消息可消费");
                }

            }
        });
            System.in.read();
            consumer.close();
            session.close();
            connection.close();

    }
}
```
## JMS
### 是什么
(JMS)Java Message Service Java消息服务

### JMS组成结构和特点
 JMS Provider 实现JMS接口和规范的消息中间件，也就是我们说的MQ服务器

 JMS Producer 消息生产者，创建和发送JMS消息的客户端应用

 JMS Consumer 消息消费者，接收和处理JMS消息的客户端应用

 JMS Message:
- 消息头
```
JMSDestination 消息发送的目的地，主要是指Queue和Topic
JMSDeliveryMode 持久模式和非持久模式
JMSExpiration 设定消息过期时间，默认是永不过期
JMSPriority 消息优先级0~9 0~4是普通消息 5~9是加急消息 不要求严格按照优先级顺序到达，但要求加急消息先于普通消息到达
JMSMessageID 唯一标识每个消息的标识由MQ产生
```
- 消息属性
```
封装具体的消息数据
有5种消息格式{
    1. TextMessage 字符串消息，包含一个String
    2. MapMessage  一个Map类型的消息，Key为String类型，而值为Java基本类型
    3. BytesMessage 二进制数组消息，包含一个byte[]
    4. StreamMessage Java数据流消息，用标准流操作来顺序填充和读取
    5.ObjectMessage 对象消息，包含一个可序列化的Java对象
}
发送和接受的消息类型必须一致
```
- 消息体
```
如果需要除消息字段以外的值，那么可以使用消息属性
```
### JMS的可靠性
#### PERSISTENT：持久性
Queue默认是持久化存储

**producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);**

非持久化：服务器宕机，消息不存储

**producer.setDeliveryMode(DeliveryMode.PERSISTENT);**

持久化：服务器宕机，消息依然存在

##### 持久的Queue
```
    // 使用点对点模式 创建队列
    Queue queue = session.createQueue(QUEUE_NAME);
    // 创建消息生产者
    MessageProducer producer = session.createProducer(queue);
    producer.setDeliveryMode(DeliveryMode.PERSISTENT);
```
##### 持久的Topic
```
public class test_productor {
    //链接地址
    private static String URL = "tcp://192.168.80.129:61616";
    private static String TOPIC_NAME = "topic";

    public static void main(String[] args) throws JMSException {
        // 创建链接工厂 这里使用默认用户名/密码 admin
        ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);

        // 创建链接
        Connection connection = factory.createConnection();
        connection.start();

        // 创建会话session  这里的两个参数 一个是事务 一个是签收 我们暂时不用管
        Session session = connection.createSession(false,Session.AUTO_ACKNOWLEDGE);

        // 使用点对点模式 创建队列
        Topic topic = session.createTopic(TOPIC_NAME);

        // 创建消息生产者
        MessageProducer producer = session.createProducer(topic);
        producer.setDeliveryMode(DeliveryMode.PERSISTENT);
        connection.start();

        //创建消息
        TextMessage textMessage = session.createTextMessage("Hello,ActiveMQ");
        
        //传入消息
        producer.send(textMessage);

        // 关闭资源
        producer.close();
        session.close();
        connection.close();

        //打印结束信息
        System.out.println("消息传入完成");

    }
}
```
```
public class test_consumer {
    //链接地址
    private static String URL = "tcp://192.168.80.129:61616";
    private static String TOPIC_NAME = "topic";

    public static void main(String[] args) throws JMSException, IOException {
        // 创建链接工厂 这里使用默认用户名/密码 admin
        ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);

        // 创建链接
        Connection connection = factory.createConnection();
       connection.setClientID("zh");

        // 创建会话session  这里的两个参数 一个是事务 一个是签收 我们暂时不用管
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        // 使用点对点模式 创建队列
        Topic topic = session.createTopic(TOPIC_NAME);

        TopicSubscriber topicSubscriber = session.createDurableSubscriber(topic, "我是张三");

        connection.start();
        topicSubscriber.setMessageListener(message -> {
            if (message instanceof TextMessage) {
                TextMessage textMessage = (TextMessage) message;
                try {
                    System.out.println("收到的持久化订阅消息: " + textMessage.getText());
                } catch (JMSException e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
```
持久化topic以后 生产者在订阅者下线的情况下发出的消息，会在订阅者上线后投递
这有点类似于微信号订阅

#### Transaction：事务
事务主要在以下行被确定

Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

如果为true session需要commit后 消息才会真正的发布，这方便回滚

#### Acknowledge：签收
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

签收的方式一般有三种：
- 1、AUTO_ACKNOWLEDGE  自动签收
- 2、CLIENT_ACKNOWLEDGE 客户端确定签收
- 3、DUPS_OK_ACKNOWLEDGE 可重复签收

如果是第二种 需要调用上文种textMessage的ackownledge方法才能签收

## ActiveMQ的Broker
Broker就相当于一个ActiveMQ服务器实例

Broker其实就是实现了用代码的形式启动ActiveMQ将MQ嵌入到Java代码中，以便随时用随时启动，在用的时候再去启动这样能节省了资源，也保证了可用性。

## ActiveMQ的传输协议
Active支持多种传输协议，例如VM，AMQP、MQTT、TCP、NIO等

在文件 /conf/activemq.xml 文件中进行配置

## ActiveMQ的高可用和持久化
为了避免意外宕机以后丢失信息，需要做到重启后可以恢复消息队列，消息系统一半都会采用持久化机制。

ActiveMQ的消息持久化机制有JDBC，AMQ，KahaDB和LevelDB，无论使用哪种持久化方式，消息的存储逻辑都是一致的。
 
就是在发送者将消息发送出去后，消息中心首先将消息存储到本地数据文件、内存数据库或者远程数据库等。再试图将消息发给接收者，成功则将消息从存储中删除，失败则继续尝试尝试发送。
 
消息中心启动以后，要先检查指定的存储位置是否有未成功发送的消息，如果有，则会先把存储位置中的消息发出去。

### 类型
- AMQ Message Store ：基于日志文件的存储方式，写入速度快，容易恢复。现在已经不使用了
- KahaDB：基于日志文件，是现在的默认存储方式
```
KahaDB是目前默认的存储方式，可用于任何场景，提高了性能和恢复能力。
消息存储使用一个事务日志和仅仅用一个索引文件来存储它所有的地址。
KahaDB是一个专门针对消息持久化的解决方案，它对典型的消息使用模型进行了优化。
数据被追加到/data的logs中。当不再需要log文件中的数据的时候，log文件会被丢弃。
```
```
KahaDB存储原理
KahaDB在消息保存的目录中有4类文件和一个lock，跟ActiveMQ的其他几种文件存储引擎相比，这就非常简洁了。
db-number.log
KahaDB存储消息到预定大小的数据纪录文件中，文件名为db-number.log。当数据文件已满时，一个新的文件会随之创建，number数值也会随之递增。当不再有引用到数据文件中的任何消息时，文件会被删除或者归档。
db.data
 该文件包含了持久化的BTree索引，索引了消息数据记录中的消息，它是消息的索引文件，本质上是B-Tree（B树），使用B-Tree作为索引指向db-number。log里面存储消息。
db.free
当问当前db.data文件里面哪些页面是空闲的，文件具体内容是所有空闲页的ID
db.redo
用来进行消息恢复，如果KahaDB消息存储再强制退出后启动，用于恢复BTree索引。
 lock
文件锁，表示当前kahadb独写权限的broker。
```
- JDBC
- LEvelDB
```
这种文件系统是从ActiveMQ5.8之后引进的，它和KahaDB非常相似，也是基于文件的本地数据库存储形式，但是它提供比KahaDB更快的持久性。
但它不使用自定义B-Tree实现来索引独写日志，而是使用基于LevelDB的索引
```
- JDBC Message Store with ActiveMQ Jounal

### JDBC 存储消息
- 添加mysql数据库的驱动包到lib文件夹
```
wget   -P    保存目录   https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.17/mysql-connector-java-8.0.17.jar
```
- jdbcPersistenceAdapter配置
```
         <persistenceAdapter> 
             <jdbcPersistenceAdapter dataSource="#mysql-ds" /> 
         </persistenceAdapter>

```
- 数据库连接池配置
```
在active.xml的broker之后进行配置
  <bean id="mysql-ds" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close"> 
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/> 
    <property name="url" value="jdbc:mysql://192.168.10.132:3306/activemq?relaxAutoCommit=true"/> 
    <property name="username" value="root"/> 
    <property name="password" value="123456"/> 
    <property name="poolPreparedStatements" value="true"/> 
  </bean>

```
- 建库SQL和创表说明
```
首先一个对应的activemq数据库
ACTIVEMQ_MSGS Queue及Topic的信息一般存储在这里面
ACTIVEMQ_ACKS Topic 持久化订阅者一般储存在这里面
ACTIVEMQ_LOCK 一般在集群中使用，只有一个Broker可以获得信息
```
- 代码运行
```
一定要开启持久化
messageProducer.setDeliveryMode(DeliveryMode.PERSISTENT);
```
## 异步投递
对于一个Slow Consumer,使用同步发送消息可能出现Producer堵塞的情况,慢消费者适合使用异步发送。

ActiveMQ默认使用异步发送的模式

使用回调函数来判断是否投递成功

## 延迟投递和定时投递
### 四大属性
AMQ_SCHEDULED_DELAY 延迟投递时间
AMQ_SCHEDULED_PERIOD 重复投递的时间间隔
AMQ_SCHEDULED_REPEAT 重复投递次数
AMQ_SCHEDULED_CRON Cron表达式

## 开启
- 要在activemq.xml中配置schedulerSupport属性为true

```
public class Producer_延迟投递 {
    private static final String ACTIVEMQ_URL = "tcp://192.168.80.129:61616";
    private static final String ACTIVEMQ_QUEUE_NAME = "Queue-延迟投递";

    public static void main(String[] args) throws JMSException {
        ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory();
        activeMQConnectionFactory.setBrokerURL(ACTIVEMQ_URL);
        Connection connection = activeMQConnectionFactory.createConnection();
        connection.start();
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        Queue queue = session.createQueue(ACTIVEMQ_QUEUE_NAME);
        //向上转型到ActiveMQMessageProducer
        MessageProducer messageProducer = session.createProducer(queue);
        long delay = 3 * 1000;      //延迟投递的时间
        long period = 4 * 1000;     //每次投递的时间间隔
        int repeat = 5;                     //投递的次数

        for (int i = 0; i < 3; i++) {
            TextMessage textMessage = session.createTextMessage("message-延时投递" + i);
            //给消息设置属性以便MQ服务器读取到这些信息,好做对应的处理
            textMessage.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_DELAY, delay);
            textMessage.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_PERIOD, period);
            textMessage.setIntProperty(ScheduledMessage.AMQ_SCHEDULED_REPEAT, repeat);
            messageProducer.send(textMessage);
        }
        messageProducer.close();
        session.close();
        connection.close();
    }
}

```
## 消费重试机制
### 那些情况会引起消息重发
- Client用了transactions且在session中调用了rollback（）
- Client用了transactions且在调用commit（）之前没有关闭或者没有commit
- 

## 死信队列
即一条消息再被重发了很多次（默认6次）,将会被移入死信队列，开发人员可在Queue中查看出路出错信息。

一般将所有DeadLetter保存在一个共享队列中，这是ActiveMQ broker端默认的策略

可以通过设置文件在deadLetterQueu属性来设置
对于Queue 默认的死信通道前缀是“ActiveMQ.DLQ.Queue”
    Topic                  则是“Active.DLQ.Topic”
    
过期消息不发送到死信队列直接删除 可以通过配置processExpired来配置

默认情况，Activemq不会把非持久死消息发送到死信队列中。processNonPersistent来设置是否放入死信队列

## 保证消息不被重复消费
如果消息是数据库的插入操作，可以给这个消息设置唯一主键，即使重复消费，也不会出现数据库脏数据。

或者使用一个第三方来记录，例如redis，给消息分配抑恶个全局id，只要消费过就存入redis中，那消费者开始消费前，先去redis中查询有没有消费记录即可。