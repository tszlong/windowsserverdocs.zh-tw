---
title: 儲存體遷移服務總覽
description: 儲存空間移轉服務可讓您更輕鬆地將伺服器遷移至較新版本的 Windows Server。 其提供的圖形工具可清查伺服器上的資料，然後將資料和設定轉送至較新的伺服器 — 完全不需要應用程式或使用者變更任何項目。
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 08/16/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: dae64b81c48b9ae6bf84c3558066ebbdf9c06ace
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69584832"
---
# <a name="storage-migration-service-overview"></a>儲存體遷移服務總覽

>適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server (半年通道)

儲存空間移轉服務可讓您更輕鬆地將伺服器遷移至較新版本的 Windows Server。 其提供的圖形工具可清查伺服器上的資料，然後將資料和設定轉送至較新的伺服器 — 完全不需要應用程式或使用者變更任何項目。

本主題討論您想要使用儲存體遷移服務的原因、遷移程式的運作方式, 以及來源和目的地伺服器的需求。

## <a name="why-use-storage-migration-service"></a>為何要使用儲存體遷移服務

使用儲存體遷移服務, 因為您有一部伺服器 (或許多伺服器) 想要遷移至較新的硬體或虛擬機器。 儲存體遷移服務的設計目的是要協助執行下列作業:

- 清查多部伺服器及其資料
- 從來源伺服器快速傳輸檔案、檔案共用和安全性設定
- 選擇性接管來源伺服器的身分識別 (也稱為「剪下」), 讓使用者和應用程式不必變更任何專案即可存取現有的資料
- 從 Windows 系統管理中心使用者介面管理一或多個遷移

![此圖顯示儲存體遷移服務將檔案從來源伺服器遷移至目的地伺服器、Azure Vm 或 Azure 檔案同步的 & 設定。](media/overview/storage-migration-service-diagram.png)

**圖 1:儲存體遷移服務的來源和目的地**

## <a name="how-the-migration-process-works"></a>遷移程式的運作方式

遷移是三個步驟的程式:

1. **清查伺服器**以收集其檔案和設定的相關資訊 (如 [圖 2] 所示)。
2. 將資料從來源伺服器**傳輸 (複製)** 到目的地伺服器。
3. **切換到新的伺服器**(選擇性)。<br>目的地伺服器會假設來源伺服器先前的身分識別, 讓應用程式和使用者不必變更任何專案。 <br>來源伺服器會進入維護狀態, 其中仍包含它們一律具有的相同檔案 (我們絕對不會移除來源伺服器中的檔案), 但無法供使用者和應用程式使用。 然後您就可以方便地解除委任伺服器。

![螢幕擷取畫面: 顯示已準備好要](media/migrate/inventory.png)
掃描**的伺服器 [圖 2]儲存體遷移服務清查伺服器**

## <a name="requirements"></a>需求

若要使用儲存體遷移服務, 您需要下列各項:

- 要從中遷移檔案和資料的**來源伺服器**
- 執行 Windows Server 2019 以遷移至的**目的地伺服器**(windows server 2016 和 Windows Server 2012 R2) 也可以運作, 但速度大約是 50%
- 執行 Windows Server 2019 以管理遷移的**協調器伺服器**  <br>如果您只是要遷移少數伺服器, 而且其中一部伺服器正在執行 Windows Server 2019, 您可以使用它作為協調器。 如果您要遷移更多伺服器, 建議使用個別的 orchestrator 伺服器。
- 除非您偏好使用 PowerShell 來管理遷移, 否則執行 **[Windows 系統管理中心](../../manage/windows-admin-center/understand/windows-admin-center.md)的電腦或伺服器**會執行儲存體遷移服務使用者介面。 Windows 管理中心和 Windows Server 2019 版本必須至少為1809版。

強烈建議協調器和目的地電腦至少要有兩個核心或兩個個 vcpu, 以及至少 2 GB 的記憶體。 使用更多的處理器和記憶體, 清查和傳輸作業的速度大幅提升。

### <a name="security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports"></a>安全性需求、儲存體遷移服務 proxy 服務和防火牆埠

- 一種遷移帳戶, 這是來源電腦和 orchestrator 電腦上的系統管理員。
- 一種遷移帳戶, 其為目的地電腦和 orchestrator 電腦上的系統管理員。
- Orchestrator 電腦必須已啟用「檔案及印表機共用」 (SMB*輸入*) 防火牆規則。
- 來源和目的地電腦必須啟用下列防火牆規則 (不過您可能已經啟用):
  - 檔案及印表機共用 (SMB-In)
  - Netlogon 服務 (NP-IN)
  - Windows Management Instrumentation (DCOM)
  - Windows Management Instrumentation (WMI-In)
  
  > [!TIP]
  > 在 Windows Server 2019 電腦上安裝儲存體遷移服務 Proxy 服務時, 會自動在該電腦上開啟必要的防火牆埠。 若要這麼做, 請連線到 Windows 管理中心的目的地伺服器, 然後移至**伺服器管理員**(在 Windows 系統管理中心) >**角色和功能**, 選取 儲存體 **遷移服務 Proxy**, 然後選取 **安裝**。


- 如果電腦屬於 Active Directory Domain Services 網域, 它們應該全都屬於相同的樹系。 如果您想要在進行剪下時將來源的功能變數名稱傳輸到目的地, 目的地伺服器也必須位於與來源伺服器相同的網域中。 技術上的轉換會跨網域運作, 但是目的地的完整功能變數名稱會與來源不同 。

### <a name="requirements-for-source-servers"></a>來源伺服器的需求

來源伺服器必須執行下列其中一個作業系統:

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

注意:Windows Small Business Server 和 Windows Server Essentials 是網域控制站。 儲存體遷移服務無法從網域控制站進行切換, 但可以清查和傳輸檔案。   

如果協調器正在執行 Windows Server 1903 版或更新版本, 您可以遷移下列其他來源類型:

- 容錯移轉叢集
- 使用 Samba 的 Linux 伺服器。 我們已測試過下列各項:
    - RedHat Enterprise Linux 7.6、CentOS 7、Debian 8、Ubuntu 16.04 和 12.04.5 l t、SUSE Linux Enterprise Server (SLES) 11 SP4
    - Samba 4.x、3.6. x

### <a name="requirements-for-destination-servers"></a>目的地伺服器的需求

目的地伺服器必須執行下列其中一個作業系統:

- Windows Server 半年通道
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> 執行 Windows Server 2019 或 Windows Server 的目的地伺服器、半年通道1809版或更新版本具有舊版 Windows Server 的傳輸效能兩倍。 這種效能提升的原因是包含內建的儲存體遷移服務 proxy 服務, 這也會開啟必要的防火牆埠 (如果尚未開啟)。

## <a name="whats-new-in-storage-migration-service"></a>儲存體遷移服務的新功能

Windows Server (版本 1903) 在 orchestrator 伺服器上執行時, 會新增下列新功能:

- 將本機使用者和群組遷移到新伺服器
- 從容錯移轉叢集遷移儲存空間
- 從使用 Samba 的 Linux 伺服器遷移儲存空間
- 使用 Azure 檔案同步，更輕鬆地同步已遷移至 Azure 的共用
- 遷移新的網路，例如 Azure

## <a name="see-also"></a>另請參閱

- [使用儲存體遷移服務遷移檔案伺服器](migrate-data.md)
- [儲存體遷移服務常見問題 (FAQ)](faq.md)
- [儲存體遷移服務的已知問題](known-issues.md)
