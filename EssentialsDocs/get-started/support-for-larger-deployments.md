---
title: 大型部署支援
description: 描述如何使用 Windows Server Essentials
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
ms.openlocfilehash: a99698519524c3b5050dc534d61921560522528c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433875"
---
# <a name="support-for-larger-deployments"></a>大型部署支援

>適用於：Windows Server 2016 Essentials

> [!IMPORTANT]  
> 本主題中所述的功能只能在 Windows Server 2016 Essentials 體驗角色啟用，而不是與 Windows Server 2016 Essentials SKU。


Windows Server Essentials 現在支援使用較大型的部署：

- 多個網域
- 多個網域控制站
- 若要指定在指定的網域控制站的能力
- 最多 500 個使用者和 500 個裝置的支援

## <a name="support-for-multiple-domains"></a>支援多個網域

Windows server 2012 R2 Essentials 支援的每一部伺服器，也就是必要的只有一個網域和 Essentials 伺服器必須是樹系的根。 仍然需要網域和樹系時，Windows Server 2016 Essentials 體驗角色現在可以部署在 Windows Server 2016 Standard 或 Datacenter 支援多個網域。

## <a name="support-for-multiple-domain-controllers"></a>支援多個網域控制站

 Windows Server Essentials 2012 R2 會封鎖任何服務來運用 Azure Active Directory，例如 Office 365，部署多個網域控制站的位置。 原因是，本機網域控制站與 Azure Active Directory 之間的帳戶和密碼同步處理可能會導致帳戶認證未同步的密碼。Windows Server 2016 Essentials 中已移除這項限制。

## <a name="ability-to-specify-a-designated-domain-controller"></a>若要指定在指定的網域控制站的能力

您現在可以選擇指定的網域控制站會改善 Active Directory 網域物件，擷取時間，以及協調跨網域中其他網域控制站的同步處理的帳戶變更。

您指定網域控制站的預設將會執行 Windows Server Essentials 體驗伺服器角色的同一部伺服器。 如果該伺服器是成員伺服器，這表示它不是網域控制站，則會自動決定指定的網域控制站預設會根據測試網域的網域控制站具有最低的網路延遲到執行的伺服器Windows Server 體驗伺服器角色。 如果您想要手動變更 哪部伺服器是在指定的網域控制站，則可以在**設定**中**Windows Server Essentials 儀表板**，如下所示。

![螢幕擷取畫面顯示的設定會控制面板在前景與背景的 Windows Server Essentials 儀表板。 目前選取的 [設定] 控制台中的 [指定網域控制站] 頁面。](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>500 位使用者和 500 個裝置的支援
-------------------------------------

支援的使用者和 Windows Server 2012 R2 Essentials 中的裝置數目上限是 25 到 50，分別。 隨著 Windows Server Essentials 體驗伺服器角色，該限制已增加為 100 位使用者和 200 台裝置。

Windows Server 2016 Essentials 可支援 500 位使用者及 500 個裝置。 進行這可能是提供者架構的更新和物件清單控制項，這些快取，並快速地呈現大型的使用者和裝置物件清單。 此外，搜尋和篩選功能已新增來快速找出使用者或裝置，您可能會尋求 （請參閱 圖 5-2）。 您可以找到搜尋和篩選功能**Windows Server Essentials 儀表板**，**遠端 Web 存取**，以及 Windows 和 Windows Phone 市集**My Server**應用程式。

![示範如何使用 [搜尋] 功能來搜尋字串"d5c。 」 的 Windows Server Essentials 儀表板的螢幕擷取畫面 這項搜尋的結果會包含兩個檔案和資料夾以及兩位使用者。](media/larger-deployments-2.PNG)

示範如何使用 [搜尋] 功能來搜尋字串"d5c 」 的 Windows Server Essentials 儀表板的螢幕擷取畫面。 這項搜尋的結果會包含兩個檔案和資料夾以及兩位使用者。

> [!NOTE]  
> 雖然支援的使用者和裝置限制已增加為 Windows Server Essentials 伺服器角色，用戶端備份會維持在 75 的支援限制。

<a name="see-also"></a>另請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)