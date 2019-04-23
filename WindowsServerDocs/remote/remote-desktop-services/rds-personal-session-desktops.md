---
title: 使用個人的工作階段桌面與遠端桌面服務
description: 了解如何共用個人化、 指派透過 RDS 的桌面
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 41f6a28c1b754a5277a0966a87dae08a6aa34e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875949"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>使用個人的工作階段桌面與遠端桌面服務

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用個人的工作階段桌面部署伺服器為基礎的雲端運算環境中的個人桌面。  （雲端運算的環境有 fabric HYPER-V 伺服器和客體虛擬機器，例如 Microsoft Azure 雲端或 Microsoft 雲端平台之間的分隔）。個人的工作階段桌面功能延伸來建立新的工作階段集合類型的遠端桌面服務工作階段型桌面部署案例，其中每位使用者指派給自己個人工作階段主機具有系統管理權限。 

使用下列資訊來建立和管理個人的工作階段桌面集合。

## <a name="create-a-personal-session-desktop-collection"></a>建立個人的工作階段桌面集合

您可以使用新增 RDSessionCollection cmdlet 來建立個人的工作階段桌面集合。 下列三個參數提供適用於個人的工作階段桌面所需的組態資訊：

- **-PersonalUnmanaged** -指定可讓您的工作階段集合的型別會將使用者指派給個人的工作階段主機伺服器。 如果您未指定此參數，做為傳統的 RD 工作階段主機集合，其中使用者已指派給下一個可用的工作階段主機在登入時建立集合。
- **-GrantAdministrativePrivilege** -如果您使用 **-PersonalUnmanaged**，指定使用者指派給工作階段主機有系統管理權限。 如果您不使用這個參數，則使用者會授與只是標準使用者權限。
- **-AutoAssignUser** -如果您使用 **-PersonalUnmanaged**，指定新的使用者透過 RD 連線代理人連線會自動指派至未指派的工作階段主機。 如果在集合中不有任何未指派的工作階段主機，則使用者會看到一則錯誤訊息。 如果您不使用這個參數，您必須[手動將使用者指派給工作階段主機](#manually-assign-a-user-to-a-personal-session-host)他們登入。

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>以手動方式將使用者指派給個人的工作階段主機
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
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>移除使用者指派從個人的工作階段主機
使用**移除 RDPersonalSessionDesktopAssignment**指令程式可移除個人的工作階段桌面和使用者之間的關聯。 此 cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

使用者\<字串\>

**– 強制**強制執行，而不要求使用者確認命令。

## <a name="query-user-assignments"></a>查詢使用者指派
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

## <a name="hardware-accelerated-graphics"></a>啟用硬體加速的圖形
Windows Server 2016 會擴充的 RemoteFX 3D 圖形介面卡 (vGPU) 技術來支援 OpenGL，並支援單一使用者的 Windows Server 2016 客體 Vm。 您可以將個人的工作階段桌面結合新 vGPU 功能以支援需要加速的圖形的裝載應用程式。 或者，您可以將個人的工作階段桌面結合新的不連續的裝置指派 (DDA) 功能，也需要加速的圖形的裝載應用程式提供支援。
