<properties
	pageTitle="使用 Azure AD 应用程序代理中的自定义域"
	description="介绍如何使用 Azure AD 应用程序代理中的自定义域。"
	services="active-directory"
	documentationCenter=""
	authors="rkarlin"
	manager="msStevenPo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="08/06/2015"
	wacn.date=""/>

# 使用 Azure AD 应用程序代理中的自定义域
> [AZURE.NOTE]应用程序代理是一项仅当升级到高级版或基本版的 Azure Active Directory 才可用的功能。有关详细信息，请参阅 [Azure Active Directory 版本](https://msdn.microsoft.com/library/azure/dn532272.aspx)。

通过使用默认域，你能够将相同的 URL 设置为用于访问应用程序的内部和外部 URL，以便不管从哪里访问，你的用户只有一个需要记住来访问应用的 URL，并且还可以使你能够在应用程序的访问面板中创建一个快捷方式。如果使用 Azure AD 应用程序代理提供的默认域，则无需额外配置即可启用你的域。如果使用自定义域，则需要执行几个事项，以确保应用程序代理能够识别你的域并验证其证书。

## 选择自定义域

1. 使用应用程序代理根据发布应用程序的说明发布应用程序。
2. 应用程序显示在应用程序列表中之后，选择该应用程序并单击“配置”。
3. 在“外部 URL”下，输入自定义域。
4. 如果外部 URL 是 https，系统则提示上传证书，以便 Azure 可以验证应用程序的 URL。还可以上载与应用程序的外部 URL 匹配的通配符证书。此域必须位于 [Azure 已验证域](https://msdn.microsoft.com/library/azure/jj151788.aspx)列表中。Azure 必须具有应用程序的域 URL 或与应用程序的外部 URL 匹配的通配符证书。
5. 请确保添加一条 DNS 记录，此记录将内部 URL 路由到应用程序，通过使用此应用程序，你能够获得进行内部和外部访问的相同 URL 以及用户的应用程序列表中的应用程序的单个快捷方式。

## 有关使用自定义域的常见问题

问：无需再次上载的情况下，是否可以选择已上载的证书？ <br> 答：以前上载的证书已自动绑定到应用程序，并且只有一个证书匹配应用程序的主机名称。 <br> …<br> 问：如何添加证书以及已导出证书应上载什么格式？ <br> 答：然后应从应用程序配置页中上载证书。证书应为 PFX 文件。 <br> …<br> 问：是否可以使用 ECC 证书？ <br> 答：签名方法没有显式限制。 <br> …<br> 问：是否可以使用 SAN 证书？ <br> 答：可以。<br> …<br> 问：是否可以使用通配符证书？ <br> 答：可以。 <br> …<br> 问：是否可以在每个应用程序上使用不同的证书？ <br> 答：可以。除非两个应用程序共享相同的外部主机。 <br> …..<br> 问：如果我注册新的域，是否可以使用该域？ <br> 答：可以。来源域列表是租户的已验证域列表。 <br> …<br> 问：证书到期时会怎么样？ <br> 答：你会在应用程序配置页的证书部分中获取一条警告。当用户尝试访问应用程序时，会弹出安全警告。 <br> …<br> 问：如果想要替换给定应用的证书，我应该怎么做？ <br> 答：从应用程序配置页中上载新证书<br> ...<br> 问：是否可以删除并替换证书？ <br> 答：当你上载新的证书时，如果旧的证书未被另一个应用程序使用，则会自动删除它<br> ...<br> 问：当证书被吊销后，会发生什么<br> 答：不对证书执行吊销检查。当用户尝试访问应用程序时，具体取决于浏览器，可能会出现安全警告。<br> …<br> 问：是否可以使用自签名的证书？ <br> 答：可以。允许使用自签名的证书。请注意，如果使用私有证书机构，证书的 CDP（证书吊销点分发点）则应为公共。 <br> ...<br> 问：是否有地方可以查看我的租户的所有证书？ <br> 答：当前版本中不支持。<br>



## 其他资源

* [以组织身份注册 Azure](/documentation/articles/sign-up-organization)
* [Azure 标识](/documentation/articles/fundamentals-identity)

<!---HONumber=71-->