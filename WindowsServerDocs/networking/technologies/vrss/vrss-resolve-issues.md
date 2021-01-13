---
title: 解決 vRSS 問題
description: 如果您沒有看到 vRSS 將流量負載平衡至 VM LPs，請解決 vRSS 問題。
ms.topic: article
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/04/2018
ms.openlocfilehash: e0764c3df4ba998936962a619f10180cd9e42bfc
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177417"
---
# <a name="resolve-vrss-issues"></a>解決 vRSS 問題

如果您已完成所有準備步驟，但仍未看到 vRSS 將流量負載平衡至 VM LPs，則會有不同的可能問題。

1. 在您執行準備步驟之前，已停用 vRSS，現在必須啟用。 您可以執行 **VMNetworkAdapter** 來為 VM 啟用 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. 已在 VM 或主機 vNIC 上停用 RSS。 Windows Server 2016 預設會啟用 RSS;有人可能已將它停用。

   - Enabled = **True**

   **查看目前的設定：**

   在 VM 中執行下列 PowerShell Cmdlet， \( 或在主機上為 \) \( 主機 vNIC 的 vrss 執行主機 \) 。

   ```PowerShell
   Get-NetAdapterRss
   ```

   **啟用此功能：**

   若要將值從 False 變更為 True，請執行下列 PowerShell Cmdlet。

   ```PowerShell
   Enable-NetAdapterRss *
   ```

   另一個全系統設定 RSS 的方法是使用 netsh。 使用

    ```cmd
   netsh int tcp show global
   ```

   以確保不會全域停用 RSS。 並視需要加以啟用。 *-Get-netadapterrss 不會觸及這項設定。

3. 如果您在設定 vRSS 之後發現 VMMQ 未啟用，請在連接至虛擬交換器的每個介面卡上，確認下列設定：

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![PowerShell 視窗的螢幕擷取畫面，其中顯示連接至虛擬交換器的每個介面卡上的設定。](../../media/vmmq-enabled.png)

   **查看目前的設定：**

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **啟用此功能：**

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```

4. _(Windows Server 2019)_ 將 **VrssQueueSchedulingMode** 設定為 **動態** 時，無法啟用 VMMQ (VmmqEnabled = False) 。 啟用 VMMQ 之後，VrssQueueSchedulingMode 不會變更為動態。<p>啟用 VMMQ 時，**動態** **VrssQueueSchedulingMode** 需要驅動程式支援。  VMMQ 是卸載邏輯處理器上的封包位置，因此需要驅動程式支援才能運用動態演算法。  請安裝支援動態 VMMQ 的 NIC 廠商驅動程式和固件。



---
