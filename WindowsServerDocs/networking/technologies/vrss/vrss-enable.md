---
title: 啟用 vRSS 虛擬網路介面卡
description: 本主題中，您會學習如何使用 裝置管理員 或 Windows PowerShell 來啟用 vRSS Windows Server 中。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19e8011fb98b84c20e8237792664551d2362d589
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882679"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>啟用 vRSS 虛擬網路介面卡

>適用於：Windows Server （半年通道），Windows Server 2016

虛擬 RSS \(vRSS\)需要虛擬機器佇列\(VMQ\)支援從實體介面卡。 如果 VMQ 已停用或不支援然後虛擬接收端調整已停用。 

如需詳細資訊，請參閱 <<c0> [ 計劃使用 vRSS](vrss-plan.md)。

## <a name="enable-vrss-on-a-vm"></a>啟用 VM 上的 vRSS
 
您可以使用下列程序，藉由使用 Windows PowerShell] 或 [裝置管理員啟用 vRSS。

-   裝置管理員
-   Windows PowerShell
  
### <a name="device-manager"></a>裝置管理員

您可以使用此程序，使用裝置管理員啟用 vRSS。

>[!NOTE]
>此程序的第一個步驟是特有的 Vm 執行 Windows 10 或 Windows Server 2016。 如果您的 VM 執行不同的作業系統，您可以開啟裝置管理員第一次開啟 [控制台]，然後尋找並開啟裝置管理員。
  
1.  VM 在工作列上，在**這裡輸入以搜尋**，型別**裝置**。 

2.  在搜尋結果中，按一下**裝置管理員**。

3.  在 [裝置管理員] 中，按一下以展開**網路介面卡**。 

4.  以滑鼠右鍵按一下您想要設定，然後按一下 網路介面卡**屬性**。<p>網路介面卡**屬性**對話方塊隨即開啟。

5.  在 [網路介面卡**屬性**，按一下**進階**] 索引標籤。 

6.  在 **屬性**，向下捲動並按一下 **接收端調整**。 

7.  請確認在 選取項目**值**是**已啟用**。 

8.  按一下 [確定] 。
  
> [!NOTE]
> 在 [**進階**] 索引標籤，某些網路介面卡也顯示配接器所支援的 RSS 佇列數目。

---

### <a name="windows-powershell"></a>Windows PowerShell

您可以使用下列程序來使用 Windows PowerShell 啟用 vRSS。

1. 虛擬機器上，開啟**Windows PowerShell**。

2. 輸入下列命令，確保您取代*AdapterName*值 **-名稱**參數要設定，然後按 ENTER 鍵的網路介面卡的名稱。 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >或者，您可以使用下列命令來啟用 vRSS。
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

如需詳細資訊，請參閱 < [Windows PowerShell 命令，RSS 和 vRSS](vrss-wps.md)。

---