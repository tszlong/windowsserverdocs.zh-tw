---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS 與 AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d75a78119d76a0f8380967292b1d0abc720597
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813139"
---
# <a name="dns-and-ad-ds"></a>DNS 與 AD DS

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Active Directory 網域服務 (AD DS) 使用網域名稱系統 (DNS) 名稱解析服務可讓用戶端找到網域控制站和網域控制站裝載目錄服務來彼此通訊。  
  
AD DS 可讓您輕鬆整合到現有的 DNS 命名空間的 Active Directory 命名空間。 功能，例如 Active Directory 整合 DNS 區域讓您更輕鬆地部署不需要設定次要區域的 DNS，然後設定 區域轉送。  
  
DNS 如何支援 AD DS 的相關資訊，請參閱節[Active Directory 技術參照的 DNS 支援](https://go.microsoft.com/fwlink/?LinkID=48147)。  
  
> [!NOTE]  
> 如果您實作脫離的命名空間中的 AD DS 網域名稱不同於用戶端使用的主要 DNS 尾碼時，AD DS 整合 dns 是更複雜。 如需詳細資訊，請參閱 <<c0> [ 脫離的命名空間](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)。  
  
## <a name="in-this-section"></a>本節內容  
  
- [網域控制站的位置](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Active Directory 整合 DNS 區域](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [電腦命名](../../ad-ds/plan/Computer-Naming.md)  
- [脫離的命名空間](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
