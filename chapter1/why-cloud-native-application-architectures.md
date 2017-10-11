# 为何使用云原生应用架构

首先我们来阐述下将应用迁移到云原生架构的动机。

## 速度

天下武功，唯快不破，市场竞争亦是如此。想象一下，能够快速创新、实验并交付软件的企业，与使用传统软件交付模式的企业，谁将在市场竞争中胜出呢？

在传统企业中，为应用提供环境和部署新版本花费的时间通常以天、周或月来计算。这种速度严重限制了每个发行版可以承担的风险，因为修复这些错误往往跟发行一个新版本有差不多的耗时。

互联网公司经常提到它们每天极版次发布的实践。为什么频繁发布如此重要？如果你可以每天实现几百次发布，你们就可以几乎立即从错误的版本恢复过来。如果你可以立即从错误中恢复过来，你就能够承受更多的风险。如果你可以承受更多的风险，你就可以做更疯狂的试验——这些试验结果可能会成为你接下来的竞争优势。

基于云基础设置的弹性和自服务的特性天生就适应于这种工作方式。通过调用云服务API来提供新的应用程序环境比基于表单的手动过程要快几个数量级。然后通过另一个API调用将代码部署到新的环境中。将自服务和hook添加到团队的CI/CD服务器环境中进一步加快了速度。现在，我们可以回答精益大师Mary Poppendick提出的问题了——“如果只是改变了应用的一行代码，您的组织需要多长时间才能把应用部署到线上？“答案是几分钟或几秒钟。

你可以大胆想象 一下，如果你也可以达到这样的速度，你的团队、你的业务可以做哪些事情呢？

## 安全

光是速度快还是不够的。如果你开车是一开始就把油门踩到底，你将因此发生事故而付出惨痛的代价（有时甚至是致命的）！不同的交通方式如飞机和特快列车都会兼顾速度和安全性。云原生应用架构在快速变动的需求、稳定性、可用性和耐久性之间寻求平衡。这是可能的而且非常有必要同时实现的。

我们前面已经提到过，云原生应用架构可以让我们迅速地从错误中恢复。我们没有谈论如何预防错误，而在企业里往往在这一点上花费了大量的时间。在追寻速度的路上，大而全的前端升级，详尽的文档，架构复核委员会和漫长的回归测试周期在一次次成为我们的绊脚石。当然，之所以这样做都是出于好意。不幸的是，所有这些做法都不能提供一致可衡量的生产缺陷改善度量。

那么我们如何才能做到即安全又快速呢？

**可视化**

我们的架构必须为我们提供必要的工具，以便可以在发生故障时看到它。 我们需要观测一切的能力，建立一个“哪些是正常”的概况，检测与标准情况的偏差（包括绝对值和变化率），并确定哪些组件导致了这些偏差。功能丰富的指标、监控、警报、数据可视化框架和工具是所有云原生应用程序体系结构的核心。

**故障隔离**

为了限制与故障带来的风险，我们需要限制可能受到故障影响的组件或功能的范围。如果每次亚马逊的推荐引擎挂掉后人们就不能再在亚马逊上买产品，那将是灾难性的。单体架构通常就是这种类型的故障模式。云原生应用程序架构通常使用微服务。通过将系统拆解为微服务，我们可以将任何一个微服务的故障范围限制在这个微服务上，但还需要结合容错才能实现这一点。

**容错**

仅仅将系统拆解为可以独立部署的微服务还是不够的；还需要防止出现错误的组件将错误传递它所依赖的组件上而造成级联故障。Mike Nygard 在他的《Release It! - Pragmatic Programmers》一书中描述了一些容错模型，最受欢迎的是**断路器**。软件断路器的工作原理就类似于电子断路器（保险丝）：断开它所保护的组件与故障系统之间的回路以防止级联故障。它还可以提供一个优雅的回退行为，比如回路断开的时候提供一组默认的产品推荐。我们将在“容错”一节详细讨论该模型。

**自动恢复**

凭借可视化、故障隔离和容错能力，我们拥有确定故障所需的工具，从故障中恢复，并在进行错误检测和故障恢复的过程中为客户提供合理的服务水平。一些故障很容易识别：它们在每次发生时呈现出相同的易于检测的模式。以服务健康检查为例，结果只有两个：健康或不健康，up或down。很多时候，每次遇到这样的故障时，我们都会采取相同的行动。在健康检查失败的情况下，我们通常只需重新启动或重新部署相关服务。云原生应用程序架构不要当应用在这些情况下无需手动干预。相反，他们会自动检测和恢复。换句话说，他们给电脑装上了寻呼机而不是人。

## 弹性扩展

随着需求的增加，我们必须扩大服务能力。过去我们通过垂直扩展来处理更多的需求：购买了更强悍的服务器。我们最终实现了自己的目标，但是步伐太慢，并且产生了更多的花费。这导致了基于高峰使用预测的容量规划。我们会问”这项服务需要多大的计算能力？”然后购买足够的硬件来满足这个要求。很多时候我们依然会判断错误，会在如黑色星期五这类事件中打破我们的可用容量规划。但是，更多的时候，我们将会遇到数以百计的服务器，它们的CPU都是空闲的，这会让资源使用率指标很难看。

创新型的公司通过以下两个开创性的举措来解决这个问题：

- 它们不再继续购买更大型的服务器，取而代之的是用大量的更便宜机器来水平扩展应用实例。这些机器更容易获得，并且能够快速部署。

- 通过将大型服务器虚拟化成几个较小的服务器，并向其部署多个隔离的工作负载来改善现有大型服务器的资源利用率。

随着像亚马逊AWS这样的公有云基础设施的出现，这两个举措融合了起来。虚拟化工作被委托给云提供商，消费者只需要关注在大量的云服务器实例很想扩展它们的应用程序实例。最近，作为应用程序部署的单元，发生了另一个转变，从虚拟机转移到了容器。

由于公司不再需要大量启动资金来部署软件，所以向云的转变打开了更多创新之门。正在进行的维护还需要较少的资本投入，并且通过API进行配置不仅可以提高初始部署的速度，还可以最大限度地提高我们应对需求变化的速度。

不幸的是，所有这些好处都带有成本。相较于垂直扩展的应用，支持水平扩展的应用程序的架构必须不同。云的弹性要求应用程序的状态短暂性。我们不仅可以快速创建新的应用实例；我们也必须能够快速、安全地处置它们。这种需求是状态管理的问题：一次性与持久性如何互相影响？ 在大多数垂直架构中采用的诸如聚类会话和共享文件系统的传统方法并不能很好地支持水平扩展。

云原生应用程序架构的另一个标志是将状态外部化到内存数据网格，缓存和持久对象存储，同时保持应用程序实例本身基本上是无状态的。无状态应用程序可以快速创建和销毁，以及附加到外部状态管理器和脱离外部状态管理器，增强我们响应需求变化的能力。当然这也需要外部状态管理器自己来扩展。大多数云基础设施提供商已经认识到这一必要性，并提供了这类服务的健康管理。

## 移动应用和客户端多样性

2014年1月，美国移动设备占互联网使用量的55％。专门针对桌面用户而开发的应用程序的时代已经过去。不过，我们必须假设用户装在口袋里到处散步的是超级计算机。这对我们的应用架构有很大的影响，因为指数级用户可以随时随地与我们的系统进行交互。

以查看银行账户余额为例。这项任务过去是通过拨打银行的呼叫中心，前往ATM，或者在银行的一个分支机构的向柜员请求完成的。这些客户互动模式在任何时间内，都会对银行为底层软件系统提出新需求产生极大的限制。

迁移到网上银行导致访问量的上升，但并没有从根本上改变交互模式。您仍然必须在计算机终端上与系统进行交互，这仍然显著限制了需求。正如我的同事Andrew Clay Shafer经常说的那样，“我们口袋里正带着超级计算机到处游走”，我们开始对这些系统带来很大负载。现在，成千上万的客户可以随时随地与银行系统进行互动。 一位银行行政人员表示，在发薪日，客户会每隔几分钟检查一次余额。 遗留的银行系统架构根本无法满足这种需求，而云原生的应用程序体系结构却可以。

移动平台的巨大差异也对应用架构提出了要求。客户随时都可能与多个不同供应商生产的设备，运行多个不同的操作系统平台，运行多个版本的相同操作平台以及不同类别的设备（例如手机与平板电脑）进行交互。 这不仅对移动应用程序开发人员，还对后端服务的开发人员造成了各种限制。

移动应用程序通常必须与多个传统系统以及云原生应用程序架构中的多个微服务进行交互。这些服务无法设计成支持客户使用的各种各样移动平台的独特需求。强迫实现这些不同的服务，为移动应用程序开发人员上带来了负担，增加了应用访问延迟和网络访问频率，导致应用响应慢、耗电量高，最终导致用户删除您的应用程序。云原生应用程序架构还通过诸如API网关之类的设计模式来支持移动优先开发的概念，API网关将服务聚合负担转移回服务器端。