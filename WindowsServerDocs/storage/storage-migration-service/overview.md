---
Title: 儲存體移轉服務概觀
description: 儲存體移轉服務可讓您更輕鬆地移轉到新版的 Windows Server 的伺服器。 它提供圖形化工具清查伺服器上的資料，然後將資料和設定傳送至較新的伺服器 — 完全不需要應用程式或使用者不必變更任何項目。
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 05/21/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: fd99058036a5b8041e4c65ca120c6a7e68b2df8d
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976651"
---
# <a name="storage-migration-service-overview"></a>儲存體移轉服務概觀

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server （半年通道）

儲存體移轉服務可讓您更輕鬆地移轉到新版的 Windows Server 的伺服器。 它提供圖形化工具清查伺服器上的資料，然後將資料和設定傳送至較新的伺服器 — 完全不需要應用程式或使用者不必變更任何項目。

本主題討論為什麼您會想要使用儲存體移轉服務，移轉程序運作的方式，以及需求會針對來源和目的地伺服器。

## <a name="why-use-storage-migration-service"></a>為何要使用儲存體移轉服務

使用儲存體移轉服務，因為您有伺服器 （或大量伺服器），您想要移轉至較新的硬體或虛擬機器。 儲存體移轉服務被設計來協助執行下列動作：

- 清查多部伺服器和其資料
- 快速傳輸檔案、 檔案共用和安全性設定從來源伺服器
- 選擇性地接管 （也就透過縮減） 的來源伺服器的身分識別，讓使用者和應用程式不需要變更任何項目來存取現有的資料
- 從 Windows Admin Center 使用者介面管理一或多個移轉

![此圖表顯示儲存體移轉服務移轉檔案和設定從來源伺服器到目的地伺服器 」、 「 Azure Vm 或 「 Azure 檔案同步。](media\overview\storage-migration-service-diagram.png)

**圖 1:儲存體移轉服務的來源和目的地**

## <a name="how-the-migration-process-works"></a>移轉程序的運作方式

移轉是三個步驟的程序：

1. **清查伺服器**收集其檔案和設定 （如 圖 2 所示） 的相關資訊。
2. **（複製） 資料傳輸**從來源伺服器到目的地伺服器。
3. **移交到新伺服器**（選擇性）。<br>目的地伺服器，讓應用程式和使用者不需要變更任何項目，假設來源伺服器的舊身分識別。 <br>來源伺服器進入維護狀態，它們仍然包含相同檔案，其一律具有 （我們永遠不會移除檔案從來源伺服器），但無法使用使用者和應用程式。 您接著可以隨時解除委任伺服器。

![顯示伺服器準備好要掃描的螢幕擷取畫面](media/migrate/inventory.png)
**圖 2:清查伺服器的儲存體移轉服務**

## <a name="requirements"></a>需求

若要使用儲存體移轉服務，您需要下列項目：

- A**來源伺服器**移轉檔案和資料
- A**目的地伺服器**執行移轉到 Windows Server 2019 — Windows Server 2016 和 Windows Server 2012 R2 正常運作，但約為速度較慢的 50%
- **Orchestrator 伺服器**執行 Windows Server 2019 來管理移轉  <br>如果您要移轉只有少數的伺服器，而且其中一個伺服器執行 Windows Server 2019，因此您可以使用可為協調器。 如果您要移轉更多的伺服器，我們建議使用不同的 orchestrator 伺服器。
- A**電腦或執行伺服器[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)** 執行儲存體移轉服務使用者介面中，除非您偏好使用 PowerShell 來管理移轉。 Windows Admin Center 和 Windows Server 2019 版本都必須至少版本 1809年。

我們強烈建議在 orchestrator 和目的地電腦有至少兩個核心或兩個 Vcpu 及至少 2 GB 的記憶體。 清查和傳輸的作業是使用更多的處理器和記憶體的速度明顯加快。

### <a name="security-requirements"></a>安全性需求

- 是來源電腦和協調器電腦上的系統管理員移轉帳戶。
- 移轉帳戶，在目的地電腦和協調器電腦上的系統管理員。
- Orchestrator 電腦必須已啟用檔案及印表機共用 (Smb-in) 防火牆規則*輸入*。
- 來源和目的地電腦必須啟用下列防火牆規則*輸入*（雖然您可能已經有啟用）：
  - 檔案及印表機共用 (SMB-In)
  - Netlogon 服務 (Np-in)
  - Windows Management Instrumentation (Dcom-in)
  - Windows Management Instrumentation (WMI-In)
  
  > [!TIP]
  > Windows Server 2019 電腦上安裝的儲存體移轉服務 Proxy 服務時，也將會自動開啟必要的防火牆連接埠，在該電腦上。
- 如果電腦所屬 Active Directory 網域服務網域，它們應該全部屬於相同的樹系。 目的地伺服器必須與來源伺服器相同網域中，如果您想要透過剪下時，傳送到目的地的來源網域名稱。 轉換技術上的運作方式跨網域，但目的地的完整網域名稱將會不同於來源...

### <a name="requirements-for-source-servers"></a>來源伺服器的需求

來源伺服器必須執行下列作業系統的其中一個：

- Windows Server 半年通道
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003

如果協調器正在執行 Windows Server 版 1903年或更新版本，您可以移轉下列額外的來源類型：

- 容錯移轉叢集
- 使用 Samba 的 Linux 伺服器。 我們已經測試下列項目：
    - 7.6 的 RedHat Enterprise Linux、 CentOS 7、 Debian 8，Ubuntu 16.04 和 12.04.5，SUSE Linux Enterprise Server (SLES) 11 SP4
    - Samba 4.x 3.6.x

### <a name="requirements-for-destination-servers"></a>目的地伺服器的需求

目的地伺服器必須執行下列作業系統的其中一個：

- Windows Server 半年通道
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> 目的地伺服器執行 Windows Server 2019 或 Windows Server 半年通道 1809年或更新版本都具有雙重傳輸的效能較早版本的 Windows Server。 此效能提升是因為包含的內建儲存體移轉服務 proxy 服務，也會開啟必要的防火牆連接埠，如果它們尚未開啟。

## <a name="whats-new-in-storage-migration-service"></a>何謂儲存體移轉服務的新功能

Windows Server 版本 1903，加入下列新功能，在 orchestrator 伺服器上執行時：

- 將本機使用者和群組移轉至新的伺服器
- 從容錯移轉叢集移轉儲存體
- 使用 Samba 的 Linux 伺服器從移轉存放裝置
- 使用 Azure 檔案同步來更輕鬆地同步至 Azure 的已移轉的共用
- 移轉到新的網路，例如 Azure

## <a name="see-also"></a>另請參閱

- [使用儲存體移轉服務來移轉檔案伺服器](migrate-data.md)
- [儲存體移轉服務常見問題集 (faq)](faq.md)
- [儲存體移轉服務的已知問題](known-issues.md)
