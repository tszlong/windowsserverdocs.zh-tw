---
title: 新增 Windows Server Essentials 做為成員伺服器
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1f59758b732046ea3f91acc13d60706daff5d36f
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471053"
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>新增 Windows Server Essentials 做為成員伺服器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本主題適用于執行 Windows Server 2012 R2 Standard、Windows Server 2012 R2 Datacenter 或 Windows Server 2016 且已安裝 Windows Server Essentials 體驗角色的伺服器。 在本文件的其餘部分，Windows Server Essentials 體驗角色會稱為 Windows Server Essentials。

> [!NOTE]
>   Windows Server Essentials 只能部署為網域控制站。 在本檔中，Windows Server Essentials 不包含 Windows Server Essentials。

 Windows Server Essentials 不需要是 Windows 網域內的主要伺服器。 您可以將 Windows Server Essentials 加入現有的 Active Directory 網域環境成為成員伺服器，然後利用它所提供的簡單資料保護、安全遠端存取，以及雲端整合功能。 此外，Windows Server Essentials 不需要是網域控制站，就可以部署在現有的 Active Directory 環境中。 這可讓您擴充儲存空間，或是利用分公司進行本機存放和系統管理。

 您可以在下列情況新增 Windows Server Essentials：

-   在分公司地點新增 Windows Server Essentials，並且使用原生工具將它加入位於不同地點之總公司的網域控制站。 您可以在此成員伺服器上開啟 BranchCache 功能以使用最佳的頻寬。

-   在 Windows Server Essentials 網路內新增 Windows Server Essentials 做為成員伺服器，藉由在您的成員伺服器上新增額外的伺服器資料夾，協助擴充網路上的存放裝置。

-   如果執行 Windows Server Essentials 的主伺服器裝載在 Microsoft Azure 中，或由協力廠商主機服務提供者裝載，請將 Windows Server Essentials 新增為本機 office 中的成員伺服器。 在當地分公司讓 Windows Server Essentials 成為成員伺服器有助於最佳化頻寬使用量。

## <a name="adding-windows-server-essentials-as-a-member-server"></a>新增 Windows Server Essentials 為成員伺服器
 若要在現有的 Active Directory 環境中，將 Windows Server Essentials 新增為執行 Windows Server 2012 R2 或 Windows Server Essentials 的主伺服器，您必須完成下列步驟：

1.  將執行 Windows Server Essentials 的伺服器加入工作群組。

2.  將執行 Windows Server Essentials 的伺服器加入主要 Windows Server Essentials 伺服器的網域。

3.  從伺服器管理員設定 Windows Server Essentials 體驗。

#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>將 Windows Server Essentials 加入工作群組或網域

1. 在第二部伺服器上完成 Windows Server Essentials 安裝之後，關閉「設定 Windows Server Essentials 精靈」。

2. 在 [搜尋]**** 方塊中，輸入 **System Settings**，然後在搜尋結果中，按一下 [檢視進階系統設定]****。

3. 在 [系統內容]**** 中，按一下 [電腦名稱]**** 索引標籤。

4. 在 [電腦名稱]**** 的 [網域]**** 區段中，按一下 [變更]****。

5. 在 [**電腦名稱稱/網域變更**] 的 [**成員**] 區段中，選擇是否要將執行 Windows server Essentials 的伺服器加入**工作組**或**網域**。

   -   若要將伺服器加入工作群組，請輸入 **workgroup**，然後按一下 [確定]****。

   -   若要將這部伺服器加入現有的 Active Directory 網域，請輸入網域名稱，然後按一下 [確定]****。

6. 重新啟動伺服器以套用變更。

   將伺服器加入主伺服器的網域之後，您可以從伺服器管理員執行 [設定 Windows Server Essentials Wizard]，繼續設定 Windows Server Essentials。

#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>在成員伺服器上設定 Windows Server Essentials 體驗

1.  (選擇性) 如果需要，變更伺服器名稱。

    > [!IMPORTANT]
    >  設定 Windows Server Essentials 體驗之後，即無法變更伺服器名稱。

2.  使用您的網域系統管理員帳戶登入伺服器。

3.  開啟 [伺服器管理員]。

4.  在 [伺服器管理員]**** 的旗標通知區域中，按一下旗標，然後按一下 [設定 Windows Server Essentials]****。

5.  選擇是否要將伺服器設定為成員伺服器，然後按 [下一步]****。

6.  按一下 [設定]**** 開始設定。 設定過程需要花費大約 10 分鐘的時間。

7.  在桌面上，按一下儀表板圖示以啟動伺服器儀表板。 在 [首頁] 頁面上，完成 [設定]**** 索引標籤上所列的 [快速入門]**** 工作。

## <a name="additional-references"></a>其他參考


-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [安裝 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

