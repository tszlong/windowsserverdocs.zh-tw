---
title: What's New in Windows Server 版 1903年時
description: 本主題描述一些 Windows Server 版 1903，也就是半年通道發行的新功能。
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 2aa84df1ca162cb6e2dfa580b4b0744ff08cc95a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983438"
---
# <a name="whats-new-in-windows-server-version-1903"></a>在 Windows Server，版本 1903，最新消息

>適用於：Windows Server (半年度管道)

本主題描述一些 Windows Server 版 1903，也就是半年通道發行的新功能。 這些功能包括執行及管理容器、 Server Core 安裝中處理的工具和 Linux 裝置從移轉存放裝置的能力的增強功能。

若要改為了解最新的長期維護通道 (LTSC) 版本的 Windows Server 的新功能，請參閱請參閱[What's New in Windows Server 2019](../get-started-19/whats-new-19.md)。 另請參閱[於 Windows 10 版本 1903，IT 專業人員內容最新消息](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903)。

此版本的系統需求會與 Windows Server 2019 相同，請參閱[系統需求](../get-started-19/sys-reqs-19.md)如需詳細資訊。 若要查看 最近項目已移除，請參閱[移除功能或已計劃，取代開頭為 Windows Server 版 1903，](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Windows 容器必須使用相同版本的 Windows 主機伺服器或*早*版本。 比方說，主機伺服器，執行 Windows Server 的發行的版本，版本 1903年 （組建 18342） 可以執行 Windows Server 容器與相同或更早版本的版本和組建編號 （即使容器會使用 Windows Insider Preview 版）。 如需詳細資訊，請參閱 < [Windows 容器版本相容性](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility)。

## <a name="enhanced-support-for-non-microsoft-container-services"></a>非 Microsoft 的容器服務的增強的支援

我們已增強為支援 Azure container service 和非 Microsoft 的容器服務的平台功能。

- 我們整合 CRI containerd 使用主機計算服務 (HCS) 上 Windows (LCOW) 在 Azure 上支援的 Windows 容器和 Linux 容器的 pod。
- 我們與 Kubernetes 社群合作以啟用 Windows 容器支援。 Kubernetes v1.14 版本中，Windows Server 節點支援正式畢業於穩定的 beta 版。 如需詳細資訊，請參閱 <<c0> [ 現在支援在 Kubernetes 中的 Windows 容器](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/)。
- Windows 的 Tigera Calico 現已全面上市屬於 Tigera Essentials 訂用帳戶和供應項目都為非重疊網路和網路原則互通跨混合式 Linux/Windows 環境。
- 我們傳遞增強網路支援 Windows 容器，包括使用 Kubernetes 透過最新版的整合 Flannel 和 Kubernetes v1.14 重疊的延展性改進功能。 如需詳細資訊，請參閱 <<c0> [ 在 Kubernetes 中的 Windows 支援簡介](https://kubernetes.io/docs/setup/windows/)。

## <a name="directx-hardware-acceleration-in-containers"></a>在容器中的 DirectX 硬體加速

我們想要啟用 Windows 容器 tp 支援案例，例如使用本機的圖形處理單元 (GPU) 硬體的 Machine Learning (ML) 推斷中的硬體加速的 DirectX Api 的支援。 如需詳細資訊，請參閱 <<c0> [ 至 Windows 容器將 GPU 加速](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939)。

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>已更新的容器身分識別 」 和 「 群組受控服務帳戶的文件

我們新增更多範例和相容性資訊才能[群組受控服務帳戶](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)文件，進行[認證規格 PowerShell 模組](https://www.powershellgallery.com/packages/CredentialSpec)「 PowerShell 資源庫中可用。 如需詳細資訊，請參閱 <<c0> [ 什麼是新的容器識別](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151)部落格文章。

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>將工作排程器和 HYPER-V 管理員新增至 Server Core 安裝

您可能已經知道，我們建議使用 Windows Server，在生產環境中的半年通道時，使用 Server Core 安裝選項。 不過，預設的 Server Core 省略了一些有用的管理工具。 您可以新增許多的最常用的工具來安裝應用程式相容性功能，但仍有某些遺漏的工具。

因此，根據客戶意見反應，我們加入兩個的更多工具在此版本的應用程式相容性功能：工作排程器 (taskschd.msc) 和 HYPER-V 管理員 (virtmgmt.msc)。

如需詳細資訊，請參閱 < [Server Core 應用程式相容性功能](../get-started-19/install-fod-19.md)。

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>本機帳戶、 叢集和 Linux 伺服器，現在會將移轉存放裝置移轉服務

儲存體移轉服務可讓您更輕鬆地移轉到新版的 Windows Server 的伺服器。 它提供圖形化工具清查伺服器上的資料，然後將資料和設定傳送至較新的伺服器 — 完全不需要應用程式或使用者不必變更任何項目。

當使用這個版本的 Windows Server 來協調移轉時，我們已新增下列功能：

- 將本機使用者和群組移轉至新的伺服器
- 從容錯移轉叢集移轉儲存體
- 使用 Samba 的 Linux 伺服器從移轉存放裝置
- 使用 Azure 檔案同步來更輕鬆地同步至 Azure 的已移轉的共用
- 移轉到新的網路，例如 Azure

如需儲存體移轉服務的詳細資訊，請參閱[儲存體移轉服務概觀](../storage/storage-migration-service/overview.md)。

## <a name="system-insights-disk-anomaly-detection"></a>系統 Insights 磁碟異常偵測

[系統 Insights](../manage/system-insights/overview.md)是預測性分析功能，在本機上分析 Windows Server 系統資料，並提供深入了解伺服器的正常運作。 它隨附許多內建功能，但我們已新增能夠在安裝額外的功能，透過 Windows Admin Center，從磁碟異常偵測。

磁碟異常偵測是一項新功能會反白顯示時磁碟運作*不同*比平常。 雖然不同不一定是一件壞事，看到這些異常時間可以是在系統上的問題進行疑難排解時很有幫助。

這項功能也適用於執行 Windows Server 2019 的伺服器。

## <a name="windows-admin-center-enhancements"></a>Windows Admin Center 增強功能

新版的 Windows Admin Center 已推出，Windows Server 中加入新的功能。 如需最新的功能資訊，請參閱[Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

## <a name="security-baseline-for-windows-10-and-windows-server"></a>適用於 Windows 10 和 Windows Server 的安全性基準

草稿版本[安全性組態基準設定](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/)版本 1903年位於適用於 Windows 10 版本 1903，，和適用於 Windows Server。

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) 1.4.1 版本都提供。

SetupDiag 是命令列工具，可協助您診斷 Windows 更新失敗的原因。 SetupDiag 透過搜尋 Windows 安裝程式記錄檔運作。 搜尋記錄檔時，SetupDiag 會使用一組規則來比對已知的問題。 在 SetupDiag 53 rules.xml 檔案中包含的規則有最新版本，它會擷取 SetupDiag 執行時。 rules.xml 檔案將在有新版 SetupDiag 可供使用時更新。

## <a name="update-rollback-improvements"></a>更新復原改善

伺服器可以現在會自動從失敗中復原啟動藉由移除更新，如果啟動失敗引進新的驅動程式 」 或 「 品質更新安裝之後。 當裝置無法正確啟動最近安裝的驅動程式更新的品質之後，Windows 會立即自動解除安裝更新，以取得備份的裝置，並正常執行。

這項功能需要使用與的 Server Core 安裝選項的伺服器[Windows 修復環境](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)資料分割。

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Microsoft Defender 進階威脅防護 (ATP) 的增強功能

Windows Server 包含 Microsoft Defender 進階執行緒保護 (如需詳細資訊，請參閱 < [Windows Server 上的 Windows Defender 防毒軟體](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016))。 此版本包含下列增強功能：

- [攻擊面縮減](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction)– IT 系統管理員可以設定裝置，讓他們定義的進階的 web protection 允許和拒絕清單，針對特定的 URL 和 IP 位址。
- [下一個層代保護](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)– 控制項已擴充為保護從勒索軟體、 認證不當使用，以及透過抽取式存放裝置所傳輸的攻擊。
    - 啟用遠端執行階段證明完整性強制功能。
    - 竄改的功能 – 使用虛擬化安全性隔離離開 OS 和攻擊者的重大 ATP 安全性功能。
- Microsoft Defender ATP 新一代保護技術：
    - **進階機器學習服務**:使用進階的機器學習和 AI 模型，讓它可以防止 apex 使用創新的弱點可能會入侵技術、 工具和惡意程式碼的攻擊者獲得改善。
    - **緊急爆發保護**:提供將時自動更新裝置與新的智慧偵測到新的爆發緊急爆發保護。
    - **ISO 27001 認證的合規性**:可確保雲端服務已分析的潛在威脅、 弱點和影響，且已備妥的風險管理和安全性控制。
    - **地理位置支援**:支援地理位置及主權範圍的範例資料，以及可設定的保留原則。