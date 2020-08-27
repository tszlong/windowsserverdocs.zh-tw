---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS 與 AD DS
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 65e23e469c6361ac2eb614280b3f1e58e2bfff08
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941108"
---
# <a name="dns-and-ad-ds"></a>DNS 與 AD DS

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services (AD DS) 使用網域名稱系統 (DNS) 名稱解析服務，讓用戶端可以找到網域控制站，以及裝載目錄服務的網域控制站，以便彼此通訊。

AD DS 可讓 Active Directory 命名空間輕鬆地整合到現有的 DNS 命名空間中。 Active Directory 整合式 DNS 區域等功能可讓您更輕鬆地部署 DNS，方法是免除設定次要區域的需求，然後設定區域傳輸。

如需 DNS 如何支援 AD DS 的詳細資訊，請參閱 [Active Directory 技術參考的 Dns 支援](/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10))一節。

> [!NOTE]
> 如果您執行的脫離命名空間，其中 AD DS 的功能變數名稱與用戶端所使用的主要 DNS 尾碼不同，則 AD DS 與 DNS 的整合比較複雜。 如需詳細資訊，請參閱脫離的 [命名空間](Disjoint-Namespace.md)。

## <a name="in-this-section"></a>本節內容

- [網域控制站位置](Domain-Controller-Location.md)
- [Active Directory 整合 DNS 區域](Active-Directory-Integrated-DNS-Zones.md)
- [電腦命名](Computer-Naming.md)
- [斷續命名空間](Disjoint-Namespace.md)
