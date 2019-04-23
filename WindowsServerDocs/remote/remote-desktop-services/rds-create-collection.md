---
title: 建立遠端桌面服務集合
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
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 52806e28c4ef87453995728623efe2954a76dfd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839239"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>建立桌面和應用程式執行的遠端桌面服務集合

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用下列步驟來建立遠端桌面服務工作階段集合。 工作階段集合會保存您想要提供給使用者的桌面與應用程式。 建立集合之後，將將它發行，以便使用者可以存取它。

建立集合之前，您必須決定需要何種集合： 共用桌面工作階段或個人的桌面工作階段。 

- **使用集區的桌面工作階段的工作階段為基礎的虛擬化**:利用 Windows Server 的計算能力，提供符合成本效益的多重工作階段環境，來驅動您的使用者的日常工作負載
- **若要建立虛擬桌面基礎結構 (VDI) 中使用的個人桌面工作階段**:利用 Windows 用戶端，以提供高效能和應用程式相容性，也都是您的使用者的 Windows 桌面體驗的熟悉度。
 
集區的工作階段中，使用多個使用者存取的共用集區的資源，而是使用個人的桌面工作階段中，使用者都會被指派自己桌面的集區中。 集區的工作階段會提供較低的整體成本，而個人的工作階段可讓使用者自訂他們的桌面體驗。

如果您需要使用大量圖形的裝載共用應用程式，您可以結合個人的工作階段桌面圖形加速設定 RemoteFX vgpu。 或者，您可以將個人的工作階段桌面結合新的不連續的裝置指派 (DDA) 功能，也需要加速的圖形的裝載應用程式提供支援。 請參閱[哪一種圖形虛擬化技術適合您](rds-graphics-virtualization.md)如需詳細資訊。


不論您選擇的集合型別，您將會填入與 RemoteApps-程式和資源，使用者可以從任何支援的裝置存取，並如同本機執行程式使用這些集合。

## <a name="create-a-pooled-desktop-session-collection"></a>建立集區的桌面工作階段集合

1.  在 [伺服器管理員] 中，按一下**遠端桌面服務 > 集合 > 工作 > 建立工作階段集合**。  
2.  輸入集合的名稱，例如**ContosoAps**。  
3.  選取您建立 （例如，Contoso Shr1） 的 RD 工作階段主機伺服器。  
4.  接受預設值**使用者群組**。  
5.  輸入您為使用者設定檔磁碟，這個集合建立的檔案共用位置 (例如**\Contoso-Cb1\UserDisksr**)。   
6.  按一下 [建立] 。 建立集合時，按一下**關閉**。  


## <a name="create-a-personal-desktop-session-collection"></a>建立個人的桌面工作階段集合

您可以使用新增 RDSessionCollection cmdlet 來建立個人的工作階段桌面集合。 下列三個參數提供適用於個人的工作階段桌面所需的組態資訊：

- **-PersonalUnmanaged** -指定可讓您的工作階段集合的型別會將使用者指派給個人的工作階段主機伺服器。 如果您未指定此參數，做為傳統的 RD 工作階段主機集合，其中使用者已指派給下一個可用的工作階段主機在登入時建立集合。
- **-GrantAdministrativePrivilege** -如果您使用 **-PersonalUnmanaged**，指定使用者指派給工作階段主機有系統管理權限。 如果您不使用這個參數，則使用者會授與只是標準使用者權限。
- **-AutoAssignUser** -如果您使用 **-PersonalUnmanaged**，指定新的使用者透過 RD 連線代理人連線會自動指派至未指派的工作階段主機。 如果在集合中不有任何未指派的工作階段主機，則使用者會看到一則錯誤訊息。 如果您不使用這個參數，您必須[手動將使用者指派給工作階段主機](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host)他們登入。

您可以使用 PowerShell cmdlet 來管理您的個人桌面工作階段集合。 請參閱[管理您的個人桌面工作階段集合](rds-manage-personal-collection.md)如需詳細資訊。

## <a name="publish-remoteapp-programs"></a>發行 RemoteApp 程式
若要在集合中發佈應用程式和資源使用下列步驟：

1.  在 [伺服器管理員] 中，選取新的集合 (**ContosoApps**)。  
2.  在 RemoteApp 程式，按一下**發佈的 RemoteApp 程式**。  
3. 選取您想要發佈，然後按一下 [的程式]**發佈**。  
