---
title: NIC 進階內容
description: 您可以透過 Windows PowerShell 或 [網路] 控制台來管理 Nic 和所有功能。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: ce9c4049ab701d647701029f41d2570b7fc8cd03
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997658"
---
# <a name="nic-advanced-properties"></a>NIC 進階內容

您可以透過 Windows PowerShell 使用[get-netadapter](/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) Cmdlet 來管理 nic 和所有功能。  您也可以使用 [網路控制台] ( # A0) 來管理 Nic 和所有功能。

1. 在**Windows PowerShell**中， `Get‑NetAdapterAdvancedProperties` 針對兩個不同的 nic 建立/型號執行 Cmdlet。

   ![NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   這兩個 NIC 的 Advanced Properties 清單中有相似和不同之處。

2. 在 [**網路控制台**] ( # A0) 中，執行下列動作：

   a. 以滑鼠右鍵按一下 NIC。

   ![[網路連線] 對話方塊](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. 在 [屬性] 對話方塊中，按一下 [**設定**]。

    ![C1 屬性](../../media/network-offload-and-optimization/c1-properties.png)

   c. 按一下 [ **advanced** ] 索引標籤以查看 [advanced] 屬性。<p>此清單中的專案會與輸出中的專案相互關聯 `Get-NetAdapterAdvancedProperties` 。

   ![Chelsio 網路介面卡屬性](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---