---
title: 解決 vRSS 問題
description: 如果您看不到 [vRSS] [將流量負載平衡到 VM] LPs，請解決 [vRSS] 問題。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: 850aa376e8cd0060992573561a0c32af563b88ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405153"
---
## <a name="resolve-vrss-issues"></a>解決 vRSS 問題

如果您已完成所有準備步驟，但仍未看到 [vRSS] 將流量負載平衡至 VM LPs，則會有不同的可能問題。

1. 在您執行準備步驟之前，已停用 vRSS，現在必須啟用。 您可以執行**VMNetworkAdapter**來啟用 VM 的 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. 已在 VM 或主機 vNIC 上停用 RSS。 Windows Server 2016 預設會啟用 RSS;有人可能已停用它。 

   - Enabled = **True**

   **查看目前的設定：** 

   \(針對 vm\)中的 vRSS 或主機 vNIC vrss\)的主機\(，在 vm 中執行下列 PowerShell Cmdlet。

   ```PowerShell
   Get-NetAdapterRss
   ```

   **啟用功能：** 

   若要將值從 False 變更為 True，請執行下列 PowerShell Cmdlet。

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   另一個全系統設定 RSS 的方式是使用 netsh。 用法 
   
    ```cmd
   netsh int tcp show global
   ```
   
   以確保不會全域停用 RSS。 並在必要時加以啟用。 *-Set-netadapterrss 不觸及這項設定。

3. 如果您在設定 vRSS 之後發現未啟用 VMMQ，請在連接至虛擬交換器的每個介面卡上確認下列設定：

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq-已啟用](../../media/vmmq-enabled.png)

   **查看目前的設定：** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **啟用功能：** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _（Windows Server 2019）_ 將**VrssQueueSchedulingMode**設定為 [**動態**] 時，您無法啟用 VMMQ （VmmqEnabled = False）。 啟用 VMMQ 之後，VrssQueueSchedulingMode 不會變更為動態。<p>啟用 VMMQ 時， **Dynamic**的**VrssQueueSchedulingMode**需要驅動程式支援。  VMMQ 是卸載邏輯處理器上的封包位置，因此需要驅動程式支援以利用動態演算法。  請安裝支援動態 VMMQ 的 NIC 廠商驅動程式和固件。



---
