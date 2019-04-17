---
title: "步驟 3： 電腦加入新的 Windows Server Essentials 伺服器"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>步驟 3： 電腦加入新的 Windows Server Essentials 伺服器

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

移轉程序的下一個步驟是 client 電腦連接到執行 Windows Server Essentials 新的伺服器。  
  
> [!NOTE]
>  您可以略過此步驟執行 Windows XP 或 Windows Vista 作業系統的電腦。 Windows Server 連接器軟體不支援執行 Windows XP 或 Windows Vista 的電腦。  
  
 您可以加入新的 Windows Server Essentials 伺服器 client 的電腦之前，您必須解除安裝 Windows Server 連接器電腦上的軟體 client 將它拔除從來源伺服器。  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>若要解除安裝 client 的電腦上的 Windows Server 連接器  
  
1.  從 client 的電腦，開放 [控制台]，然後打開**程式和功能**。  
  
2.  在 [程式] 清單，以滑鼠右鍵按一下您的電腦所執行的連接器應用程式。  
  
    > [!NOTE]
    >  連接器應用程式可以**Windows 小型企業 Server 2011 Essentials 連接器**，或**Windows Server Essentials 連接器**、 client 電腦已連接到 Windows Server Essentials 的版本而定。  
  
3.  按一下**解除安裝**。  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>若要重新連接到伺服器 client 電腦  
  
1.  登入至您想要伺服器連接的電腦。  
  
    > [!NOTE]
    >  如果這台電腦有多個使用者帳號，請使用使用者帳號的文件、 圖片及個人化您想要保留您的電腦連接到伺服器之後的喜好設定來登入。  
  
2.  打開網際網路瀏覽器，例如 Internet Explorer。  
  
3.  在 [位址列中，輸入**http://<servername\>/Connect**，然後按 ENTER 鍵。  
  
4.  依照畫面上的指示 client 將電腦加入新的 Windows Server Essentials 伺服器。  
  
## <a name="next-steps"></a>後續步驟  
 您已經加入 client 電腦執行 Windows Server Essentials 的伺服器。 立即移至[執行 「 步驟 4： 移動設定和資料目的 Windows Server Essentials 伺服器移轉到](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  
  

若要檢視所有的步驟，請查看[Windows Server essentials 移轉](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

