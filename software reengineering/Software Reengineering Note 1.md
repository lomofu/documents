# Software Reengineering Note 1



## Software Architecture

### The organisational structure of a system 系统组织架构

> 反应了将需求转换为代码的关键设计决策

### Multifaceted 多方面的

- Secruity (Safety) 安全性
- Maintainability 可维护性
- Accessibility 可访问性
- Functionality 功能性

### Traceability 可追溯性

>  将架构关注点与代码联系起来的能力称为`可追溯性`
>
> * 可能随着时间的推移而恶化



## Capturing Architectures 捕捉架构信息

由于架构往往倾向于变得负责且多方面，因此：

- 一个文档往往不能真实的了解整个架构
- 需要考虑不同方面的意见 (the views of different facets)

### Dimensions 维度

由 Rozanski 和 Woods提出：

- Stakeholders: users(用户), developers(开发者), maintainers(维护人员)
- Viewpoints: functionality(功能性), information, location, etc
- Perspectives: security(安全性)，evolution(演变)，regulation(监管), etc

#### Stakeholders

| Types 类型                      | Description 描述                                         |
| ------------------------------- | -------------------------------------------------------- |
| Acquirers 收购方                | 监督系统或者产品的采购                                   |
| Assessors 评估员                | 监督系统是否符合标准和法律规定                           |
| Communicators 沟通者            | 通过文档和培训手册，向其他的利益相关者解析系统           |
| Developers 开发者               | 根据规范来构建和开发系统                                 |
| Maintainers 维护人员            | 管理已经运行的系统的演进                                 |
| Production engineers 产品工程师 | 设计，发布和管理系统构建，测试和运行时所需要的软件和硬件 |
| Suppliers 供应商                | 构建或者提供系统将要运行的软硬件或者基础设施             |
| Support staff 后勤员工          | 提供用户或者系统的支援，当系统运行时                     |
| System admins 系统管理员        | 一旦项目被发布，立刻运行                                 |
| Testers 测试人员                | 测试系统确保可用                                         |
| Users 用户                      |                                                          |











