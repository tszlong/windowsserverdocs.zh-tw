---
title: 安裝 Nano 伺服器
description: Nano Server 的全新安裝、升級、移轉及評估
ms.prod: windows-server
manager: dougkim
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: get-started-article
ms.assetid: 2c2fa45b-6f3b-4663-b421-2da6ecc463bf
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 68de1697c8655075041cd9e598ccd2bbc2e6237b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826981"
---
# <a name="install-nano-server"></a>安裝 Nano 伺服器

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 1709 版開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 

Windows Server 2016 提供新的安裝選項︰Nano Server。 Nano Server 是一個遠端管理的伺服器作業系統，已針對私人雲端和資料中心最佳化。 它類似於 Server Core 模式的 Windows Server，但明顯較小、沒有本機登入功能，而且只支援 64 位元應用程式、工具和代理程式。 比起 Windows Server，它佔用的磁碟空間更少、設定的速度明顯地更快，而且所需的更新和重新啟動次數更少。 如果重新啟動，重新啟動的速度會更快。 Windows Server 2016 Standard 和 Datacenter Edition 提供 Nano Server 安裝選項。  

Nano Server 適用於一些案例：  
  
-   作為 Hyper-V 虛擬機器的計算主機 (可以在或不在叢集中)  
  
-   作為向外延展檔案伺服器的存放裝置主機。  
  
-   作為 DNS 伺服器  
  
-   作為執行 Internet Information Services (IIS) 的網頁伺服器  
  
-   作為使用雲端應用程式模式開發並在容器或虛擬機器客體作業系統中執行的應用程式主機  
  
## <a name="important-differences-in-nano-server"></a>Nano Server 中的重要差異

由於 Nano Server 經過最佳化，作為輕量型作業系統，用來執行以容器和微服務為基礎的雲端原生應用程式，或是作為靈活且符合成本效益的資料中心主機，並大幅減少使用量，因此 Nano Server 與 Server Core 或「使用桌面體驗的伺服器」有重要的差異︰

- Nano Server 為無周邊，不具本機登入功能或圖形化使用者介面。
- 僅支援 64 位元應用程式、工具及代理程式。
- Nano Server 無法作為 Active Directory 網域控制站。
- 不支援群組原則。 不過，您可以使用[預期狀態設定](https://msdn.microsoft.com/powershell/dsc/nanoDsc)大規模地套用設定。
- 無法將 Nano Server 設為使用 Proxy 伺服器存取網際網路。
- 不支援 NIC 小組 (具體而言，指負載平衡和容錯移轉，或稱 LBFO)。 改為支援切換內嵌小組 (SET)。
- 不支援 Microsoft Endpoint Configuration Manager 及 System Center Data Protection Manager。
- 不支援最佳做法分析程式 (BPA) Cmdlet 及 BPA 與伺服器管理員整合。
- Nano Server 不支援虛擬主機匯流排介面卡 (HBA)。
- Nano Server 不需要以產品金鑰啟用。 Nano Server 作為 Hyper-V 主機運作時，不支援[自動虛擬機器啟用](https://technet.microsoft.com/library/dn303421%28v=ws.11%29.aspx) \(AVMA\) (英文)。 在 Nano Server 主機上執行的虛擬機器可使用[金鑰管理服務](https://technet.microsoft.com/library/jj612867(v=ws.11).aspx) \(KMS\) (英文) 搭配一般的大量授權金鑰，或使用[Active Directory 型啟用](https://technet.microsoft.com/library/dn502534(v=ws.11).aspx) \(英文\) 來啟用。
- Nano Server 隨附的 Windows PowerShell 的版本具有重大差異。 如需詳細資料，請參閱 [Nano Server 上的 PowerShell](PowerShell-on-Nano-Server.md)。
- Nano Server 僅在最新商務分支 (CBB) 模型上受支援，Nano Server 目前沒有長期維護分支 (LTSB) 版本。 如需詳細資訊，請參閱下列各子章節。

### <a name="current-branch-for-business"></a>最新商務分支
Nano Server 由更為活躍的模型提供服務，稱作最新商務分支 (CBB)，以替使用快速開發週期移至雲端節奏的客戶提供支援。 在此模型中，Nano Server 預期每年會有兩到三次的功能更新版本。 此模型需要[軟體保證](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx)，才能在生產環境中部署及操作 Nano Server。 為了維持支援，系統管理員不得落後超過兩個 CBB 版本。 不過，這些版本不會自動更新現有的部署；系統管理員會依其便利性手動安裝新的 CBB 版本。 如需其他資訊，請參閱 [Windows Server 2016 新的最新商務分支服務選項](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/)。

Server Core 和「使用桌面體驗的伺服器」安裝選項仍由[長期維護分支 (LTSB) 模型](https://support.microsoft.com/lifecycle#gp%2Fgp_msl_policy)提供服務，由 5 年的主流支援及 5 年的延伸支援組成。

## <a name="installation-scenarios"></a>安裝案例

### <a name="evaluation"></a>評估
您可以從 [Windows Server 評估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)取得 Windows Server 的 180 天授權評估版。 若要試用 Nano Server，請選擇 [Nano Server] | [64 位元 EXE] 選項  ，然後回到 [Nano Server 快速入門](Nano-Server-Quick-Start.md)或[部署 Nano Server](Deploy-Nano-Server.md) 以開始使用。

### <a name="clean-installation"></a>全新安裝
因為您藉由設定 VHD 來安裝 Nano Server，所以全新安裝會是最快速且最簡單的部署方法。

- 若要使用 DHCP 取得 IP 位址，以快速開始進行 Nano Server 的基本部署，請參閱 [Nano Server 快速入門](Nano-Server-Quick-Start.md) 
- 如果您已經熟悉 Nano Server 的基本概念，從[部署 Nano Server](Deploy-Nano-Server.md) 開始的更詳細主題提供一組完整的指示，來自訂映像、使用網域、線上或離線安裝伺服器角色和其他功能的套件，以及執行其他更多工作。

> [!IMPORTANT]  
> 一旦安裝程式完成，而且您已安裝所有必要的伺服器角色和功能之後，請立即檢查並安裝 Windows Server 2016 的可用更新。 針對 Nano Server，請參閱[管理 Nano Server](Manage-Nano-Server.md) 的＜在 Nano Server 中管理更新＞一節。

### <a name="upgrade"></a>升級
因為 Nano Server 是 Windows Server 2016 的新功能，所以從舊版作業系統升級到 Nano Server 之間沒有升級路徑。

### <a name="migration"></a>移轉
因為 Nano Server 是 Windows Server 2016 的新功能，所以從舊版作業系統升級到 Nano Server 之間沒有移轉路徑。
  
-------------------------------------
如果您需要不同的安裝選項，您可以[回到 Windows Server 2016 主頁面](windows-server-2016.md) 

  


 
