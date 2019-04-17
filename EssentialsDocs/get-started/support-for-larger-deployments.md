---
title: "大部署的支援"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c60e5f73c88a225fbd1067992894f9d20da745ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
#<a name="support-for-larger-deployments"></a>大部署的支援

>適用於： Windows Server 2016 Essentials

> [!IMPORTANT]  
> 與 Essentials 體驗角色支援，而不是與 Windows Server 2016 Essentials SKU，本主題中所述的功能只有在 Windows Server 2016 上運作。


Windows Server Essentials 現在支援使用較大部署：

- 多個網域
- 多個網域控制站
- 若要指定指定的網域控制站的功能
- 500 最多使用者和 500 裝置的支援

##<a name="support-for-multiple-domains"></a>多個網域支援

Windows server 2012 R2 Essentials 支援伺服器，這是必要的每一個網域和程式集伺服器必須之子-森林根。 網域和森林仍然需要時，現在可在 Windows Server 2016 標準或 Datacenter 支援多個網域部署的 Windows Server 2016 Essentials 體驗角色。

## <a name="support-for-multiple-domain-controllers"></a>多個網域控制站的支援

 Windows Server Essentials 2012 R2 會封鎖任何利用 Azure Active Directory，例如 Office 365、 部署一個以上的網域控制站的位置的服務。 是，account 認證會導致 account 和密碼同步處理本機網域控制站和 Azure Active Directory 之間同步的密碼。 在 Windows Server 2016 Essentials 已經移除這項限制。

##<a name="ability-to-specify-a-designated-domain-controller"></a>若要指定指定的網域控制站的功能

您現在可以選擇將改善擷取次 Active Directory domain 物件，以及協調同步處理 account 變更的跨網域中的其他網域控制站的指定的網域控制站。

指定網域控制站預設將會執行 Windows Server Essentials 體驗伺服器角色相同的伺服器。 如果該伺服器成員伺服器，不網域控制站，這表示然後指定的網域控制站將會自動判斷根據測試的網域控制站網域中的預設會有到執行 Windows Server 體驗伺服器角色伺服器的最低網路延遲。 如果您要手動變更伺服器指定的網域控制站，您可以在**設定**在**Windows Server Essentials 儀表板**，如下所示。

![顯示設定的螢幕擷取畫面控制面板在前景中執行 Windows Server Essentials 儀表板中的背景。 指定網域控制站的設定 [控制台] 頁面目前已選取。](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>500 使用者和 500 裝置的支援
-------------------------------------

支援的使用者和 Windows Server 2012 R2 Essentials 的裝置數目上限是 25 和 50 分別。 Windows Server Essentials 體驗伺服器角色介紹限制該增加 100 使用者和 200 裝置。

Windows Server 2016 Essentials 支援 500 使用者和 500 裝置。 讓這可能會提供者 framework 的更新，並物件清單控制讓它們快取，並快速地呈現大型使用者和裝置物件清單。 此外，已搜尋及篩選功能新增至的使用者或裝置，您可能會尋找 （查看圖 5 2） 來快速搜尋。 您會發現中的搜尋和篩選功能**Windows Server Essentials 儀表板**，**遠端 Web 存取**，以及 Windows 和 Windows Phone 市集**我伺服器**應用程式。

![顯示 Windows Server Essentials 儀表板搜尋字串 」 d5c。 」 的搜尋功能的使用螢幕擷取畫面 搜尋結果包括有兩個檔案和資料夾和兩個使用者。](media/larger-deployments-2.PNG)

顯示 Windows Server Essentials 儀表板搜尋字串 」 d5c 」 的搜尋功能的使用螢幕擷取畫面。 搜尋結果包括有兩個檔案和資料夾和兩個使用者。

> [!NOTE]  
> 同時針對 Windows Server Essentials 伺服器角色，client 備份仍在 75 的支援限制增加支援的使用者和裝置數限制。

<a name="see-also"></a>也了
--------
[開始使用 Windows Server Essentials](get-started.md)