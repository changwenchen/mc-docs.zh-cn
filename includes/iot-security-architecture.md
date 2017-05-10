# 物联网安全体系结构

在设计系统时，我们应了解该系统的潜在威胁并添加适当的防御机制，以妥善设计系统与其体系结构。在一开始设计产品时就先考虑安全是非常重要的，因为了解攻击者可能如何破坏系统，有助于从一开始就准备好适当的安全防护功能。

## 安全从威胁模型开始

Microsoft 产品在威胁模型方面历史悠久，并已公开发表公司的威胁建模过程。Microsoft 的成功经验证实，建模除了可以立即掌握哪些是最关键的威胁，还拥有超越期望的优点。例如，建模操作还能创建与其他外部开发团队开放讨论的管道，以获取产品的新构想和增强功能。

威胁建模操作的目标是了解攻击者可能如何破坏系统，以确保准备好适当的安全防护功能。威胁建模操作可确保设计团队在系统设计阶段就考虑安全防护功能，而不是等到系统部署之后才亡羊补牢。这一点是非常重要的，因为要现场修改大量设备的安全防御措施非但不可行、容易产生错误，而且将让客户面临风险。

许多开发团队能精准掌握对客户有益的系统功能要求。但是，识别有心人士可能如何滥用系统的隐匿方式则更具挑战性。威胁建模操作可帮助开发团队了解攻击者可能采取的行动和原因。威胁建模操作是一种结构化的过程，可创建系统安全设计决策的介绍，以及过程中的设计更改将对安全产生哪些影响。尽管威胁模型只是一种文档，但此文档也是传承知识、保存所学经验，并帮助新团队快速上手的理想方式。最后，威胁建模操作的结果应可让你考虑安全的其他层面，例如想要提供给客户哪些安全承诺。将这些承诺与威胁建模操作配合使用，即可针对物联网 (IoT) 解决方案提供分析与驱动测试。

### 威胁建模的时机

在设计阶段中包含[威胁建模](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx)时，才能发挥最大的价值。因为只有在设计阶段，才有最大的弹性空间可进行威胁消除更改。最理想的结果就是通过设计来消除潜在威胁。这样做比添加安全防护功能、进行测试，并确保它们保持最新状态等操作更加容易，因为消除不是随时想做就能做到。随着产品日益成熟，消除威胁会变得更加困难，相比于开发早期采用的威胁建模，这需要更多投入与更高难度的取舍。

### 威胁建模应该注意什么？

应该将模型与整个方案集成成一体，同时将焦点放在以下领域：

- 安全与隐私功能
- 其成败将影响安全的相关功能
- 触及信任边界的功能

### 威胁模型跟谁有关

威胁建模与任何其他过程一样。建议将威胁模型文档视为方案的任何其他组件，并进行验证。许多开发团队能精准掌握对客户有益的系统功能要求。但是，识别有心人士可能如何滥用系统的隐匿方式则更具挑战性。威胁建模操作可帮助开发团队了解攻击者可能采取的行动和原因。

### 如何进行威胁建模

威胁建模过程包括以下四个步骤：

- 为应用程序建模
- 枚举威胁
- 缓解威胁
- 验证缓解措施

#### 过程步骤

构建威胁模型时，请谨记以下三项准则：

1. 创建参考体系结构图表。
2. 先从广度下手。获取概述，先概略了解整个系统，再进行深入工作。这有助于确保在适当的位置进行深入工作。
3. 由你控制过程，而不是受到过程的摆布。如果在建模阶段发现问题，并想要进行探索，尽管放心去做！ 不需要盲目遵循这些步骤。

#### 威胁

威胁模型的四个核心元素为：

- 进程（Web 服务、Win32 服务、*nix 守护程序等）请注意，如果无法在这些区域中进行技术性向下钻取时，可将某些复杂实体（例如现场网关和传感器）抽象化为进程。
- 数据存储（存储数据的任何位置，例如配置文件或数据库）
- 数据流（其中数据将在应用程序的其他元素之间移动）
- 外部实体（与系统交互但不在应用程序控制之下的任何要素，例如用户和附属源）

体系结构图中的所有元素都受限于各种威胁；针对这些威胁，我们将使用 STRIDE 助记符。请阅读 [Threat Modeling Again, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/)（再次威胁建模 STRIDE）以详细了解 STRIDE 元素。

应用程序图表的不同元素受限于特定 STRIDE 威胁：

- 进程受限于 STRIDE
- 数据流受限于 TID
- 数据存储受限于 TID，有时受限于 R（如果数据存储是日志文件）。
- 外部实体受限于 SRD

## IoT 中的安全性

特殊用途的连接设备具有大量的潜在交互接口区与交互模式，必须全面考虑所有项目才能提供适当的框架，以保护这些设备的数字访问安全性。此处的“数字访问”一词是用于与通过直接设备交互来执行的任何操作进行隔离，而直接交互的访问安全通过物理访问控制来保障。例如，将设备放入室内，然后将门上锁。尽管物理访问无法通过软件和硬件拒绝访问，但你仍可以采取一些措施来防止物理访问干扰系统。

在探索交互模式时，我们将对“设备控制”和“设备数据”同等重视。“设备控制”可以归类为任一方提供给设备的任何信息，其目标是要更改或影响设备针对其状态或环境状态的行为。“设备数据”可以归类为任何信息，其为设备发出给另一方的设备状态和环境观察状态相关信息。

为了优化最佳安全实践，建议将典型的 IoT 体系结构划分为多个组件/区域，作为威胁建模练习的一部分。本部分将详细说明这些区域，并包括以下重点：

- 设备，
- 现场网关，
- 云网关，以及
- 服务。

区域是从广度方面来隔离方案；每个区域通常将有自身的数据以及身份验证和授权要求。区域也可以用于隔离损毁，并限制低度信任区域对较高信任区域的影响。

每个区域都由信任边界来隔离；下图中其以红色虚线标示。它代表从某个源到另一个源之间的数据/信息转换。在这种转换期间，数据/信息可能受限于 STRIDE，即欺骗 (Spoofing)、篡改 (Tampering)、可否认性 (Repudiation)、信息泄漏 (Information Disclosure)、拒绝服务 (Denial of Service) 和权限提升 (Elevation of Privileg)。

![IoT 安全区域](./media/iot-security-architecture/iot-security-architecture-fig1.png)

每个边界内所描述的组件也将受限于 STRIDE，因此我们需启用解决方案的全方位威胁建模视图。以下部分详述了每个组件和特定的安全注意事项，以及应准备好的解决方案。

后续部分将介绍这些区域中常见的标准组件。

### 设备区域

设备环境是指设备周围实时的物理空间，其中可进行设备的物理访问和/或“局域网”对等数字访问。“局域网”假设为不同的隔离网络，但可能桥接到公共 Internet，并包含任何短程无线电技术以让设备进行对等通信。它*不*包括任何网络虚拟化技术（其将创建此类局域网络的假象），也不包含公共电信营运商的网络（其中任两个设备需要具有对等通信关联，才能跨公共网络空间进行通信）。

### 现场网关区域

现场网关是一种设备/工具，或作为通信启用器的一般用途服务器计算机软件，甚至可能是设备控制系统和设备数据处理中心。现场网关的区域包括现场网关本身以及连接到它的所有设备。顾名思义，现场网关可作为外部专用的数据处理设备，通常与位置绑定，也可能受限于物理入侵的影响，并具有限的操作冗余。简而言之，现场网关是在大家知道其功能以后，最容易触及与蓄意破坏的设备。

现场网关和单纯的流量路由器不同，它在管理访问与数据流方面扮演积极的角色，这意味着它是由应用程序处理的实体和网络连接或会话终端。相反地，NAT 设备或防火墙并不算现场网关，因为它们并不是明确的连接或会话终端，而是连接或会话经过的路线（或块）。现场网关有两个不同的外围应用。一个外围应用表示区域的内部，并面对附加到该区域的设备，而另一个外围应用表示区域的边缘，并面对所有外部方。

### 云网关区域

云网关是一种系统，其可让跨公共网络空间的多个不同站点的设备之间进行远程通信；它通常针对基于云的控制和数据分析系统，也可以说是此类系统的联合。在某些情况下，云网关可立即提升特殊用途设备的终端（如平板电脑或手机）访问速度。在此处所述的上下文中，“云”是指未绑定到附加设备或现场网关的同一站点的专用数据处理系统。此外，在云区域中，用于防止物理访问目标的操作措施并不一定要向“公有云”基础结构公开。

云网关可能将映射到网络虚拟化覆盖，以隔离云网关与所有附加设备或现场网关的任何其他网络流量。云网关本身不是设备控制系统，也不是处理或存储设备数据的设备；而是这些设备与云网关对接。云网关区域包含云网关和所有现场网关，以及直接或间接附加到其上的设备。区域边缘是不同的外围应用，所有外部方将通过此处进行通信。

### 服务区域

此上下文将“服务”定义为任何软件组件或模块，它通过现场网关或云网关来与设备交互、进行数据收集和分析，以及执行命令和控制等功能。服务属于中介。它们将以面向网关和其他子系统的标识来执行操作、存储和分析数据、根据数据的深入信息或计划自动向设备发出命令，并向授权用户公开信息与控制功能。

### 信息设备与专用设备

电脑、手机和平板电脑主要是交互式信息设备。手机和平板电脑专门针对最大电池续航时间做了优化。当无需立即与人员交互，或者不提供播放音乐或为设备所有者进行路线规划等服务时，最好将这些设备部分关闭。从系统角度来看，这些信息技术设备主要用作面向用户的代理。也就是说，它们是建议操作的“用户行动器”，以及收集输入的“用户传感器”。

专用设备则各有不同，从简单的温度传感器到每条生产线包含上千个组件的复杂工厂生产线都算在内。这些设备主要以实际用途为目的，即使提供用户接口，多半都只限于用于对接或集成在现实世界中的资产。它们可以测量和报告环境情况、打开阀门、控制伺服、发出警报、切换灯号，以及执行许多其他任务。如果使用信息设备进行某些任务将大材小用、过于昂贵、过大或太脆弱，就可以改用专用设备。此类设备的具体用途确定了它们的技术设计，包括可用的生产预算金额与预定的生命周期操作。这两项关键因素的组合将限制可用的操作能源预算、实际使用量，乃至可用的存储空间、计算和安全功能。

如果具有自动化功能或可远程控制的设备发生某些问题，例如，物理缺陷或控制逻辑缺陷导致蓄意的未经授权入侵和操作。这些问题可能导致生产批受损、建筑物被侵占或烧毁，甚至可能造成人员伤亡。此类损失的等级显然比信用卡失窃和被刷爆来得严重。因此，如果是涉及物品移动的设备以及最终将发出命令导致物品移动的传感器数据，这样的安全标准就必须高于任何电子商务或银行方案。

### 设备控制与设备数据的交互

特殊用途的连接设备具有大量的潜在交互接口区与交互模式，必须全面考虑所有项目才能提供适当的框架，以保护这些设备的数字访问安全性。此处的“数字访问”一词是用于与通过直接设备交互来执行的任何操作进行隔离，而直接交互的访问安全通过物理访问控制来保障。例如，将设备放入室内，然后将门上锁。尽管物理访问无法通过软件和硬件拒绝访问，但你仍可以采取一些措施来防止物理访问干扰系统。

在探索交互模式与进行威胁模型时，我们将对“设备控制”和“设备数据”同等重视。“设备控制”可以归类为任一方提供给设备的任何信息，其目标是要更改或影响设备针对其状态或环境状态的行为。“设备数据”可以归类为任何信息，其为设备发出给另一方的设备状态和环境观察状态相关信息。

## 对 Azure IoT 参考体系结构进行威胁建模

Microsoft 使用上述体系结构来对 Azure IoT 进行威胁建模。因此，在以下部分中，我们将使用 Azure IoT 参考体系结构的具体示例来演示如何分析 IoT 威胁建模，以及如何处理所识别的威胁。在本例中，我们将识别以下四个主要区域：

- 设备和数据源，
- 数据传输，
- 设备和事件处理，以及
- 呈现

![Azure IoT 威胁建模](./media/iot-security-architecture/iot-security-architecture-fig2.png)

下图提供了简化的 Microsoft IoT 体系结构视图，其中使用 Microsoft 威胁建模工具所用的数据流原理图模型：

![使用 MS 威胁建模工具为 Azure IoT 进行威胁建模](./media/iot-security-architecture/iot-security-architecture-fig3.png)

请注意，该体系结构将设备和网关功能隔离开来。这可让用户利用更安全的网关设备：此类设备可以使用安全的协议与云网关进行通信，且通常需要的处理开销高于本机设备（例如恒温器）可自行提供的量。在 Azure 服务区域中，我们假设 Azure IoT 中心服务代表云网关。

### 设备与数据源/数据传输

本部分将透视威胁建模，以探讨前面概述的体系结构，并提供如何解决一些固有考虑因素的概述。我们将着重于威胁模型的核心元素：

- 进程（包括我们掌控的进程以及外部项）
- 通信（也称为数据流）
- 存储（也称为数据存储）

#### 进程

在 Azure IoT 体系结构所述的每个类别中，我们尝试跨数据/信息存在的三个阶段来降低不同威胁的风险：进程、通信以及存储。下面我们提供“进程”分类中最常见威胁的概述，并概要说明如何以最妥善的方式降低这些威胁的风险：

**欺骗 (S)**：攻击者可能从设备提取加密密钥材料（无论软件或硬件级别），然后使用获取加密密钥数据的设备的标识，配合不同的物理或虚拟设备来访问系统。遥控器就是很好的例子，它既可以遥控电视机，也是很流行的恶作剧工具。

**服务拒绝 (D)**：通过干扰无线电频率或剪断线路，导致设备无法运行或无法通信。例如，蓄意破坏监控摄影头的电源或网络连接，使其完全无法报告数据。

**篡改 (T)**：如果非法程序可以使用密钥材料或保存密钥材料的加密设备，攻击者可能将部分或全部替换设备上执行的软件，以此让取而代之的软件运用设备的正版标识。例如，攻击者可能使用提取的密钥材料来拦截和隐藏通信路径上的设备数据，并以窃取的密钥材料来通过身份验证，再以假数据取而代之。

**信息泄露 (I)**：如果设备正在执行受操控的软件，此类受操控的软件可能将会数据泄漏给未经授权方。例如，攻击者可能使用提取的密钥材料来介入设备、控制器、现场网关或云网关之间的通信路径，以此窃取信息。

**权限提升 (E)**：具有特定功能的设备可能将被强制执行其他操作。例如，编程为半开的阀门可能将受骗而完全打开。

| **组件** | **威胁** | **缓解措施** | **风险** | **实现** |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 设备 | S | 将标识分配给设备并对设备进行身份验证 | 用其他设备替换设备或部分设备。如何确定我们是否在与正确的设备通信？ | 使用传输层安全性 (TLS) 或 IPSec 来验证设备。如果设备无法处理完全非对称加密，则基础结构应该支持在这些设备上使用预共享密钥 (PSK)。利用 Azure AD，OAuth |
| | TRID | 通过让人很难甚至不可能从设备提取密钥和其他加密材料，以便为设备应用防篡改机制。 | 但风险是我们不知道设备是不是已受到篡改（物理干扰）。如何确定设备未受到篡改。 | 最有效的缓解措施是信赖平台模块 (TPM) 功能，它可让你将密钥存放在特殊的芯片电路中，以确保密钥不被读取，而仅可用于只使用密钥而不会揭露密钥的加密操作。设备的内存加密。设备的密钥管理。为代码签名。 |
| | E | 针对设备使用访问控制。授权方案。 | 如果设备可以根据外部源甚至遭入侵的传感器的命令来执行各项操作，则意味着攻击可以凭此执行操作，而不仅是访问内容。 | 针对设备使用授权方案 |
| 现场网关 | S | 现场网关到云网关的验证（证书式、PSK、基于声明...） | 如果某人可以欺骗现场网关，则就可以伪装成任何设备。 | TLS RSA/PSK、IPSe、[RFC 4279](https://tools.ietf.org/html/rfc4279)。同样地，一般的设备密钥存储和证明考虑因素都高度建议使用 TPM。IPSec 的 6LowPAN 扩展可支持无线传感器网络 (WSN)。 |
| | TRID | 保护现场网关免遭篡改 (TPM) | 欺骗攻击将让云网关误以为和现场网关通信，而可能导致信息泄漏与数据遭篡改。 | 内存加密、TPM、身份验证。 |
| | E | 针对现场网关使用访问控制机制 | | |

下面是此类威胁的一些示例：

欺骗：攻击者可能从设备提取加密密钥材料（无论软件或硬件级别），然后使用获取加密密钥数据的设备的标识，配合不同的物理或虚拟设备来访问系统。

**服务拒绝**：通过干扰无线电频率或剪断线路，导致设备无法运行或无法通信。例如，蓄意破坏监控摄影头的电源或网络连接，使其完全无法报告数据。

**篡改**：如果非法程序可以使用密钥材料或保存密钥材料的加密设备，攻击者可能将部分或全部替换设备上执行的软件，以此让取而代之的软件运用设备的正版标识。

**篡改**：显示空走廊图像的监控摄影头可能是对着走廊拍摄图片。烟雾或火灾传感器可能将报告下面有人手持打火机。任一种情况下，系统可能将在技术上完全信任设备，但是设备报告的信息却有可能受到操控。

**篡改**：攻击者可能使用提取的密钥材料来拦截和隐藏通信路径上的设备数据，并以窃取的密钥材料来通过身份验证，再以假数据取而代之。

**篡改**：如果非法程序可以使用密钥材料或保存密钥材料的加密设备，攻击者可能将部分或全部替换设备上运行的软件，以此让取而代之的软件运用设备的正版标识。

**信息泄露**：如果设备正在执行受操控的软件，此类受操控的软件可能将会数据泄漏给未经授权方。

**信息泄漏**：攻击者可能使用提取的密钥材料来介入设备、控制器、现场网关或云网关之间的通信路径，以此窃取信息。

**拒绝服务**：可将设备关闭或改为无法通信的模式（许多工业机器刻意设计了此功能）。

**篡改**：可将设备重新配置为在未知状态下运行，以控制系统（在已知校正参数的范围以外），并提供可误导的数据

**权限提升**：执行特定功能的设备可能将被强制执行其他操作。例如，编程为半开的阀门可能将受骗而完全打开。

**拒绝服务**：可将设备变成为无法通信的状态。

**篡改**：可将设备重新配置为在未知状态下运行，以控制系统（在已知校正参数的范围以外），并提供可误导的数据。

**欺骗/篡改/否认性**：如果未受保护（在使用者进行远程控制时很少出现这种情况），攻击者可以匿名方式操控设备的状态。遥控器就是很好的例子，它既可以遥控电视机，也是很流行的恶作剧工具。

#### 通信

设备、设备和现场网关，以及设备和云网关之间的通信路径周围也充满威胁。针对设备/VPN 上的开放套接字，下表提供了相关指导：

| **组件** | **威胁** | **缓解措施** | **风险** | **实现** |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 设备 IoT 中心 | TID | 用于加密流量的 (D)TLS (PSK/RSA) | 窃听或干扰设备与网关之间的通信 | 协议级别的安全性。使用自定义协议时，我们需要了解如何对其进行保护。在大多数情况下，通信主要是发生在设备到 IoT 中心这一段位置（由设备发起连接）。 |
| 设备 设备 | TID | 用于加密流量的 (D)TLS (PSK/RSA)。 | 读取设备之间正在传输的数据。篡改数据。使用新连接使设备过载 | 协议级别的安全性 (HTTP(S)/AMQP/MQTT/CoAP)。使用自定义协议时，我们需要了解如何对其进行保护。DoS 威胁的缓解措施是通过云网关或现场网关将设备设为对等，使其仅可充当网络客户端。在网关代理对等方之后，对等互连可以实现对等方之间的直接连接。 |
| 外部实体设备 | TID | 外部实体与设备的强式配对 | 窃听设备的连接。干扰设备的通信 | 使用 NFC/蓝牙 LE 安全配对外部实体与设备。控制设备（物理）的操作面板 |
| 现场网关 云网关 | TID | 用于加密流量的 TLS (PSK/RSA)。 | 窃听或干扰设备与网关之间的通信 | 协议级别的安全性 (HTTP(S)/AMQP/MQTT/CoAP)。使用自定义协议时，我们需要了解如何对其进行保护。 |
| 设备 云网关 | TID | 用于加密流量的 TLS (PSK/RSA)。 | 窃听或干扰设备与网关之间的通信 | 协议级别的安全性 (HTTP(S)/AMQP/MQTT/CoAP)。使用自定义协议时，我们需要了解如何对其进行保护。 |

下面是此类威胁的一些示例：

**拒绝服务**：当受限设备主动侦听入站连接或网络上来路不明的数据报时，通常会处于 DoS 威胁之下，或者设备可能将被来路不明的流量给填满，因为攻击者可以打开许多并发连接但却不提供服务或以非常缓慢的速度提供。在这两种情况下，网络可能会将设备解释为无法正常工作。

**欺骗、信息泄漏**：受限设备和专用设备通常提供一个通用的安全机制，例如密码或 PIN 码保护，或完全依赖网络信任，这意味着当设备位于同一网络时，将授予对信息的访问权限，且该网络通常只受到共享密钥的保护。这意味着，当设备或网络共享的机密遭到透露时，攻击者可以控制设备或观察从设备发出的数据。

**欺骗**：攻击者可能拦截或部分覆盖广播，从而欺骗发起方（中间人）

**篡改**：攻击者可能拦截或部分覆盖广播，并发送虚假的信息

**信息泄露**：攻击者可能窃听广播并未经授权获取信息 
**拒绝服务**：攻击者可能阻塞广播信号，并拒绝信息分发

#### 存储

每个设备和现场网关都有某种形式的存储（临时为数据排队、OS 映像存储）。

| **组件** | **威胁** | **缓解措施** | **风险** | **实现** |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 设备存储 | TRID | 存储加密、为日志签名 | 读取存储中的数据（PII 数据）、篡改遥测数据。篡改排队或缓存的命令控制数据。在本地缓存或排队时篡改配置或固件更新包可能导致 OS 和/或系统组件遭到入侵 | 加密、消息身份验证代码 (MAC) 或数字签名。尽可能通过资源的访问控制列表 (ACL) 或权限进行强式访问控制。 |
| 设备 OS 映像 | TRID | | 篡改 OS/替换 OS 组件 | 只读 OS 分区、签名的 OS 映像、加密 |
| 现场网关存储（将数据排队） | TRID | 存储加密、为日志签名 | 读取存储中的数据（PII 数据）、篡改遥测数据、篡改排队或缓存的命令控制数据。在本地缓存或排队时篡改配置或固件更新包（发往设备或现场网关）可能导致 OS 和/或系统组件遭到入侵 | BitLocker |
| 现场网关 OS 映像 | TRID | | 篡改 OS/替换 OS 组件 | 只读 OS 分区、签名的 OS 映像、加密 |

### 设备和事件处理/云网关区域

云网关是一种系统，其实现跨公共网络空间的多个不同站点的设备之间进行远程通信；它通常针对基于云的控制和数据分析系统，也可以说是此类系统的联合。在某些情况下，云网关可立即提升特殊用途设备的终端（如平板电脑或手机）访问速度。在此处所述的上下文中，“云”是指未绑定到附加设备或现场网关的同一站点的专用数据处理系统，且其中用于防止物理访问目标的操作措施并不一定要向“公有云”基础结构公开。云网关可能将映射到网络虚拟化覆盖，以隔离云网关与所有附加设备或现场网关的任何其他网络流量。云网关本身不是设备控制系统，也不是处理或存储设备数据的设备；而是这些设备与云网关对接。云网关区域包含云网关和所有现场网关，以及直接或间接附加到其上的设备。

云网关通常是指以服务形式运行且自定义构建的软件，包含公开的终结点供现场网关和设备进行连接。因此，它的设计必须完全考虑到安全问题。请遵循 [SDL](https://www.microsoft.com/en-us/sdl/default.aspx) 过程来设计和构建此服务。

#### 服务区域

控制系统（或控制器）是一种软件解决方案，可与设备、现场网关或云网关对接来控制一个或多个设备，以便收集、存储和/或分析设备数据，进行演示或后续控制。在本文的范畴内，控制系统是唯一可立即帮助与人交互的实体。设备上的中间物理控制面（类似于可让用户关闭设备或更改其他属性的开关）则属例外情况，它们没有能够以数字方式访问的对等功能。

在中间物理控制面中，任何类型的控制逻辑都可限制实体控制面的功能，使对等功能可以从远程启动或避免远程输入的输入冲突 – 此类元控制面在概念上是连接到本地控制系统，该系统运用的基础功能与设备可并行附加的任何其他远程控制系统相同。请参阅 [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/)（云安全联盟 (CDA)）页，以了解云计算的头号威胁。

## 其他资源

有关其他信息，请参阅以下文章：

- [SDL Threat Modeling Tool（SDL 威胁建模工具）](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)

- [Azure IoT reference architecture（Microsoft Azure IoT 参考体系结构）](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf)

<!---HONumber=Mooncake_0523_2016-->