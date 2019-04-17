---
title: Azure 堆疊 HCI 概觀
description: Azure 堆疊 HCI 是使用超融合式 Windows Server 2019 叢集驗證硬體以執行虛擬化工作負載在內部，選擇性地連線到雲端為基礎的備份、 站台復原及更多的 Azure 服務。 使用 Microsoft 驗證的硬體，以確保最佳效能和可靠性、 azure 堆疊 HCI 解決方案，並包含例如 NVMe 磁碟機、 持續性記憶體，以及遠端直接記憶體存取 (RDMA) 網路功能技術支援。
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/26/2019
---

# Azure 堆疊 HCI 概觀

>適用於： Windows Server 2019

Azure 堆疊 HCI 是使用超融合式 Windows Server 2019 叢集驗證硬體以執行虛擬化工作負載在內部，選擇性地連線到雲端為基礎的備份、 站台復原及更多的 Azure 服務。 使用 Microsoft 驗證的硬體，以確保最佳效能和可靠性、 azure 堆疊 HCI 解決方案，並包含例如 NVMe 磁碟機、 持續性記憶體，以及遠端直接記憶體存取 (RDMA) 網路功能技術支援。

## Azure Stack 系列

Azure 堆疊 HCI 是一部分 Azure，Azure Stack 系列中，使用相同的軟體定義計算、 儲存和網路功能的軟體即 Azure Stack。 以下是不同的解決方案的快速摘要：

- [Azure](https://azure.microsoft.com)使用公用雲端服務
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) -運作雲端服務的內部部署
- [Azure 堆疊 HCI](https://azure.microsoft.com/overview/azure-stack/hci) -執行虛擬化應用程式在內部，與選用連線至 Azure

![Azure 和 Azure 堆疊 HCI 執行時，執行雲端服務的 Azure Stack 虛擬化應用程式在內部部署](media/azure-and-azure-stack-family.png)

若要深入了解：

- 我們的[混合式雲端虛擬事件](https://info.microsoft.com/ww-landing-building-a-successful-hybrid-cloud-strategy.html)註冊在 2019 年 3 月 28 日。
- 深入了我們[Azure 堆疊 HCI](https://azure.microsoft.com/overview/azure-stack/hci)解決方案網站。
- 請觀看 Microsoft 專家 Jeff Woolsey 和 Vijay Tewari[討論新的 Azure 堆疊 HCI 解決方案](https://aka.ms/AzureStackOverviewVideo)。

## 硬體合作夥伴

您可以購買 15 合作夥伴從執行 Windows Server 2019 的已驗證的 Azure 堆疊 HCI 解決方案。 您慣用的 Microsoft 合作夥伴取得您設定並執行而不冗長的設計和建置階段，並提供單一存取點的實作和支援服務連絡人。

請造訪[Azure 堆疊 HCI 網站](https://azure.microsoft.com/overview/azure-stack/hci)，來檢視我們 70 + Azure 堆疊 HCI 解決方案目前可從這些 Microsoft 合作夥伴： ASUS，Axellio，bluechip DataON、 Dell EMC、 Fujitsu、 HPE、 Hitachi、 Huawei、 Lenovo、 NEC、 primeLine 解決方案、 QCT、 SecureGUARD和 Supermicro。

## 常見問題

### 什麼 Azure Stack 及 Azure 堆疊 HCI 解決方案共通？ 
Azure 堆疊 HCI 解決方案功能相同的 HYPER-V 基礎軟體定義運算、 儲存和網路功能技術，做為 Azure Stack。 這兩個供應項目符合嚴格的測試及驗證條件以確保可靠性和與基礎硬體平台的相容性。

### 它們如何是不同？
使用 Azure Stack 您執行雲端服務在內部。 您可以執行 Azure IaaS 和 PaaS 服務在內部以一致的方式建置並執行雲端應用程式在任何地方，使用 Azure 入口網站內部部署管理。

使用您執行虛擬化工作負載在內部，Azure 堆疊 HCI 熟悉的 Windows Server 與 Windows Admin Center 管理工具。 您可以選擇性地連線到 Azure 進行混合式案例，例如雲端為基礎的站台復原、 監視和其他項目。

### Microsoft 將提供給 Azure Stack 系列其 HCI 的為何？ 
Microsoft 的超融合技術已經 Azure Stack 的基礎。 

許多 Microsoft 客戶有複雜 IT 環境，而且我們的目標是要提供符合它們都具有適當的技術，正確的企業版方案必須。 Azure 堆疊 HCI 是由我們的硬體合作夥伴先前可用的 Windows Server 2016 為基礎的 windows server 軟體定義 (WSSD) 解決方案的進化版。 我們帶入它 Azure Stack 系列因為我們已開始為提供順暢地與 Azure 連線基礎結構管理服務的新選項。 

### 我能從 Azure 堆疊 HCI 升級至 Azure Stack？ 
否，但是客戶可以移轉 Azure 堆疊 HCI 從其工作負載到 Azure Stack 或 Azure。

### 有哪些 Azure 服務我可以連線到 Azure 堆疊 HCI？

您可以連線到 Azure 堆疊 HCI 的 Azure 服務的更新的清單，請參閱[連接到混合式 Azure 服務的 Windows Server](../azure-hybrid-services/index.md)。

### 我該如何購買 Azure 堆疊 HCI 解決方案？
請依照下列步驟執行：

1. 從您的慣用的硬體合作夥伴購買 Microsoft 驗證硬體系統。
1. 安裝 Windows Server 2019 Datacenter edition 和 Windows Admin Center 管理以及連線至 Azure 雲端服務的能力
1. 要附加到您的工作負載的雲端式管理和安全性服務，選擇使用 Azure 帳戶。