---
title: "Azure Active Directory 风险事件 | Microsoft Docs"
description: "本主题详细概述了风险事件。"
services: active-directory
keywords: "azure active directory identity protection , 安全性, 风险, 风险级别, 漏洞, 安全策略"
author: MarkusVi
manager: femila
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 56e5a634412a21302e0583f4585530480056a918
ms.openlocfilehash: c83597d16354bb9bb5507d77cae5ff0799922750
ms.lasthandoff: 03/02/2017


---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory 风险事件

Azure Active Directory 可帮助保护标识。 保护过程的一部分是检测与用户帐户相关的可疑操作。 该检测基于自适应机器学习算法和启发。 每个检测到的可疑操作都存储在称为“风险事件”的记录中。 


![风险事件](./media/active-directory-reporting-risk-events/91.png)


目前，Azure Active Directory 检测&6; 种类型的风险事件。

| 风险事件类型 | 风险级别 | 检测类型 |
| :-- | --- | --- |
| [具有已泄漏凭据的用户](#leaked-credentials) | 高 | 脱机 |
| [从匿名 IP 地址登录](#sign-ins-from-anonymous-ip-addresses) | 中型 | 实时 |
| [不可能前往异常位置](#impossible-travel-to-atypical-locations) | 中型 | 脱机 |
| [从不熟悉的位置登录](#sign-in-from-unfamiliar-locations) | 中型 | 实时 |
| [从受感染的设备登录](#sign-ins-from-infected-devices) | 低 | 脱机 |
| [从具有可疑活动的 IP 地址登录](#sign-ins-from-ip-addresses-with-suspicious-activity) | 中型 | 脱机|

## <a name="risk-level"></a>风险级别

风险级别属性是风险事件严重性的指示器（高、中或低）。 此属性可帮助确定必须执行的操作的优先级。 

风险事件的严重性表示标识入侵（结合它通常引发的干扰）预测的信号强度。

* **高**：高置信度和高严重性风险事件。 这些事件指出用户的标识明显已遭入侵，应立即补救所有受影响的用户帐户。

* **中**：高严重性，但置信度较低的风险事件，或反之。 这些事件具有潜在风险，应补救所有受影响的用户帐户。

* **低**：低置信度和低严重性风险事件。 此事件可能不要求立即采取措施，但与其他风险事件结合时，可能指出标识明显遭到入侵。

![风险级别](./media/active-directory-reporting-risk-events/01.png)


## <a name="detection-type"></a>检测类型

检测类型属性是风险事件检测时间范围的指示器（实时、脱机）。  
目前，大多数风险事件是在风险事件发生后，在后处理操作中脱机检测出的。

下表列出了不同检测类型显示在相关报表中时所用的时间。

| 检测类型 | 报告延迟 |
| --- | --- |
| 实时 | 5 - 10 分钟 |
| 脱机 | 2 - 4 小时 |



## <a name="risk-event-types"></a>风险事件类型

风险事件类型属性是已创建风险事件记录的可疑操作的标识符。  
目前，Azure Active Directory 检测&6; 种类型的事件。

Microsoft 对检测过程的持续投入使得：

- 现有风险事件的检测精度有所改进 
- 将来会添加新的风险事件类型

### <a name="leaked-credentials"></a>已泄漏凭据
Microsoft 安全研究人员发现了在暗网中公开发布的已泄漏凭据。 这些凭据通常以纯文本形式出现。 针对 Azure AD 凭据检查它们，如果存在匹配项，它们将在 Identity Protection 中报告为“已泄漏凭据”。

已泄漏凭据事件归类为“高”严重性风险事件，因为它们明确表示向攻击者提供用户名和密码。

### <a name="sign-ins-from-anonymous-ip-addresses"></a>从匿名 IP 地址登录
这一风险事件类型标识的是从匿名代理 IP 地址成功登录的用户。 这些代理通常是想要隐藏其设备 IP 地址的用户才会使用，他们可能意图不良。

我们建议你立即联系用户，验证他们是否使用了匿名 IP 地址。 此风险事件类型的风险级别为“中等”，因为本身使用匿名 IP 地址无法明显指示帐户受到威胁。


### <a name="impossible-travel-to-atypical-locations"></a>不可能前往异常位置
此风险事件类型可标识从相距遥远的地理位置进行的两次登录，根据用户以往的行为，其中至少有一个位置属于异常。 此外，两次登录之间相隔的时间短于用户从第一个位置前往第二个位置需要的时间，这表明有不同的用户在使用同一个凭据。 

这个机器学习算法会忽略明显的“误报”，从而改善不可能前往条件，例如组织中其他用户定期使用的 VPN 和位置。  系统具有 14 天的初始学习期限，在此期间它将学习新用户的登录行为。

不可能前往通常是一个出色的指示器，可指示黑客已能够成功登录。 但是，当用户使用新设备或使用组织中其他用户通常不使用的 VPN 前往时，可能会发生误报。 容易产生误报的另一个源头是误将服务器 IP 作为客户端 IP 传递的应用程序，这可能会让登录看起来像是从托管应用程序后端的数据中心发起的（这些数据中心通常是 Microsoft 数据中心，所以登录看起来像是从 Microsoft 拥有的 IP 地址发起的）。 由于这些误报，此风险事件的风险级别为“中等”。

### <a name="sign-in-from-unfamiliar-locations"></a>从不熟悉的位置登录
这一风险事件类型是一种实时登录评估机制，可根据过去的登录位置（IP、纬度/经度和 ASN）来确定新的/不熟悉的登录位置。 系统会存储用户以前的登录位置信息，并将其视为“熟悉”位置。 当从尚未在熟悉位置列表中列出的位置登录时，将触发此风险事件。 系统具有 14 天的初始学习期限，在此期间不会将任何新位置标记为不熟悉的位置。 系统还会忽略从常用设备和接近熟悉位置的地理位置进行登录。 

不熟悉的位置可明显指示攻击者能够尝试使用被盗的标识。 当用户旅行，试用新设备或使用新 VPN 时，可能发生误报。 由于这些误报，此事件类型的风险级别为“中等”。



### <a name="sign-ins-from-infected-devices"></a>从受感染的设备登录
此风险事件类型可标识从感染了恶意软件的设备（即主动与僵尸服务器通信）登录。 这通过分析用户设备的 IP 地址与连接过僵尸服务器的 IP 地址之间的相互关系可得以确定。 

此风险事件可标识 IP 地址，而非用户设备。 如果有多台设备使用单个 IP 地址，并且僵尸网络只控制了其中部分设备，那么从未被控制的设备登录则会发生误报，这就是将此风险事件的级别分类为“低”的原因。  

我们建议你联系用户并扫描用户的所有设备。 用户的个人设备也可能受感染，或者如前面所述，其他人也可能以用户身份从相同 IP 地址使用受感染的设备。 这些设备之所以受感染，通常是因为下载了防病毒软件尚未识别的恶意软件，也可能是因为用户存在不良习惯，导致设备容易遭受感染。

有关如何解决恶意软件感染的详细信息，请参阅[恶意软件防护中心](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409)。


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>从具有可疑活动的 IP 地址登录
此风险事件类型可标识在较短时间内在多个用户帐户上发现大量失败登录尝试的 IP 地址。 这与攻击者使用的 IP 地址的流量模式相似，在很大程度上表明帐户已遭到入侵或即将遭到入侵。 这是可忽略明显“误报”的机器学习算法，例如通常由组织中其他用户使用的 IP 地址。  系统具有 14 天的初始学习期限，在此期间它将学习新用户和新租户的登录行为。

我们建议你联系用户，验证他们实际上是否已从标记为可疑的 IP 地址登录。 此事件类型的风险级别是“中等”，因为同一 IP 地址后可能有多台设备，但仅某些设备可能导致可疑活动。 



## <a name="next-steps"></a>后续步骤
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)


