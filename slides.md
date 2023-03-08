---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left
# use UnoCSS
css: unocss
---

# Go 框架选型

Go微服务技术框架选型

<div class="pt-12">
  
</div>

---

# 候选技术

<br>
<br>

- **Web应用框架** - 提供HTTP协议服务为基础的，快速构建应用程序的模型和工具库，beego、gin、iris、fiber、echo等。
- **微服务框架** - Java 体系的 Spring Cloud ，Go 体系的 kratos、Go Zero 和 Go Kit 等。这些框架的共同点都是 用来帮助我们在开发过程中快速引入各种服务治理的组件。
- **服务网格（Service Mesh）** - 服务网格被定义为一个专门的基础设施层，用于管理服务与服务之间的通信，使其可管理、可见、可控制。

<br>
<br>

### 侵入式 vs 非侵入性

### 业务代码中 vs 基础设施

---

# Web应用框架 vs 微服务框架

<br>

- **通讯方式** - Web应用框架以**HTTP协议**服务为基础；微服务框架大多以**protobuf和grpc**为基础
- **使用方式** - Web应用框架以API调用为主，实现和规范耦合重；微服务框架以DSL(protobuf)约定规范，然后使用生成式的模式为主。
- **业务代码** - Web应用框架以传统的MVC或者Handler模式为主；微服务框架以logic或者domain对象实现为主，其余适配性代码以脚手架生成为主。
- **扩展性** - Web应用框架以框架和插件的模式进行功能集成，微服务框架以微服务组件的形式为主

<br>

### 使用范围

- Web应用框架主要应用在单体服务中
- 微服务框架主要是用在微服务框架中，以框架侵入式的方式进行微服务治理

---

<img src="/microservice-pattern-language.jpg" class="h-full mx-auto" />

---

# 服务治理

为了将业务和基础设施解耦

### **Communication patterns（通信模式）**

- **Style（风格）** - 通信机制的选择。有远程过程调用（Remote Procedure Invocation）、消息（Messaging）和领域独用协议（Domain-specific protocol）。
- **Service registry（服务注册）** - 用于记录服务实例位置的服务注册表。
- **Service discovery（服务发现）** - 通过查询服务注册表获取服务实例的位置。有客户端服务发现（Client-side discovery）和服务器端服务发现（Server-side discovery）两种模式。
- **Reliability（可靠性）** - 当调用远端服务返回的故障率超过一定的阀值时，需要使用断路器（Circuit Breaker）熔断服务调用方和远端服务的调用链并立刻返回失败的信息，以防止引发服务雪崩现象。
- **External API（外部 API）** - 外部客户端与服务之间的通讯。需要引入 API 网关（API gateway），为了让后端 API 满足不同的前端使用场景，可能还需要引入 BFF 层，即服务前端的后端（Backend for front-end）。

---

# 服务治理

为了将业务和基础设施解耦

### **Security（安全）**

- 服务实例之间可以通过在请求头携带访问令牌（Access Token）等方式来安全地传递客户端的身份信息。

---

# 服务治理

为了将业务和基础设施解耦

### **Observability（可观测性）**

- **应用日志（Log aggregation）** - 聚合应用程序产生的日志文件。
- **审计日志（Audit logging）** - 把用户行为记录在数据库中供日后核查。
- **应用指标（Application metrics）** - 在代码中实现收集应用运营过程中各类指标的功能。
- **分布式追踪（Distributed tracing）** - 在服务代码中针对每一个外部访问，都分配一个唯一的标识符，并在跨服务访问时传递这个标识符以供追踪分布式引发的问题。
- **异常追踪（Exception tracking）** - 把所有服务程序代码触发的异常信息都汇聚到集中的异常追踪服务，并根据一定的逻辑对开发者或运维人员发出通知。
- **健康检查 API（Health check API）** - 一个监控服务可调用的 API，通常返回服务健康度信息，或对 ping 等心跳检查请求做出响应。

---

# 微服务框架比较

|框架|概述|优势|缺点|特性|
|:----|:----|:----|:----|:----|
|go-kit|微服务的工具集|提供了微服务的标准工具集|需要很多手写代码|Go-kit将自己描述为微服务的标准库|
|go-micro|是最早最经典的Go微服务框架之一|轻量级框架，入门简单，文档清晰|版本兼容性差，社区活跃度一般，目前已经不维护|服务发现、负载均衡、消息编码、请求/响应：基于RPC的请求/响应，支持双向流。Async Messaging：PubSub是异步通信和事件驱动架构的重要设计思想。可插拔接口。|


---

# 微服务框架比较

|框架|概述|优势|缺点|特性|
|:----|:----|:----|:----|:----|
|go-kratos| Go 微服务框架，包含大量微服务相关功能及工具|轻量级的微服务框架，框架定位于解决微服务的核心诉求。|文档教程难懂，学习使用难度高|APIs、Errors、Metadata、Config、Logger、Metrics、Tracing、Encoding、Transport、Registry、Validation、SwaggerAPI|
|go-zero|是一个集成了各种工程实践的 web 和 rpc框架|社区生态非常好，新手学习成本低，快速上手有强大的goctl脚手架工具|较重，有一定强约束|稳定性；内建级联超时控制、限流、自适应熔断、自适应降载等微服务治理能力；微服务治理中间件可无缝集成到其它现有框架使用；极简的 API 描述，一键生成各端代码；自动校验客户端请求参数合法性；大量微服务治理和并发工具包|

---

# 服务网格
参考项目 [go-coffee](https://github.com/thangchung/go-coffeeshop)

### 特点
- **grpc构建服务**
- **使用grpc-gateway** 提供 HTTP API
- **使用[Standard Go Project Layout](https://github.com/golang-standards/project-layout/blob/master/README_zh.md)** 作为项目布局
- **使用DDD的模式** 组织业务模块
- 应用层代码中不包含任何服务治理相关功能
- 使用Nomad, Consul Connect, Vault作为基础设施集成

<br>

### 待解决问题

使用Istio替代Nomad, Consul Connect, Vault的配置部署方式

---

# Standard Go Project Layout

<img src="/layout.jpg" class="h-full mx-auto" />


---

# DDD 组织业务模块

<img src="/ddd.jpg" class="h-full mx-auto" />

---

# Nomad, Consul Connect, Vault作为基础设施集成

<img src="/job.jpg" class="h-full mx-auto" />

---

<img src="/coffeeshop_hashicorp.svg" class="h-full mx-auto" />




