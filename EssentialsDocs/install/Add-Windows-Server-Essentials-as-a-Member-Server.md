---
title: "Windows Server Essentials 新增為成員伺服器"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8fb73f8186d3984c9e93f7a6e39cb72a54db1e58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Windows Server Essentials 新增為成員伺服器

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

此主題適用於已安裝 Windows Server Essentials 體驗角色執行 Windows Server 2012 標準 R2、Windows Server 2012 R2 Datacenter 或 Windows Server 2016 的伺服器。 本文件中的其餘部分，在 Windows Server Essentials 體驗角色將稱為 Windows Server Essentials。  
  
> [!NOTE]
>   Windows Server Essentials 只能為網域控制站部署。 本文件 Windows Server Essentials 不包含 Windows Server Essentials。  
  
 Windows Server Essentials 不需要將 Windows 網域中的主要伺服器。 您可以為成員伺服器的 Windows Server Essentials 加入現有的 Active Directory domain 環境並充分利用簡單的資料保護、遠端存取的安全，以及它所提供的雲端整合功能。 此外，Windows Server Essentials 可以在現有的 Active Directory 環境中部署，而不需要為網域控制站。 這可讓您會延伸儲存空間，或使用分公司本機存放區與管理。  
  
 您可以在下列案例中加入 Windows Server Essentials:  
  
-   加入 Windows Server Essentials 分公司位置中，它加入的網域控制站位於的原生的工具主要辦公室在不同的位置。 您可以在此成員伺服器上的最佳頻寬使用量 BranchCache 功能關閉。  
  
-   新增成員伺服器為協助延長您網路上的儲存空間透過新增額外的伺服器資料夾成員伺服器上的 Windows Server Essentials 網路在 Windows Server Essentials。  
  
-   Windows Server Essentials 新增為本機 office 成員伺服器，如果您執行的 Windows Server Essentials 的主要伺服器裝載在 Microsoft Azure 或第三方主裝載。 有 Windows Server Essentials 為最佳化頻寬使用您本機 office 網站有助於在成員伺服器。  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Windows Server Essentials 新增為成員伺服器  
 Windows Server Essentials 為成員伺服器加入現有的 Active Directory 環境中執行 Windows Server 2012 R2 或 Windows Server Essentials 的主要伺服器，您必須完成以下步驟：  
  
1.  加入到工作群組執行 Windows Server Essentials 的伺服器。  
  
2.  加入的網域主要 Windows Server Essentials 伺服器執行 Windows Server Essentials 的伺服器。  
  
3.  設定 Windows Server Essentials 體驗從伺服器管理員。  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Windows Server Essentials 加入網域或工作群組  
  
1.  完成您的第二個伺服器上安裝 Windows Server Essentials 之後，關閉 [設定 Windows Server Essentials 精靈。  
  
2.  在**搜尋**方塊中，輸入**系統設定**，並在搜尋結果中，按一下 [**檢視進階系統設定**。  
  
3.  在**系統屬性**，按一下 [**電腦名稱**索引標籤。  
  
4.  在**電腦名稱**，請在**網域**區段中，按一下 [**變更**。  
  
5.  在**電腦名稱日網域變更**，請在**成員**區段中，選擇您要加入執行以 Windows Server Essentials 的伺服器**群組**或**網域**。  
  
    -   伺服器加入位於工作群組中，輸入**群組**，然後按一下 [ **[確定]**。  
  
    -   此伺服器加入現有的 Active Directory domain 中，輸入的網域名稱，然後按一下**[確定]**。  
  
6.  重新開機伺服器適用於所做的變更。  
  
 您已加入您的主要伺服器的網域伺服器之後，您可以繼續在伺服器管理員中執行設定的 Windows Server Essentials 精靈設定 Windows Server Essentials。  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>若要設定成員伺服器上的 Windows Server Essentials 的體驗  
  
1.  （選擇性）視需要變更伺服器名稱]。  
  
    > [!IMPORTANT]
    >  設定 Windows Server Essentials 體驗之後，您無法變更伺服器名稱。  
  
2.  使用您的網域管理員過去登入伺服器。  
  
3.  打開伺服器管理員。  
  
4.  在旗標通知區域中**伺服器管理員**、 [旗標，然後**設定 Windows Server Essentials**。  
  
5.  伺服器設定為成員伺服器，選擇，然後按一下**下一步**。  
  
6.  按一下**設定**若要開始設定。 設定程序會需要約 10 分鐘的時間來完成。  
  
7.  在桌面上按一下儀表板圖示以開始伺服器儀表板。 在首頁上完成**開始**上所列之工作**設定**索引標籤。  
  
## <a name="see-also"></a>也了  
  

-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [安裝 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

