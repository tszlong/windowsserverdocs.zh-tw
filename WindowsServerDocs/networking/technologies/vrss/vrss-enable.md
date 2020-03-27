---
title: 在虛擬網路介面卡上啟用 vRSS
description: 在本主題中，您將瞭解如何使用 Device Manager 或 Windows PowerShell，在 Windows Server 中啟用 vRSS。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e4be9060da4a738e3ad8e4976d037f3a05467da3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315378"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>在虛擬網路介面卡上啟用 vRSS

>適用於：Windows Server (半年通道)、Windows Server 2016

虛擬 RSS \(vRSS\) 需要來自實體介面卡的虛擬機器佇列 \(VMQ\) 支援。 如果 VMQ 已停用或不受支援，則會停用虛擬接收端調整。 

如需詳細資訊，請參閱[規劃使用 vRSS](vrss-plan.md)。

## <a name="enable-vrss-on-a-vm"></a>在 VM 上啟用 vRSS
 
請使用下列程式，使用 Windows PowerShell 或 Device Manager 來啟用 vRSS。

-   裝置管理員
-   資料夾，然後按一下 [Windows PowerShell]
  
### <a name="device-manager"></a>裝置管理員

您可以使用這個程式，利用 Device Manager 來啟用 vRSS。

>[!NOTE]
>此程式的第一個步驟是執行 Windows 10 或 Windows Server 2016 的 Vm 專用。 如果您的 VM 執行不同的作業系統，您可以先開啟 [控制台] 開啟 Device Manager，然後尋找並開啟 [Device Manager]。
  
1.  在 VM 工作列的 [**輸入此處以搜尋**] 中，輸入**device**。 

2.  在搜尋結果中，按一下 [ **Device Manager**]。

3.  在 Device Manager 中，按一下以展開 [**網路介面卡**]。 

4.  在您要設定的網路介面卡**上按一下滑鼠**右鍵，然後按一下 [內容]。<p>[**網路介面卡內容**] 對話方塊隨即開啟。

5.  在 [網路**配接器內容] 中，** 按一下 [ **Advanced** ] 索引標籤。 

6.  在 [**屬性**] 中，向下流覽至，然後按一下 [**接收端調整**]。 

7.  確定 [**值**] 中的選取專案已**啟用**。 

8.  按一下 [確定]。
  
> [!NOTE]
> 在 [ **Advanced** ] 索引標籤上，某些網路介面卡也會顯示介面卡支援的 RSS 佇列數目。

---

### <a name="windows-powershell"></a>資料夾，然後按一下 [Windows PowerShell]

請使用下列程式，使用 Windows PowerShell 來啟用 vRSS。

1. 在虛擬機器上，開啟**Windows PowerShell**。

2. 輸入下列命令，並確保以您要設定的網路介面卡名稱取代 **-Name**參數的*AdapterName*值，然後按 enter。 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >或者，您可以使用下列命令來啟用 vRSS。
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

如需詳細資訊，請參閱[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---