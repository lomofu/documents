# Computer Security and Forensics 01

> Introduction, Motivation, Fundamentals and Access Control



## SQL Injection Attacks SQL注入攻击

### **Simplified Syntax** 

> 例如在一个典型的登录界面进行用户登录，

- **PasswordDatabase(userId) EQ randomstring OR (1==1)**

password=‘randomstring or 1=1’

```sql
select * from users where password='randomstring' or 1=1
```

- **注释注入**

username=123’or 1=1 #

password=123’or 1=1 #

```sql
select * from users where username='123' or 1=1 #' and password='123' or 1=1 #'
```



### Parse Trees

- Expected Parse Tree

<img src="img/01/image-20220222011152985.png" alt="image-20220222011152985"  />

- Actual Parse Tree

![image-20220222011240417](img/01/image-20220222011240417.png)



## Secure Design Flow 安全设计流程

![image-20220222184007917](img/01/image-20220222184007917.png)

> 上图展示了一个大致遵循安全设计和软件设计生命周期的的模块

一个安全设计的流程大概包含以下：

### Foundations and Security Technologies 基础和安全技术

- **Identification** 识别，**Authentication** 认证，**Authorization** 授权，**Access Control** 访问控制
- **Cryptography** 加密
- **Security Protocols** 安全协议

### **Building Secure Systems** 构建安全系统

- **Analysing Security Protocols** 分析安全协议
- **Application Security and Secure Programming**  应用程序安全和安全编程
- **Security Testing** 安全测试

### Secure Operations Response and Forensics 安全操作响应和取证

- **Secure Operations** 安全操作
- **Security Response** 安全响应
- **Forensics** 取证  

## Cyber Security 网络安全

> Cyber security refers to the protection of information systems (hardware, software and associated infrastructure), the data on them, and the services they provide, from unauthorised access, harm or misuse.

网络安全指的是保护信息系统（软硬件，相关的基础设施），系统上的数据，提供的服务不被未授权访问，伤害或者滥用。

## Typical Resources - Data 数据资源

- **Data At Rest** 静态数据：保护存储在数据库里的数据

>  引入加密的数据库

- **Data In Transit** 传输中的数据：保护正在传输的数据（例如：上传信息）

> 使用安全信道　`secure channels` （例如 `TLS 协议`）

- **Data In Execution** 执行中的数据：保护正在操作中的数据（例如： 比较）

> 　引入[同态加密](https://zh.wikipedia.org/wiki/%E5%90%8C%E6%80%81%E5%8A%A0%E5%AF%86)　`homomorphic encryption`

## CIA: Confidentiality, Integrity and Availability

**`计算机安全的关键目标`**

<img src="img/01/image-20220222231445483.png" alt="image-20220222231445483" style="zoom:67%;" />

### Confidentiality 保密性

对信息的访问和公开进行授权限制，阻止信息泄露给未授权方

### Integrity 完整性

防止出现信息的不恰当修改和破坏

### Availability 可用性

确保信息系统能及时可靠地访问和使用

## Identity and AAA

> 为了能够判断一个 **主体 `subject`** 是不是 **授权方`authorized party` **，并且可以访问一个 **资源 `resource` **

### Identification 鉴别

与鉴别一个实体相关联

### Authentication 认证

>  检验一些东西的合法性 （通常是**系统实体声明的身份 `the claimed identity of a system entity`**）

**Types of authentication 认证的类型**

- **Knowledge** 认知 (Something that you know) :  Password, PIN

- **Ownership** 所有权 (Something that you own)：Smartcard, Keys
  - **Hardware Tokens**
    - Chip Cards
    - One-time password Generators

- **Characteristics** 特征（Something that you are）: Fingerprints, Weight distribution on a chair 
  - **Biometrics**
    - Fingerprints
    - Retinal Scans 视网膜扫描
    - Facial Recognition 面部识别

### Authorization 授权

授予或者拒绝一个系统实体的访问资源的权限

### Accountability 问责制

要求一个实体的 **行为`action`**可以唯一地追踪到该实体

## Access control 访问控制

Access control = authentication + authorization

### Abstraction 抽象概念

<img src="img/01/image-20220223003515905.png" alt="image-20220223003515905" style="zoom:67%;" />



TAGS: **主体 `Subject`**, **目标 `Object`**, **拥有者 `Owners`**  **策略`Policies`**, **主体特权`Subject Privileges`**

- **主体 `Subject`** 想要使用 **目标 `Object`**
- **所有者 `Owners`** 拥有 **目标 `Object`**
- **拥有者 `Owners`**  使用 目标 `Object` 定义 指定 **主体特权`Subject Privileges`** 的 **`Policies` 策略**
- **拥有者 `Owners`**  创建 **实现`Implementations`** 来允许  **主体 `Subject`** 来自我认证

### Access control phases 访问控制的阶段

1. **Policy Definition Phase 策略定义阶段**：**拥有者 `Owners`**  指定谁可以访问哪个资源。
   1.  身份注册 Identity registration， 授权 authorization 会在这里发生
2. **Policy Enforcement Phase 策略执行阶段**
   1. Subject 尝试访问资源。不论结果是通过或者被拒绝，他的行为都会被记录下来

#### Policy Enforcement Phase 策略执行阶段

访问控制大致可以分为以下两类：

|                   | Physical access controls 物理访问控制          | Logical access controls 逻辑访问控制                         |
| ----------------- | :--------------------------------------------- | :----------------------------------------------------------- |
| Control Resources | **Physical resources**:  a PC, a paper records | **Electronic systems and resources**: databases, electronic records |
| Controlled by     | Facility manager 设备管理员                    | System administrators 系统管理员                             |
| Example           | Keys to office，Smart-cards                    | - Username and passwords<br />- Access to other accounts, such as email addresses. |

#### Policies vs Models 策略vs模型

|          | Security policy                                              | Security model                                               |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Define   | What is allowed ( and / or forbidden)<br />定义了什么可以被访问或者被拒绝 | Representation of a class of systems (and their behaviour)<br />一类系统（及其行为）的表示 |
| Features | - Analogous to a set of laws (类似一个法规集)<br />- Defined in terms of rules and / or requirements 被定义为规则或者需求的形式 | - 突出显示被选定的抽象级别上的安全功能<br />- 提供制定具体策略的词汇表 |

### Access Control Models 访问控制模型

> Focus on **`authorization`**
>
> - 规定了谁被允许做什么（Permissions）
> - 如何更新和改变权限

Simple access control model 就是一个 **Subject x Object x Request** 的关系

#### Access Control Matrix Model 访问控制矩阵模型

> 基于 `Subjects` 对 `Objects` 的 `Privileges` 思想

- **Subjects**: Users, processes, agents, groups, …
- **Objects**: Data, memory banks, other processes, files, …
- **Privileges**: Right to read, write, modtify, …

|     Access Control in the **abstract**     |         Access Control implementation          |
| :----------------------------------------: | :--------------------------------------------: |
|                   Model                    |                   Mechanism                    |
| 当我们抽象地提到访问控制时，我们指的是模型 | 当我们提到访问控制的实现时，我们指的是一种机制 |

##### Protection State 保护状态

> A protection state ( relative to a set of privileges $P$ ) is a triple ($S$, $O$, $M$)
>
> 保护状态（ 相当于一组权限 $P$ ) 是一个三元组 ($S$, $O$, $M$)

- $S$: 表示一组现有的 `Subjects`
- $O$: 表示一组现有的 `Objects`
- $M$: 一个访问控制矩阵
  - 每一个 `Privilege` 特权表示为 $$ (S,O) \in O$$
  - 访问控制矩阵定义了一个关系: $$ S \times O \times P $$

| Subject $$S$$ | Object $$O$$           | Privilege $$P$$ |
| ------------- | ---------------------- | --------------- |
| sspielberg    | indy_last_crusade.txt  | Read            |
| glucas        | indy_last_crusade.txt  | Write           |
| sspielberg    | indy_crystal_skull.txt | Read            |

#### Examples of Access Control Models 一些访问控制的模型

> 因为不同的环境需要不同位置的 `object` 访问，因此需要定义一个更普遍化的一些 models 的集合来描述权限是如何被授予的

##### Discretionary access control (DAC) 自主访问控制

> The `owner ` of the `object` decides which `subjects` has which `privileges`

是一种软件机制，用来控制用户对文件和目录的访问。 DAC 允许所有者根据自己的判断来设置文件和目录的保护。

##### Mandatory access control (MAC) 强制访问控制

> System-wide polices define `subject` access to `objects`, and `owners` cannot change this

是一种基于标签关系的，由**系统强制执行**的访问控制机制。拥有者不可以改变它

##### Role Based Access Control （RBAC）基于角色的访问控制

