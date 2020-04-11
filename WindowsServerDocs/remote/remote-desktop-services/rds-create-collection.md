---
title: 建立遠端桌面服務集合
description: 了解如何將 RDSH 和 RemoteApp 程式新增至您的 RDS 部署。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/22/2019
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 6a842c7984dc63fe40c05300f6cfbb6718846525
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852951"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>建立遠端桌面服務集合，以執行桌面和應用程式

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以使用下列步驟來建立遠端桌面服務工作階段集合。 工作階段集合會保存您要提供給使用者的桌面與應用程式。 建立集合之後，請將它發佈，以便使用者存取。

建立集合之前，您必須先決定需要何種集合：集區桌面工作階段或個人桌面工作階段。 

- **針對工作階段型虛擬化，使用集區桌面工作階段**：利用 Windows Server 的運算能力，提供符合成本效益的多重工作階段環境，以提升使用者的日常工作負載
- **使用個人桌面工作階段，建立虛擬桌面基礎結構 (VDI)** ：利用 Windows 用戶端，提供符合使用者期望之 Windows 桌面體驗的高效能、應用程式相容性及熟悉度 。
 
使用集區工作階段時，多位使用者可以存取資源的共用集區，而使用個人桌面工作階段時，使用者可獲得集區內的專屬桌面指派。 集區工作階段可提供較低的整體成本，而個人工作階段可讓使用者自訂他們的桌面體驗。

如果您需要共用圖形密集型託管應用程式，則可以將個人工作階段桌面與新的離散裝置指派 (DDA) 功能結合，以同時支援需要使用加速圖形的託管應用程式。 如需詳細資訊，請參閱[哪一種圖形虛擬化技術適合您？](rds-graphics-virtualization.md)


不論您選擇哪種集合類型，都要使用 RemoteApps 填入這些集合，其包含的程式和資源可讓使用者從任何支援的裝置進行存取，並如本機執行程式般加以使用。

## <a name="create-a-pooled-desktop-session-collection"></a>建立集區桌面工作階段集合

1.  在 [伺服器管理員] 中，按一下 [遠端桌面服務] > [集合] > [工作] > [建立工作階段集合]  。  
2.  輸入集合的名稱，例如 **ContosoAps**。  
3.  選取您建立的 RD 工作階段主機伺服器 (例如 Contoso-Shr1)。  
4.  接受預設的**使用者群組**。  
5.  輸入您為這個集合所建立使用者設定檔磁碟的檔案共用位置 (例如 **\Contoso-Cb1\UserDisksr**)。   
6.  按一下 [建立]  。 建立集合之後，請按一下 [關閉]  。  


## <a name="create-a-personal-desktop-session-collection"></a>建立個人桌面工作階段集合

您可以使用 New-RDSessionCollection Cmdlet 來建立個人工作階段桌面集合。 下列三個參數提供個人工作階段桌面所需的設定資訊：

- **-PersonalUnmanaged** - 指定可讓您將使用者指派給個人工作階段主機伺服器的工作階段集合類型。 如果您未指定此參數，則系統會將集合建立為傳統的「RD 工作階段主機」集合；其會在使用者登入時將他們指派給下一個可用的工作階段主機。
- **-GrantAdministrativePrivilege** - 指定要將系統管理權限授與指派給工作階段主機的使用者 (如果您使用 **-PersonalUnmanaged** 的話)。 如果您未使用此參數，則只會將標準使用者權限授與使用者。
- **-AutoAssignUser** - 指定將透過 RD 連線代理人連線之新使用者自動指派給未指派的工作階段主機 (如果您使用 **-PersonalUnmanaged** 的話)。 如果在集合中有任何未指派的工作階段主機，則使用者會看到一則錯誤訊息。 如果您未使用此參數，則必須在使用者登入前[手動將使用者指派給工作階段主機](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host)。

您可以使用 PowerShell Cmdlet 來管理您的個人桌面工作階段集合。 如需詳細資訊，請參閱[管理您的個人桌面工作階段集合](rds-manage-personal-collection.md)。

## <a name="publish-remoteapp-programs"></a>發佈 RemoteApp 程式
使用下列步驟，發佈集合中的應用程式和資源：

1.  在 [伺服器管理員] 中，選取新的集合 (**ContosoApps**)。  
2.  在 [RemoteApp 程式] 下方，按一下 [發佈 RemoteApp 程式]  。  
3. 選取您要發佈的程式，然後按一下 [發佈]  。  
