---
title: "混合标识所需的端口和协议 - Azure | Microsoft 文档"
description: "此技术参考页面描述了需要为 Azure AD Connect 打开的端口"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 935246ad590af0dcdb6d52257a94169f94dd66e5
ms.openlocfilehash: 3ff7256578cffd30ae9b75ead2dd7f08babca171
ms.lasthandoff: 02/17/2017


---
# <a name="hybrid-identity-required-ports-and-protocols"></a>混合标识所需的端口和协议
以下文档是用于实现混合标识解决方案所需的端口和协议的技术参考。 使用下图并参考相应的表格。

![什么是 Azure AD Connect](./media/active-directory-aadconnect-ports/required3.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>表 1 - Azure AD Connect 和本地 AD
此表描述了 Azure AD Connect 服务器与本地 AD 之间通信所需的端口和协议。

| 协议 | 端口 | 说明 |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |在目标林中进行 DNS 查找。 |
| Kerberos |88 (TCP/UDP) |对 AD 林进行 Kerberos 身份验证。 |
| MS-RPC |135 (TCP/UDP) |该端口绑定到 AD 林后，将在初始配置 Azure AD Connect 向导期间使用。 |
| LDAP |389 (TCP/UDP) |用于从 AD 导入数据。 数据将使用 Kerberos 签名和签章加密。 |
| LDAP/SSL |636 (TCP/UDP) |用于从 AD 导入数据。 数据传输经过签名和加密。 仅当你使用 SSL 时才使用该端口。 |
| RPC |49152-65535（随机高 RPC 端口）(TCP/UDP) |该端口绑定到 AD 林后，将在初始配置 Azure AD Connect 期间使用。 有关详细信息，请参阅 [KB929851](https://support.microsoft.com/kb/929851)、[KB832017](https://support.microsoft.com/kb/832017) 和 [KB224196](https://support.microsoft.com/kb/224196)。 |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>表 2 - Azure AD Connect 和 Azure AD
此表描述了 Azure AD Connect 服务器与 Azure AD 之间通信所需的端口和协议。

| 协议 | 端口 | 说明 |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |用于下载 CRL（证书吊销列表）以验证 SSL 证书。 |
| HTTPS |443 (TCP/UDP) |用来与 Azure AD 同步。 |

有关需要在防火墙中打开的 URL 和 IP 地址列表，请参阅 [Office 365 URLs and IP address ranges](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)（Office 365 URL 和 IP 地址范围）。

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>表 3 - Azure AD Connect 和 AD FS 联合身份验证服务器/WAP
此表描述了 Azure AD Connect 服务器与 AD FS 联合身份验证服务器/WAP 服务器之间通信所需的端口和协议。  

| 协议 | 端口 | 说明 |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |用于下载 CRL（证书吊销列表）以验证 SSL 证书。 |
| HTTPS |443 (TCP/UDP) |用来与 Azure AD 同步。 |
| WinRM |5985 |WinRM 侦听器 |

## <a name="table-4---wap-and-federation-servers"></a>表 4 - WAP 和联合服务器
此表描述了联合服务器与 WAP 服务器之间通信所需的端口和协议。

| 协议 | 端口 | 说明 |
| --- | --- | --- |
| HTTPS |443 (TCP/UDP) |用于身份验证。 |

## <a name="table-5---wap-and-users"></a>表 5 - WAP 和用户
此表描述了用户与 WAP 服务器之间通信所需的端口和协议。

| 协议 | 端口 | 说明 |
| --- | --- | --- |
| HTTPS |443 (TCP/UDP) |用于设备身份验证。 |
| TCP |49443 (TCP) |用于证书身份验证。 |

## <a name="table-6a--6b---pass-through-authentication-with-single-sign-on-sso-and-password-hash-sync-with-single-sign-on-sso"></a>表 6a 和 6b - 通过单一登录 (SSO) 进行直通身份验证和通过单一登录 (SSO) 进行密码哈希同步
下表描述了 Azure AD Connect 与 Azure AD 之间通信所需的端口和协议。

### <a name="table-6a---pass-through-authentication-with-sso"></a>表 6a - 通过 SSO 进行直通身份验证
|协议|端口号|说明
| --- | --- | ---
|HTTP|80|启用出站 HTTP 流量以进行安全验证，例如 SSL。
|HTTPS|443|    针对 Azure AD 启用用户身份验证
|HTTPS|10100–10120|    启用连接器向 Azure AD 返回响应。 
|Azure 服务总线|9352、5671|    为传入请求启用连接器到 Azure 服务之间的通信。
|HTTPS|9350|    可选，提高传入请求的性能
|HTTPS|8080/443|    启用连接器启动序列和连接器自动更新
|HTTPS|9090|    启用连接器注册（只有连接器注册过程才需要）
|HTTPS|9091|    启用连接器信任证书自动续订

### <a name="table-6b---password-hash-sync-with-sso"></a>表 6b - 通过 SSO 进行密码哈希同步

|协议|端口号|说明
| --- | --- | ---
|HTTPS|9090|    启用 SSO 注册（只有 SSO 注册过程才需要）。

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>表 7a 和 7b - 适用于 (AD FS/Sync) 和 Azure AD 的 Azure AD Connect Health 代理
下表描述了在 Azure AD Connect Health 代理与 Azure AD 之间通信所需的终结点、端口和协议

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>表 7a - 适用于 (AD FS/Sync) 和 Azure AD 的 Azure AD Connect Health 代理的端口和协议
此表描述了 Azure AD Connect Health 代理与 Azure AD 之间通信所需的以下出站端口和协议。  

| 协议 | 端口 | 说明 |
| --- | --- | --- |
| HTTPS |443 (TCP/UDP) |出站 |
| Azure 服务总线 |5671 (TCP/UDP) |出站 |

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>7b - 适用于 (AD FS/Sync) 和 Azure AD 的 Azure AD Connect Health 代理的终结点
有关终结点的列表，请参阅 [Azure AD Connect Health 代理的要求部分](../connect-health/active-directory-aadconnect-health-agent-install.md#requirements)。


