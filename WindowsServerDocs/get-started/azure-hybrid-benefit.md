---
title: 適用於 Windows Server 的 Azure Hybrid Benefit
description: 使用您的內部部署 Windows Server 授權來節省使用 Azure VM
ms.prod: windows-server
ms.date: 11/10/2017
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.openlocfilehash: 62821abc6c9eec660fa6af832bb1aba151708021
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63686390"
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>適用於 Windows Server 的 Azure Hybrid Benefit

>適用於：Windows Server

## <a name="benefit-description-rules-and-use-cases"></a>Benefit 描述、規則與使用案例

適用於 Windows Server 的 Azure Hybrid Benefit 可讓您利用內部部署 Windows Server 授權搭配軟體保證，在 Azure 中節省高達 40% 的 Windows Server VM。  透過這項權益，客戶只需要支付基礎結構成本，因為軟體保證權益已涵蓋 Windows Server 的授權。  此權益適用於 Windows Server 2008R2、2012、2012R2 與 2016 版本的 Standard 和 Datacenter 版本。  這項權益在所有地區和 Sovereign 雲端都能使用。


![影像 1](media/ahb01.png)

若想符合此權益資格，您只需擁有使用中軟體保證或訂閱授權，例如 EAS、SCE 訂閱或 Windows Server 授權的 Open Value Subscription。  

每份 Windows Server 2 處理器授權搭配使用中 SA/訂閱，以及每組 16 Windows Server 核心授權搭配 SA/訂閱，能讓客戶在跨最多兩個 Azure 基本執行個體配置的高達 16 個虛擬核心上的 Microsoft Azure 使用 Windows Server。 每多一組 8 核心授權搭配 SA/訂閱，可以使用高達 8 個虛擬核心以及一個基本執行個體 (VM)。

| 授權搭配 SA/訂閱            | 授與的 VM 和核心            | 使用方式                                |
|-----------------------------------------|----------------------------------|-----------------------------------------------------|
| WS Datacenter (16 核心或 2 處理器授權)  | 最多兩個 VM 和高達 16 核心 | 可同時在內部部署和 Azure 中執行虛擬電腦  |
| WS Standard (16 核心或 2 處理器授權)    | 最多兩個 VM 和高達 16 核心 | 可在內部部署或 Azure 中執行虛擬電腦 |

運用 Azure Hybrid Benefit 的 VM 只有 SA/訂閱期間才能在 Azure 中執行。 接近 SA/訂閱到期時間時，客戶可以選擇續約 SA/訂閱、關閉該 VM 的混合式權益功能或解除佈建使用混合式權益的 VM。 

### <a name="savings-examples"></a>節省範例 

![影像 2](media/ahb02.png)
 
下面提供一個參考表，有助於您更詳細了解權益規則。 綠色欄顯示相同類型 VM 的數量，藍色列顯示每個 VM 的核心密度。 黃色儲存格顯示 2 處理器授權的數目 (或 16 核心組數)，其中一個必須部署特定核心密度的特定 VM 數目。 

Windows Server 搭配 SA 需求參考表：

![影像 3](media/ahb03.png)
 
適用於 Windows Server 的 Azure Hybrid Benefit 也提供根據您的需求來執行設定，以及結合不同類型 VM 的彈性。

幾個授權定位的設定範例：

![圖片 4](media/ahb04.png)
![圖片 5](media/ahb05.png)

 
如果您想深入了解適用於 Windows Server 的 Azure Hybrid Benefit，請移至 Azure Hybrid Benefit 網站。

## <a name="how-to-maintain-compliance"></a>如何維持合規性

想將 Azure Hybrid Benefit 應用至其 Windows Server VM 的客戶，必須先確認符合資格的授權數目，以及 SA/訂閱的涵蓋期間，然後才能啟用權益並套用上述指導方針以部署權益提供的正確 VM 數目。 如果您已經有搭配 Azure Hybrid Benefit 執行的 VM，您將需要清查正在執行多少單位，並根據您擁有的作用中 SA 授權進行檢查。  請連絡您的 Microsoft Enterprise Agreement 授權專家，來驗證您的 SA 授權定位。
若要查看並計算訂閱中搭配適用於 Windows Server 的 Azure Hybrid Benefit 部署的所有虛擬電腦，您可以執行下列其中一個方法：

1. 設定 Microsoft Azure 入口網站來顯示適用於 Windows Server 的 Azure Hybrid Benefit 使用量。在 Microsoft Azure 入口網站的虛擬電腦區段清單檢視中，新增「Azure Hybrid Benefit」欄。 

    ![影像 6](media/ahb06.png)

2.  使用 PowerShell 列出適用於 Windows Server 的 Azure Hybrid Benefit 使用量

    ```
    $vms = Get-AzureRMVM 
    foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
    ```

3.  查看您的 Microsoft Azure 帳單明細，以判斷您正在執行多少部搭配適用於 Windows Server 的 Azure Hybrid Benefit 的虛擬機器。 搭配權益的執行個體數目相關資訊會顯示在 [其他資訊] 下：

    ```
    "{"ImageType":"WindowsServerBYOL","ServiceType":"Standard_A1","VMName":"","UsageType":"ComputeHR"}" 
    ```

請注意，帳單不會即時套用，亦即在您啟動搭配 Hybrid Benefit 的 VM 到它顯示在帳單上，會有幾個小時的延遲時間。
接著您可以將結果填入下方的**適用於 Windows Server 的 Azure Hybrid Benefit SA 計數工具**，以取得所需的 SA 或訂閱涵蓋 WS 授權數。

請務必在您擁有的每個訂閱中進行清查，以產生您授權定位的完整檢視。

[Azure Hybrid Benefit WS SA 計數工具](http://download.microsoft.com/download/7/1/2/712FEFF0-155C-4ABF-96C0-CE4EC4DB0516/Azure_Hybrid_Benefit_Windows_Server_SA_Count_Tool.xlsx)

如果您執行上述步驟並確認您已獲得您正在執行的 Azure Hybrid Benefit 執行個體數目的完整授權，則不需要任何其他動作。 如果您發現您可使用權益涵蓋增量式 VM，您可能會想要切換到搭配權益的執行中執行個體而非全部成本，來進一步最佳化您的成本。

如果針對已部署的 VM 數，您尚未擁有足夠的符合資格 Windows Server 授權，您可能需要透過下列其中一個管道購買軟體保證涵蓋的其他 Windows Server 內部部署授權、以固定小時計費購買 Windows Server VM，或關閉某些 VM 的 Hybrid Benefit 功能。 請注意，您可以依 8 核心的增量來購買核心授權，以符合每個額外 Azure Hybrid Benefit VM 的資格。 

可透過下列其中一個 Microsoft 授權管道組合，購買 Windows Server 軟體保證和/或訂閱：

| 頻道                      | 開啟     | OVS      | Select/ Select Plus  | MPSA       | EA/EAS   |
|------------------------------|----------|----------|-----------------------|-----------|----------|
| 一般大小 (裝置數)  | 5-250    | 5-250    | >250                  | >250      | >500     |
| SA/訂閱            | 選擇性 | 內含 | 選擇性              | 選擇性  | 內含 |

Microsoft 保留隨時稽核最終客戶以驗證其是否符合 Azure Hybrid Benefit 資格的權利。 

## <a name="deployment-guidance"></a>部署指導 

我們已為所有擁有符合資格授權的客戶啟用預先建置庫映像可用性，無論其購買地點，此外也允許合作夥伴可以代表客戶執行部署。 

請[至此處](https://azure.microsoft.com/pricing/hybrid-use-benefit/)尋找所有可用部署選項的指示，包括： 
-   強調運用預先建置庫映像之新部署體驗的詳細影片
-   上傳自訂建置 VM 的詳細指示 
-   使用 PowerShell 來利用 Azure 站台復原移轉現有虛擬機器的詳細指示。 
