---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS 與 AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 817c6ca168e33df1a0c59e151a303c4259313743
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624296"
---
# <a name="dns-and-ad-ds"></a>DNS 與 AD DS

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services （AD DS）使用網域名稱系統（DNS）名稱解析服務，讓用戶端可以找到網域控制站，以及裝載目錄服務以彼此通訊的網域控制站。

AD DS 可以輕鬆地將 Active Directory 命名空間整合到現有的 DNS 命名空間。 Active Directory 整合式 DNS 區域之類的功能，可讓您更輕鬆地部署 DNS，方法是排除設定次要區域的需求，然後設定區域轉送。

如需 DNS 如何支援 AD DS 的詳細資訊，請參閱[Active Directory 技術參考的 DNS 支援](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10))一節。

> [!NOTE]
> 如果您執行脫離的命名空間，其中 AD DS 的功能變數名稱與用戶端使用的主要 DNS 尾碼不同，AD DS 與 DNS 的整合會更複雜。 如需詳細資訊，請參閱脫離的[命名空間](Disjoint-Namespace.md)。

## <a name="in-this-section"></a>本節內容

- [網域控制站位置](Domain-Controller-Location.md)
- [Active Directory 整合 DNS 區域](Active-Directory-Integrated-DNS-Zones.md)
- [電腦命名](Computer-Naming.md)
- [斷續命名空間](Disjoint-Namespace.md)
