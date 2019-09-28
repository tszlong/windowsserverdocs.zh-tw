---
title: 搭配使用個人工作階段桌面與遠端桌面服務
description: 了解如何透過 RDS 共用指派的個人化桌面。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 7429cd9cb87db310a716136c171de47cfe0892f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387357"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>搭配使用個人工作階段桌面與遠端桌面服務

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以使用個人工作階段桌面，將以伺服器為基礎的個人桌面部署到雲端運算環境中。  (雲端運算環境會分隔網狀架構 Hyper-V 伺服器與客體虛擬機器，例如 Microsoft Azure 雲端或 Microsoft 雲端平台)。個人工作階段桌面功能會延伸遠端桌面服務中以工作階段為基礎的桌面部署案例，以建立新類型的工作階段集合，而讓每個使用者都會被指派具有系統管理權限的個人工作階段主機。 

請使用下列資訊來建立和管理個人工作階段桌面集合。

## <a name="create-a-personal-session-desktop-collection"></a>建立個人工作階段桌面集合

您可以使用 New-RDSessionCollection Cmdlet 來建立個人工作階段桌面集合。 下列三個參數提供個人工作階段桌面所需的設定資訊：

- **-PersonalUnmanaged** - 指定可讓您將使用者指派給個人工作階段主機伺服器的工作階段集合類型。 如果您未指定此參數，則系統會將集合建立為傳統的「RD 工作階段主機」集合；其會在使用者登入時將他們指派給下一個可用的工作階段主機。
- **-GrantAdministrativePrivilege** - 指定要將系統管理權限授與指派給工作階段主機的使用者 (如果您使用 **-PersonalUnmanaged** 的話)。 如果您未使用此參數，則只會將標準使用者權限授與使用者。
- **-AutoAssignUser** - 指定將透過 RD 連線代理人連線之新使用者自動指派給未指派的工作階段主機 (如果您使用 **-PersonalUnmanaged** 的話)。 如果在集合中有任何未指派的工作階段主機，則使用者會看到一則錯誤訊息。 如果您未使用這個參數，則必須在使用者登入前[手動將使用者指派給工作階段主機](#manually-assign-a-user-to-a-personal-session-host)。

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>以手動方式將使用者指派給個人工作階段主機
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
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>從個人工作階段主機中移除使用者指派
使用 **Remove-RDPersonalSessionDesktopAssignment** Cmdlet，可移除個人工作階段桌面與使用者之間的關聯。 此 Cmdlet 支援下列參數：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

-User \<string\>

**–Force** 會強制執行命令而不要求使用者確認。

## <a name="query-user-assignments"></a>查詢使用者指派
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

## <a name="hardware-accelerated-graphics"></a>硬體加速圖形
Windows Server 2016 延伸了 RemoteFX 3D 圖形介面卡 (vGPU) 技術，以支援 OpenGL 和單一使用者 Windows Server 2016 客體 VM。 您可以將個人工作階段桌面與新的 vGPU 功能結合，以支援需要使用加速圖形的主控應用程式。 或者，您可以將個人工作階段桌面與新的離散裝置指派 (DDA) 功能結合，以同時支援需要使用加速圖形的託管應用程式。
