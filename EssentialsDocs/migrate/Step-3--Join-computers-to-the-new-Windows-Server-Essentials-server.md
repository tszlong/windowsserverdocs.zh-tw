---
title: 步驟 3：將電腦加入新的 Windows Server Essentials 伺服器
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f71ac280e2de0b7d945f2d979fe52d173f7c3323
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861869"
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>步驟 3：將電腦加入新的 Windows Server Essentials 伺服器

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

移轉程序的下一個步驟是將用戶端電腦連線到執行 Windows Server Essentials 的新伺服器。  
  
> [!NOTE]
>  執行 Windows XP 或 Windows Vista 作業系統的電腦可以略過這個步驟。 Windows Server 連接器軟體不支援執行 Windows XP 或 Windows Vista 的電腦。  
  
 加入新的 Windows Server Essentials 伺服器的用戶端電腦之前，您必須中斷來源伺服器解除安裝用戶端電腦上的 Windows Server 連接器軟體。  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>解除安裝用戶端電腦上的 Windows Server 連接器  
  
1.  從用戶端電腦開啟 [控制台]，然後開啟 [程式和功能]。  
  
2.  在程式清單中，以滑鼠右鍵按一下在電腦上執行的連接器應用程式。  
  
    > [!NOTE]
    >  連接器應用程式可以**Windows Small Business Server 2011 Essentials Connector**，或**Windows Server Essentials 連接器**，取決於哪一個版本的 Windows Server Essentials用戶端電腦連線到。  
  
3.  按一下 [解除安裝] 。  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>將用戶端電腦重新連線至伺服器  
  
1.  登入到您想要連線到伺服器的電腦。  
  
    > [!NOTE]
    >  如果此電腦有多個使用者帳戶，請使用在您將此電腦連線至伺服器後，要保留其文件、圖片和個人喜好設定的使用者帳戶登入。  
  
2.  開啟網際網路瀏覽器，例如 Internet Explorer。  
  
3.  在 [網址] 列中，輸入**http://<servername\>/connect**，然後按 ENTER 鍵。  
  
4.  請遵循畫面上的指示，將用戶端電腦加入新的 Windows Server Essentials 伺服器。  
  
## <a name="next-steps"></a>後續步驟  
 您已將您的用戶端電腦加入新的伺服器執行 Windows Server Essentials。 現在請移至[步驟 4:將設定和資料移至目的地伺服器的 Windows Server Essentials 移轉的](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  
  

若要檢視所有步驟，請參閱[移轉至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

