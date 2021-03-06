---
title: 儲存體遷移服務總覽
description: 儲存體遷移服務可讓您更輕鬆地將存放裝置遷移至 Windows Server 或 Azure。 它提供了一種圖形化工具，可清查 Windows 和 Linux 伺服器上的資料，然後將資料傳輸到較新的伺服器或 Azure 虛擬機器。 儲存體遷移服務也提供將伺服器身分識別傳送至目的地伺服器的選項，讓應用程式和使用者可以存取其資料，而不需要變更連結或路徑。
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 03/26/2020
ms.topic: article
ms.openlocfilehash: 18c9bb2bf22f107b988942706850907c67b5ec9a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939326"
---
# <a name="storage-migration-service-overview"></a>儲存體遷移服務總覽

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server (半年通道) 

儲存體遷移服務可讓您更輕鬆地將存放裝置遷移至 Windows Server 或 Azure。 它提供了一種圖形化工具，可清查 Windows 和 Linux 伺服器上的資料，然後將資料傳輸到較新的伺服器或 Azure 虛擬機器。 儲存體遷移服務也提供將伺服器身分識別傳送至目的地伺服器的選項，讓應用程式和使用者可以存取其資料，而不需要變更連結或路徑。

本主題討論為何您會想要使用儲存體遷移服務、遷移程式的運作方式、來源與目的地伺服器的需求，以及[儲存體遷移服務的新功能](#whats-new-in-storage-migration-service)。

## <a name="why-use-storage-migration-service"></a>為何要使用儲存體遷移服務

使用儲存體遷移服務，因為您的伺服器 (或許多伺服器) 您想要遷移至較新的硬體或虛擬機器。 儲存體遷移服務的設計目的是要協助執行下列作業：

- 清查多部伺服器及其資料
- 從來源伺服器快速傳輸檔案、檔案共用和安全性設定
- 選擇性接管來源伺服器的身分識別 (也稱為「剪下」) ，讓使用者和應用程式不必變更任何專案即可存取現有的資料
- 從 Windows 系統管理中心使用者介面管理一或多個遷移

![此圖顯示儲存體遷移服務將檔案從來源伺服器遷移至目的地伺服器、Azure Vm 或 Azure 檔案同步的 & 設定。](media/overview/storage-migration-service-diagram.png)

**圖1：儲存體遷移服務的來源和目的地**

## <a name="how-the-migration-process-works"></a>遷移程式的運作方式

遷移是三個步驟的程式：

1. **清查伺服器**以收集其檔案和設定的相關資訊 (如 [圖 2]) 所示。
2. **傳輸 (將) 資料**從來源伺服器複製到目的地伺服器。
3.  (選擇性) **切換到新的伺服器**。<br>目的地伺服器會假設來源伺服器先前的身分識別，讓應用程式和使用者不必變更任何專案。 <br>來源伺服器會進入維護狀態，其中仍會包含一律具有的相同檔案 (我們絕對不會從來源伺服器移除檔案) 但無法供使用者和應用程式使用。 然後您就可以方便地解除委任伺服器。

![螢幕擷取畫面，顯示已準備好要掃描的伺服器 ](media/migrate/inventory.png)
 **圖2：儲存體遷移服務清查伺服器**

以下影片示範如何使用儲存體遷移服務來採用伺服器，例如現在不支援的 Windows Server 2008 R2 伺服器，以及將存放裝置移至較新的伺服器。

> [!VIDEO https://www.youtube.com/embed/h-Xc9j1w144]

## <a name="requirements"></a>需求

若要使用儲存體遷移服務，您需要下列各項：

- 用來遷移檔案和資料的**來源伺服器**或**容錯移轉**叢集
- 執行 Windows Server 2019 (叢集或獨立) 以遷移至的**目的地伺服器**。 Windows Server 2016 和 Windows Server 2012 R2 也可以運作，但速度大約是50%
- 執行 Windows Server 2019 以管理遷移的**協調器伺服器**  <br>如果您只是要遷移少數伺服器，而且其中一部伺服器正在執行 Windows Server 2019，您可以使用它作為協調器。 如果您要遷移更多伺服器，建議使用個別的 orchestrator 伺服器。
- 除非您偏好使用 PowerShell 來管理遷移，否則執行** [Windows 系統管理中心](../../manage/windows-admin-center/overview.md)的電腦或伺服器**會執行儲存體遷移服務使用者介面。 Windows 管理中心和 Windows Server 2019 版本必須至少為1809版。

強烈建議協調器和目的地電腦至少要有兩個核心或兩個個 vcpu，以及至少 2 GB 的記憶體。 使用更多的處理器和記憶體，清查和傳輸作業的速度大幅提升。

### <a name="security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports"></a>安全性需求、儲存體遷移服務 proxy 服務和防火牆埠

- 一種遷移帳戶，這是來源電腦和 orchestrator 電腦上的系統管理員。
- 一種遷移帳戶，其為目的地電腦和 orchestrator 電腦上的系統管理員。
- Orchestrator 電腦必須*已啟用 [* 檔案及印表機共用] (SMB) 防火牆規則。
- 來源和目的地電腦必須 (啟用下列防火牆規則，但您可能已經*將它們啟用*) ：
  - 檔案及印表機共用 (SMB-In)
  - Netlogon 服務 (NP) 
  - Windows Management Instrumentation (DCOM-In)
  - Windows Management Instrumentation (WMI-In)

  > [!TIP]
  > 在 Windows Server 2019 電腦上安裝儲存體遷移服務 Proxy 服務時，會自動在該電腦上開啟必要的防火牆埠。 若要這麼做，請連線到 Windows 管理中心的目的地伺服器，然後移至 Windows 管理中心的**伺服器管理員** () >**角色和功能**]，選取 [儲存體] [**遷移服務 Proxy**]，然後選取 [**安裝**]。


- 如果電腦屬於 Active Directory Domain Services 網域，它們應該全都屬於相同的樹系。 如果您想要在進行剪下時將來源的功能變數名稱傳輸到目的地，目的地伺服器也必須位於與來源伺服器相同的網域中。 技術上的轉換會跨網域運作，但是目的地的完整功能變數名稱會與來源不同 .。。

### <a name="requirements-for-source-servers"></a>來源伺服器的需求

來源伺服器必須執行下列其中一個作業系統：

- Windows Server 半年通道
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003
- Windows Small Business Server 2003 R2
- Windows Small Business Server 2008
- Windows Small Business Server 2011
- Windows Server 2012 Essentials
- Windows Server 2012 R2 Essentials
- Windows Server 2016 Essentials
- Windows Server 2019 Essentials
- Windows Storage Server 2008
- Windows Storage Server 2008 R2
- Windows Storage Server 2012
- Windows Storage Server 2012 R2
- Windows Storage Server 2016

注意： Windows Small Business Server 和 Windows Server Essentials 是網域控制站。 儲存體遷移服務無法從網域控制站進行切換，但可以清查和傳輸檔案。

如果協調器執行的是 Windows Server、1903版或更新版本，或如果協調器執行舊版的 Windows Server 並安裝[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ，您可以遷移下列其他來源類型：

- 執行 Windows Server 2012、Windows Server 2012 R2、Windows Server 2016、Windows Server 2019 的容錯移轉叢集
- 使用 Samba 的 Linux 伺服器。 我們已測試過下列各項：
    - CentOS 7
    - Debian GNU/Linux 8
    - RedHat Enterprise Linux 7。6
    - SUSE Linux Enterprise Server (SLES) 11 SP4
    - Ubuntu 16.04 LTS 和 12.04.5 l t LTS
    - Samba 4.8、4.7、4.3、4.2 和3。6

### <a name="requirements-for-destination-servers"></a>目的地伺服器的需求

目的地伺服器必須執行下列其中一個作業系統：

- Windows Server 半年通道
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> 執行 Windows Server 2019 或 Windows Server、半年通道或更新版本的目的地伺服器，具有舊版 Windows Server 的兩倍傳輸效能。 這種效能提升的原因是包含內建的儲存體遷移服務 proxy 服務，這也會開啟必要的防火牆埠（如果尚未開啟）。

## <a name="azure-vm-migration"></a>Azure VM 遷移

Windows Admin Center 1910 版可讓您部署 Azure 虛擬機器。 這會將 VM 部署整合到儲存體遷移服務。 不是在部署工作負載之前手動在 Azure 入口網站中建立新的伺服器和 Vm，而是可能遺失必要的步驟和設定-Windows 管理中心可以部署 Azure VM、設定其儲存體、將其加入您的網域、安裝角色，然後設定您的分散式系統。

   以下影片顯示如何使用儲存體遷移服務來遷移至 Azure Vm。
   > [!VIDEO https://www.youtube-nocookie.com/embed/k8Z9LuVL0xQ]

如果您想要將虛擬機器隨即轉移至 Azure，而不需要遷移至較新的作業系統，請考慮使用 Azure Migrate。 如需詳細資訊，請參閱[Azure Migrate 總覽](https://go.microsoft.com/fwlink/?linkid=2056064)。

## <a name="whats-new-in-storage-migration-service"></a>儲存體遷移服務的新功能

Windows 管理中心1910版新增了部署 Azure 虛擬機器的功能。 這會將 Azure VM 部署整合到儲存體遷移服務。 如需詳細資訊，請參閱[AZURE VM 遷移](#azure-vm-migration)。

在 Windows Server、1903版或更新版本上執行儲存體遷移伺服器協調器，或安裝了[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)的舊版 windows server 時，可以使用下列新功能：

- 將本機使用者和群組遷移到新伺服器
- 從容錯移轉叢集遷移存放裝置、遷移至容錯移轉叢集，以及在獨立伺服器與容錯移轉叢集之間進行遷移
- 從使用 Samba 的 Linux 伺服器遷移儲存空間
- 使用 Azure 檔案同步，更輕鬆地同步已遷移至 Azure 的共用
- 遷移新的網路，例如 Azure

## <a name="additional-references"></a>其他參考資料

- [使用儲存體遷移服務遷移檔案伺服器](migrate-data.md)
- [儲存體遷移服務的常見問題 (常見問題) ](faq.md)
- [儲存體遷移服務的已知問題](known-issues.md)
