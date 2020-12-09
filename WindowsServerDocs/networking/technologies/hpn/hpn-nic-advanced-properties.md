---
title: NIC 進階內容
description: 您可以透過 Windows PowerShell 或網路主控台來管理 Nic 和所有功能。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: a08de5815a364dc39dd975a37ac3be594978ea88
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864487"
---
# <a name="nic-advanced-properties"></a>NIC 進階內容

您可以使用 [get-netadapter](/powershell/module/netadapter/) Cmdlet，透過 Windows PowerShell 管理 nic 和所有功能。  您也可以使用網路主控台 ( # A0) 來管理 Nic 和所有功能。

1. 在 **Windows PowerShell** 中， `Get‑NetAdapterAdvancedProperties` 針對兩個不同的 nic 組成/型號執行 Cmdlet。

   ![Get-NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   這兩個 NIC Advanced Properties 清單中有相似和差異。

2. 在 [ **網路主控台** ( # A0) 中，執行下列動作：

   a. 以滑鼠右鍵按一下 NIC。

   ![網路連接對話方塊](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. 在 [屬性] 對話方塊中，按一下 [ **設定**]。

    ![C1 屬性](../../media/network-offload-and-optimization/c1-properties.png)

   c. 按一下 [ **advanced （advanced** ）] 索引標籤以查看 [advanced] 屬性。<p>這份清單中的專案會與輸出中的專案相互關聯 `Get-NetAdapterAdvancedProperties` 。

   ![Chelsio 網路介面卡屬性](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---
