---
title: 大型部署支援
description: 深入瞭解多個網域、多個網域控制站、500使用者和500裝置的支援，以及指定指定網域控制站的能力。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ec8cea711b679c41562b1e0580430cdb264ace0f
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696936"
---
# <a name="support-for-larger-deployments"></a>大型部署支援

>適用于： Windows Server 2016 Essentials

> [!IMPORTANT]
> 本主題所描述的功能只適用于已啟用 Essentials 體驗角色的 Windows Server 2016，而不適用 Windows Server 2016 Essentials SKU。


Windows Server Essentials 現在支援更大的部署：

- 多個網域
- 多個網域控制站
- 能夠指定指定的網域控制站
- 最多可支援500使用者和500裝置

## <a name="support-for-multiple-domains"></a>支援多個網域

Windows server 2012 R2 Essentials 在每部伺服器上只支援一個網域，這是必要的，而且 Essentials 伺服器必須是樹系的根。 雖然仍然需要網域和樹系，但 Windows Server 2016 Essentials 體驗角色現在可以部署在 Windows Server 2016 Standard 或 Datacenter 上，以支援多個網域。

## <a name="support-for-multiple-domain-controllers"></a>支援多個網域控制站

 Windows Server Essentials 2012 R2 會封鎖任何利用 Azure Active Directory 的服務，例如 Microsoft 365，也就是部署一個以上的網域控制站。 原因是本機網域控制站與 Azure Active Directory 之間的帳戶和密碼同步處理可能會導致帳號憑證的密碼不同步。Windows Server 2016 Essentials 已移除這項限制。

## <a name="ability-to-specify-a-designated-domain-controller"></a>能夠指定指定的網域控制站

您現在可以選擇指定的網域控制站，以改善 Active Directory 網域物件的抓取時間，以及協調網域中其他網域控制站間的帳戶變更同步處理。

預設指定的網域控制站將會是執行 Windows Server Essentials Experience server 角色的相同伺服器。 如果該伺服器是成員伺服器，亦即它不是網域控制站，則系統會根據測試網域中的網域控制站，自動決定網域中的網域控制站是執行 Windows Server Experience Server 角色之伺服器的最低網路延遲。 如果您想要手動變更哪一台伺服器是指定的網域控制站，您可以在 [ **Windows Server Essentials 儀表板**] 的設定中進行 **設定**，如下所示。

![螢幕擷取畫面，顯示前景中的 [設定] 控制台，以及背景中的 [Windows Server Essentials 儀表板]。 目前已選取 [設定] 控制台中的 [指定的網域控制站] 頁面。](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>支援500使用者和500裝置
-------------------------------------

Windows Server 2012 R2 Essentials 中支援的使用者和裝置數目上限分別為25和50。 隨著 Windows Server Essentials 體驗伺服器角色的推出，這項限制已增加到100使用者和200裝置。

Windows Server 2016 Essentials 支援500使用者和500裝置。 這樣做可能是提供者架構和物件清單控制項的更新，因此它們會快取並快速轉譯大型的使用者和裝置物件清單。 此外，已新增搜尋和篩選功能，以快速尋找您可能要尋找的使用者或裝置 (請參閱圖 5-2) 。 您可以在 **Windows Server Essentials 儀表板**、 **遠端 Web 存取** 以及 Windows 和 Windows Phone 儲存 **My Server** 應用程式中找到搜尋和篩選功能。

![顯示使用 Windows Server Essentials 儀表板搜尋功能來搜尋字串 "d5c" 的螢幕擷取畫面。 這項搜尋的結果包括兩個檔案和資料夾，以及兩位使用者。](media/larger-deployments-2.PNG)

顯示使用 Windows Server Essentials 儀表板搜尋功能來搜尋字串 "d5c" 的螢幕擷取畫面。 這項搜尋的結果包括兩個檔案和資料夾，以及兩位使用者。

> [!NOTE]
> 當 Windows Server Essentials 伺服器角色支援的使用者和裝置限制增加時，支援的用戶端備份限制仍會維持在75。

<a name="see-also"></a>請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)