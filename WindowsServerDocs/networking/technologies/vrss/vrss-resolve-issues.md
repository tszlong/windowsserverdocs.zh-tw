---
title: 解決 vRSS 問題
description: 如果您不會看到負載平衡流量到 VM LPs vRSS，請解決 vRSS 問題。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: a2d6eb43149361b4270565b63fc99f483f364f74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824029"
---
## <a name="resolve-vrss-issues"></a>解決 vRSS 問題

如果您已完成所有準備步驟，您仍然看不見 vRSS 負載平衡流量到 VM LPs 有不同的可能問題。

1. 執行準備步驟之前，vRSS 已停用-，而且現在必須已啟用。 您可以執行**Set-vmnetworkadapter**來針對 VM 啟用 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. 在 VM 中或在主機 vNIC 上，已停用 RSS。 Windows Server 2016 預設情況下，啟用 RSS有人可能已停用它。 

   - 已啟用 = **，則為 True**

   **檢視目前的設定：** 

   在 VM 中執行下列 PowerShell cmdlet\(的 VM 中的 vRSS\)或在主機上\(主機 vNIC vRSS 的\)。

   ```PowerShell
   Get-NetAdapterRss
   ```

   **啟用此功能：** 

   若要將值從 False 變更為 True，執行下列 PowerShell cmdlet。

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   若要設定 RSS 的另一個全系統的方法使用 netsh。 用法 
   
    ```cmd
   netsh int tcp show global
   ```
   
   若要確定，RSS 不是全域停用。 然後必要時加以啟用。 此設定不會接觸到的 *-NetAdapterRSS。

3. 如果您發現 VMMQ 未啟用 vRSS 設定之後，請確認每個連接至虛擬交換器的介面卡上的下列設定：

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **，則為 True**

   ![vmmq-enabled](../../media/vmmq-enabled.png)

   **檢視目前的設定：** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **啟用此功能：** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ 您無法啟用 VMMQ (VmmqEnabled = False) 時設定**VrssQueueSchedulingMode**要**動態**。 一旦啟用 VMMQ VrssQueueSchedulingMode 不會變更為動態。<p>**VrssQueueSchedulingMode**的**動態**VMMQ 啟用時需要的驅動程式支援。  VMMQ 已卸載的封包上放置邏輯處理器，而且此情況下，需要運用動態演算法的驅動程式支援。  請安裝 NIC 廠商的驅動程式和支援動態 VMMQ 的韌體。



---
