---
title: 啟用虛擬網路介面卡上的 vRSS
description: 本主題中，您將了解如何使用 [裝置管理員] 或 [Windows PowerShell 來啟用 Windows Server 中的 vRSS。
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133414"
---
# 啟用虛擬網路介面卡上的 vRSS

>適用於：Windows Server (半年通道)、Windows Server 2016

虛擬 RSS \(vRSS\) 需要虛擬機器佇列 \(VMQ\) 支援從實體介面卡。 如果 VMQ 已停用或不支援然後虛擬接收端調整已停用。 

如需詳細資訊，請參閱[計劃 vRSS 使用](vrss-plan.md)。

## 啟用 VM 上 vRSS
 
若要使用 Windows PowerShell 或裝置管理員啟用 vRSS 使用下列程序。

-   [裝置管理員]
-   Windows PowerShell
  
### [裝置管理員]

若要啟用 vRSS 使用 [裝置管理員，您可以使用此程序。

>[!NOTE]
>此程序的第一個步驟是針對執行 Windows 10 或 Windows Server 2016 的 Vm。 如果您的 VM 執行不同的作業系統，您可以藉由第一次開啟 [控制台]，然後尋找並開啟裝置管理員 \] 來開啟裝置管理員 \]。
  
1.  VM 在工作列上，在**這裡要搜尋的類型**中，輸入**裝置**。 

2.  在搜尋結果中，按一下 [**裝置管理員 \]**。

3.  在 [裝置管理員] 中，按一下以展開**網路介面卡**。 

4.  以滑鼠右鍵按一下您想要設定的網路介面卡，然後按一下 [**屬性**。<p>網路介面卡的 [**屬性**] 對話方塊隨即開啟。

5.  在網路介面卡**的屬性**，按一下 [**進階**] 索引標籤。 

6.  在**屬性**中，向下捲動到，並按一下 [**接收端調整**。 

7.  請確定**的值**中選取項目已**啟用**。 

8.  按一下 **\[確定\]**。
  
> [!NOTE]
> **進階**] 索引標籤中，某些網路介面卡也會顯示與介面卡支援的 RSS 佇列數目。

---

### Windows PowerShell

若要使用 Windows PowerShell 來啟用 vRSS 使用下列程序。

1. 在虛擬機器，開啟**Windows PowerShell**。

2. 輸入下列命令，以確保您取代的*AdapterName*值 **-名稱**參數搭配您想要設定，然後按 enter 鍵的網路介面卡的名稱。 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >或者，您可以使用下列命令來啟用 vRSS。
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

如需詳細資訊，請參閱[RSS 」 和 「 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---