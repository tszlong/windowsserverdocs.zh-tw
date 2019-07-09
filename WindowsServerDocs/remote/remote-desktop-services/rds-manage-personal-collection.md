---
title: 管理 RDS 中的個人桌面工作階段集合
description: 了解如何將 RDSH 和 RemoteApp 程式新增至您的 RDS 部署。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 286c7ba4bd4428669d135c35c825033d22b8f40e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743520"
---
## <a name="manage-your-personal-desktop-session-collections"></a>管理您的個人桌面工作階段集合

請使用下列資訊來管理遠端桌面服務中的個人桌面工作階段集合。

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>以手動方式將使用者指派給個人工作階段主機
使用 **Set-RDPersonalSessionDesktopAssignment** Cmdlet，以手動方式將使用者指派給集合中的個人工作階段主機伺服器。 此 Cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\> 

-User \<string\>

-Name \<string\>

- **–CollectionName** - 指定個人工作階段桌面集合的名稱。 此為必要參數。
- **–ConnectionBroker** - 為您的遠端桌面部署指定遠端桌面連線代理人 (RD 連線代理人)。 若未提供此值，Cmdlet 將會使用本機電腦的完整網域名稱 (FQDN)。
- **–User** - 以「網域\使用者」的格式指定要與個人工作階段桌面產生關聯的使用者帳戶。 此為必要參數。
- **–Name** - 指定工作階段主機伺服器的名稱。 此為必要參數。 此處指定的工作階段主機必須是 **-CollectionName** 參數所指定的集合成員。

**Import-RDPersonalSessionDesktopAssignment** Cmdlet 會從文字檔中匯入使用者帳戶與個人工作階段桌面之間的關聯。 此 Cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string>

**–Path** 會指定要匯入之檔案的路徑和檔案名稱。
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>從個人工作階段主機中移除使用者指派
使用 **Remove-RDPersonalSessionDesktopAssignment** Cmdlet，可移除個人工作階段桌面與使用者之間的關聯。 此 Cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

-User \<string\>

**–Force** 會強制執行命令而不要求使用者確認。

### <a name="query-user-assignments"></a>查詢使用者指派
使用 **Get-RDPersonalSessionDesktopAssignment** Cmdlet，可取得個人工作階段桌面和相關使用者帳戶的清單。 此 Cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-User \<string\>

-Name \<string\>

您可以執行此 Cmdlet，以依集合名稱、使用者名稱或工作階段桌面名稱進行查詢。 如果您僅指定 **–CollectionName** 參數，則 Cmdlet 會傳回工作階段主機和相關使用者的清單。 當您也指定了 **–User** 參數，則會傳回與該使用者相關聯的工作階段主機。 如果您提供 **–Name** 參數，則會傳回與該工作階段主機相關聯的使用者。 


**Export-RDPersonalPersonalDesktopAssignment** Cmdlet 會將使用者與個人虛擬桌面目前的關聯匯出至文字檔。 此 Cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string\>


所有的新 Cmdlet 均支援一般參數：-Verbose、-Debug、-ErrorAction、-ErrorVariable、-OutBuffer 和 -OutVariable。 如需詳細資訊，請參閱 [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216)。
