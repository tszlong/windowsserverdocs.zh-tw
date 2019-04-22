---
title: NIC 進階內容
description: 您可以管理 Nic 和透過 Windows PowerShell 或 [網路] 控制台中的所有功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: d1a5fb57bf71fd981e001cfd9ac595ab5bc3cfc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819899"
---
# <a name="nic-advanced-properties"></a>NIC 進階內容

您可以管理 Nic 和透過 Windows PowerShell 中使用的所有功能[NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) cmdlet。  您也可以管理 Nic 和使用 「 網路控制台 (ncpa.cpl) 的所有功能。 

1. 在  **Windows PowerShell**，請執行`Get‑NetAdapterAdvancedProperties`cmdlet 針對兩個不同品牌/型號的 Nic。

   ![Get-NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   有一些相似之處和差異這些兩個 NIC 進階屬性清單。

2. 在 **網路控制項台中**(ncpa.cpl)，執行下列動作：

   a. 以滑鼠右鍵按一下 nic。

   ![網路連線 對話方塊](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. 在 [屬性] 對話方塊中，按一下**設定**。

    ![C1 屬性](../../media/network-offload-and-optimization/c1-properties.png)

   c.  按一下 **進階**索引標籤，檢視 進階 屬性。<p>這份清單中的項目相互關聯中的項目`Get-NetAdapterAdvancedProperties`輸出。

   ![Chelsio 網路介面卡內容](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---