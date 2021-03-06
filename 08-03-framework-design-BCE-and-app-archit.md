
## BCE方法
### 识别类
- Boundary：与外部 Actor 交互的类。包括 UI、外部系统接口
- Controller：处理外部事件，实现控制流的类。通常是一个子系统、一个用例一个类
- Entity：领域对象或数据实体

### 静态设计
- Boundary 对象：表示参与者与系统之间进行的交互以及信息交流
- Controller 对象：一个用例具有的事件流的控制行为
	- Boundary 发生的用户事件消息，皆是 Controller 的方法。
	- 以下都是不正确的交互：
		- UI 有箭头指向模型
		- 模型有箭头指向控制器。或控制器有除创建之外的箭头指向界面
		- 无论安卓或web，控制器都设计为多用户，即控制器不包含状态变量
	- 不能考虑多线程，使用多线程更新界面。要使用回调函数（消息）机制完成异步操作。
- Entity 对象：表示数据库中存储的信息及相关行为
	- 从 Domain Model 获取属性
	- 如果模型之间存在关联，请将关系转化为合适的实现（关联属性）
	- 将 Controller 消息转化为方法

## 映射
不同架构和框架映射机制不一样，以传统 java web 为例：

- 表示层
  - M （ViewModel） 与 用例涉及的 Entities 数据一致， 放入 models 或 pojos 或 entities 包
  - V （View） 就是视图模板，或部分视图模板（如查询表单）
  - C （Controller） 与 Controller 对象一致，处理一类 UI 事件
  - 如果模板数据 是 Entities 数据的投影、join，设计为 dto（data transfer object） 对象，放入 dtos 包
  - 如果一个表达或数据需要在多个界面共享，可设计为应用程序范围或 Session 范围变量，如输入表单，一般放入 form 包
  - 将常用数据验证方法、翻页等，应写成统一的实用程序包 utilities
  - 将常用数据转换（序列化、反序列化、格式化）类，放在 converter 包中
- 业务层（services 包）
  - Entities 的方法
  - 获取关联对象的方法
- 数据层（daos 或 repos 包）
  - Entities 的 CRUD 方法

## 框架目录设计与逻辑架构
- **框架目录设计**：框架目录是为了服务开发人员，而非限制开发人员。 框架本身并不关注业务，至少不受业务影响或制约。框架就是要从具体的业务功能中，分离出能覆盖所有业务的设计、实现与组成，并使得各业务功能从开发实现的角度，变得解耦合、可重组、易维护。 不同的架构方法论，会将架构分为不同视图，每个视图侧重某一个方面、领域的问题。
- **逻辑架构**：其关注的是功能，包含用户直接可见的功能，还有系统中隐含的功能。或者更加通俗来描述，逻辑架构更偏向我们日常所理解的“分层”，把一个项目分为“表示层、业务逻辑层、数据访问层”这样经典的“三层架构”。

## 两者关系
ECB是框架目录设计与逻辑架构的一种具体方法。
