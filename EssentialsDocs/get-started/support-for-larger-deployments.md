---
title: 大型部署支援
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 056e41413d8e11f1f65ab17c1ba365eba2ee2d1a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310350"
---
# <a name="support-for-larger-deployments"></a>大型部署支援

>適用于： Windows Server 2016 Essentials

> [!IMPORTANT]  
> 本主題所述的功能僅適用于已啟用「Essentials 體驗」角色的 Windows Server 2016，而不適用於 Windows Server 2016 Essentials SKU。


Windows Server Essentials 現在支援更大的部署：

- 多個網域
- 多個網域控制站
- 能夠指定指定的網域控制站
- 最多可支援500使用者和500裝置

## <a name="support-for-multiple-domains"></a>支援多個網域

Windows server 2012 R2 Essentials 在每部伺服器上僅支援一個網域，這是必要的，而且 Essentials 伺服器必須是樹系的根目錄。 雖然仍然需要網域和樹系，但 Windows Server 2016 Essentials 體驗角色現在可以部署在 Windows Server 2016 Standard 或 Datacenter 上，以支援多個網域。

## <a name="support-for-multiple-domain-controllers"></a>支援多個網域控制站

 Windows Server Essentials 2012 R2 會封鎖任何利用 Azure Active Directory 的服務，例如 Office 365，其中部署了一個以上的網域控制站。 原因是本機網域控制站與 Azure Active Directory 之間的帳戶和密碼同步處理可能會導致帳號憑證的密碼不同步。Windows Server 2016 Essentials 已移除這項限制。

## <a name="ability-to-specify-a-designated-domain-controller"></a>能夠指定指定的網域控制站

您現在可以選擇指定的網域控制站，這將可改善 Active Directory 網域物件的抓取時間，以及在網域中的其他網域控制站之間協調帳戶變更的同步處理。

預設指定的網域控制站將會是執行 Windows Server Essentials 體驗伺服器角色的相同伺服器。 如果該伺服器是成員伺服器，這表示它不是網域控制站，則預設指定的網域控制站會根據測試，自動判斷網域中的網域控制站是否有最低的網路延遲，讓伺服器執行Windows Server Experience Server 角色。 如果您想要手動變更哪一台伺服器是指定的網域控制站，您可以在**Windows Server Essentials 儀表板**的 [**設定**] 中執行此動作，如下所示。

![螢幕擷取畫面，顯示前景中的 [設定] 控制台和背景中的 [Windows Server Essentials 儀表板]。 目前已選取 [設定] 控制台的 [指定的網域控制站] 頁面。](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>支援500使用者和500裝置
-------------------------------------

Windows Server 2012 R2 Essentials 中支援的使用者和裝置數目上限分別為25和50。 隨著 Windows Server Essentials Experience Server 角色的引進，此限制已增加到100個使用者和200個裝置。

Windows Server 2016 Essentials 支援500使用者和500裝置。 這可能是提供者架構和物件清單控制項的更新，因此它們會快取並快速呈現大型使用者和裝置物件清單。 此外，也新增了搜尋和篩選功能，以快速找出您要尋找的使用者或裝置（請參閱圖5-2）。 您可以在**Windows Server Essentials 儀表板**、**遠端 Web 存取**，以及 Windows 和 Windows Phone 儲存**我的伺服器**應用程式中找到搜尋和篩選功能。

![螢幕擷取畫面，顯示如何使用 Windows Server Essentials 儀表板的搜尋功能來搜尋字串 "d5c"。 此搜尋的結果包含兩個檔案和資料夾，以及兩個使用者。](media/larger-deployments-2.PNG)

螢幕擷取畫面，顯示如何使用 Windows Server Essentials 儀表板的搜尋功能來搜尋字串 "d5c"。 此搜尋的結果包含兩個檔案和資料夾，以及兩個使用者。

> [!NOTE]  
> 雖然 Windows Server Essentials 伺服器角色的支援使用者和裝置限制已增加，但用戶端備份支援的限制仍在75。

<a name="see-also"></a>另請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)