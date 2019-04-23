---
title: 管理 RDS 在個人的桌面工作階段集合
description: 了解如何新增及 RDSH 和 RemoteApp 程式，您的 RDS 部署。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865719"
---
## <a name="manage-your-personal-desktop-session-collections"></a>管理您的個人桌面工作階段集合

您可以使用下列資訊來管理遠端桌面服務的個人桌面工作階段集合。

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>以手動方式將使用者指派給個人的工作階段主機
使用**組 RDPersonalSessionDesktopAssignment** cmdlet 來手動將使用者指派給集合中的個人的工作階段主機伺服器。 此 cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\> 

使用者\<字串\>

-Name \<string\>

- **– CollectionName** -指定個人的工作階段桌面集合的名稱。 此為必要參數。
- **– ConnectionBroker** -指定您的遠端桌面部署的遠端桌面連線代理人 （RD 連線代理人） 伺服器。 如果您未提供值，此 cmdlet 會使用本機電腦的完整的網域名稱 (FQDN)。
- **– 使用者**-指定個人的工作階段桌面，以 DOMAIN\User 的格式相關聯的使用者帳戶。 此為必要參數。
- **– 名稱**-指定工作階段主機伺服器的名稱。 此為必要參數。 此處列出的工作階段主機必須是集合的成員， **-CollectionName**參數指定。

**匯入 RDPersonalSessionDesktopAssignment** cmdlet 從文字檔匯入的使用者帳戶和個人的工作階段的桌上型電腦之間的關聯。 此 cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string>

**-路徑**指定要匯入之檔案的路徑和檔案名稱。
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>移除使用者指派從個人的工作階段主機
使用**移除 RDPersonalSessionDesktopAssignment**指令程式可移除個人的工作階段桌面和使用者之間的關聯。 此 cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

使用者\<字串\>

**– 強制**強制執行，而不要求使用者確認命令。

### <a name="query-user-assignments"></a>查詢使用者指派
使用**Get RDPersonalSessionDesktopAssignment** cmdlet 來取得一份個人的工作階段桌面和相關聯的使用者帳戶。 此 cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

使用者\<字串\>

-Name \<string\>

集合名稱、 使用者名稱或桌面工作階段的名稱，您可以執行此指令程式來查詢。 如果您只有指定 **– CollectionName**參數，cmdlet 會傳回一份工作階段主機和相關聯的使用者。 當您同時指定 **– 使用者**參數，傳回與該使用者相關聯的工作階段主機。 如果您提供 **– 名稱**參數，傳回與該工作階段主機相關聯的使用者。 


**匯出 RDPersonalPersonalDesktopAssignment** cmdlet 會將使用者和個人虛擬桌面的目前關聯匯出到文字檔。 此 cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string\>


所有新的 cmdlet 支援一般參數:-Verbose、-Debug、-ErrorAction、-ErrorVariable、-OutBuffer 和-OutVariable。 如需詳細資訊，請參閱 [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216)。
