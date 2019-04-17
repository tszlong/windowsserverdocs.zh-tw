---
title: 解決 vRSS 問題
description: 如果您不會看到 vRSS 負載平衡流量 VM Lp，解決 vRSS 問題。
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
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232517"
---
## 解決 vRSS 問題

如果您已經完成所有準備步驟，但您仍然不看到 vRSS 負載平衡 VM Lp 流量，有不同可能的問題。

1. 執行準備步驟之前，vRSS 已停用-，現在必須啟用。 您可以執行**組 VMNetworkAdapter**啟用 vRSS vm。

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS 已停用在 VM 或主機 vNIC 上。 Windows Server 2016 RSS 預設啟用的;有人可能已停用它。 

   - 已啟用 = **True**

   **檢視目前的設定：** 

   在 （適用於在 VM\ vRSS) VM\ 中執行下列 PowerShell cmdlet 或主機上 \ （適用於主機 vNIC vRSS\)。

   ```PowerShell
   Get-NetAdapterRss
   ```

   **啟用功能：** 

   若要將值變更從 False，則為 True，請執行下列 PowerShell cmdlet。

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   若要設定 RSS 的另一個全系統的方式使用 netsh。 用法 
   
    ```cmd
   netsh int tcp show global
   ```
   
   若要確定該 RSS 不是全域停用。 而且如果必要啟用它。 此設定不由觸碰 *-NetAdapterRSS。

3. 如果您發現 VMMQ 不會啟用您設定 vRSS 後，，確認每個連接到虛擬交換器的介面卡上的下列設定：

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq 啟用](../../media/vmmq-enabled.png)

   **檢視目前的設定：** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **啟用功能：** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows 2019) Server_您將無法啟用 VMMQ (VmmqEnabled = False) 為**動態**設定**VrssQueueSchedulingMode**時。 一旦啟用 VMMQ VrssQueueSchedulingMode 不會變更為動態。<p>**動態** **VrssQueueSchedulingMode**需要驅動程式支援 VMMQ 啟用時。  VMMQ 是卸載邏輯處理器上封包位置的因此，需要利用動態演算法的驅動程式支援。  請先安裝 NIC 廠商的驅動程式和韌體支援動態 VMMQ。



---
