---
title: Windows Server 版本 1903 和 1909 中的新功能
description: 本主題說明 Windows Server 版本 1903 和版本 1909 (半年通道版本) 中的一些新功能。
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 11/12/2019
ms.openlocfilehash: 13b9c1b004f57d46307664427f7ea3483c04c00f
ms.sourcegitcommit: b9ec35416a06854c1bc875a2b731d42a436fe313
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962010"
---
# <a name="whats-new-in-windows-server-versions-1903-and-1909"></a>Windows Server 版本 1903 和 1909 中的新功能

>適用於：Windows Server (半年通道)

本主題說明 Windows Server 版本 1903 (半年通道版本) 中的一些新功能。 這些功能包括執行及管理容器的功能優化、作用於核心安裝中的工具，以及從 Linux 裝置遷移儲存空間的能力。

Windows Server 版本 1909 是 Windows Server 的下一個半年通道版本，著重於可靠性、效能和其他一般改善功能，但沒有任何新功能。 與其他半年通道版本一樣，其第一次推出就提供 18 個月的支援。 如需半年通道版本支援日期的詳細資訊，請參閱 [Windows Server 版本資訊](../get-started/windows-server-release-info.md)。

若要另外了解 Windows Server 最新長期維護通道 (LTSC) 版本中的新功能，請參閱 [Windows Server 2019 的新功能](../get-started-19/whats-new-19.md)。 另請參閱 [Windows 10 (版本 1903) 新功能 IT 專業人員內容](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903)。

此版本的系統需求與 Windows Server 2019 相同—如需詳細資訊，請參閱[系統需求](../get-started-19/sys-reqs-19.md)。 若要查看最近移除的項目，請參閱[從 Windows Server 版本 1903 開始移除或計劃取代的功能](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Windows 容器必須使用相同版本的 Windows 作為主機伺服器 (或是「更早」  的版本)。 例如，執行 Windows Server 發行版本 1903 版 (組建 18342) 的主機伺服器，可以執行相同或此之前版本及組建號碼的 Windows Server 容器 (即使容器使用 Insider Preview 版的 Windows)。 如需詳細資訊，請參閱 [Windows 容器版本相容性](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility)。

## <a name="enhanced-support-for-non-microsoft-container-services"></a>非 Microsoft 容器服務的增強支援

我們已增強平台功能來支援 Azure容器服務和非 Microsoft 容器服務。

- 我們已整合 CRI-containerd 與主機計算服務 (HCS)，可在 Azure 上支援 Windows 容器及 Windows 上 Linux 容器 (LCOW) 的 Pod。
- 我們已與 Kubernetes 社群合作，藉此啟用 Windows 容器支援。 透過 Kubernetes v1.14 版本，Windows Server 節點支援已正式從 Beta 狀態轉換為穩定狀態。 如需詳細資訊，請參閱 [Kubernetes 現在支援 Windows 容器](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/)。
- Windows 的 Tigera Calico 現在已作為 Tigera Essentials 訂用帳戶的一部份來正式發行，並且提供可在混合式 Linux/Windows 環境中通用的非重疊網路和網路原則。
- 我們已改善延展性來增強 Windows 容器的重疊網路支援，包括透過最新版的 Flannel 和 Kubernetes v1.14 來與 Kubernetes 整合。 如需詳細資訊，請參閱 [Kubernetes 中的 Windows 支援簡介](https://kubernetes.io/docs/setup/windows/)。

## <a name="directx-hardware-acceleration-in-containers"></a>容器中的 DirectX 硬體加速

我們將在 Windows 容器中啟用 DirectX API 的硬體加速支援，以支援像是使用本機圖形處理單元 (GPU) 硬體進行 Machine Learning (ML) 推斷的案例。 如需詳細資訊，請參閱[將 GPU 加速帶入 Windows 容器](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939)。

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>更新容器身分識別和群組管理服務帳戶文件

我們已將更多範例和相容性資訊新增至[群組管理服務帳戶](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)文件，並讓您可從 PowerShell 資源庫中取得[認證規格的 PowerShell 模組](https://www.powershellgallery.com/packages/CredentialSpec)。 如需詳細資訊，請參閱[容器識別最新動向](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151)的部落格文章。

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>將工作排程器和Hyper-V 管理員新增至核心安裝

您可能已經知道，在生產環境中使用 Windows Server 半年通道時，我們會建議使用核心安裝選項。 不過，Server Core 預設會省略一些有用的管理工具。 您可以藉由安裝應用程式相容性功能，來新增許多最常用的工具，但仍會遺漏某些工具。

因此，根據客戶的意見反應，我們在此版本的應用程式相容性功能中加入兩個工具：工作排程器 (taskschd.msc) 和 Hyper-V 管理員 (virtmgmt.msc)。

如需詳細資訊，請參閱 [Server Core 應用程式相容性功能](../get-started-19/install-fod-19.md)。

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>儲存空間移轉服務現在可遷移本機帳戶、叢集和 Linux 伺服器

儲存空間移轉服務可讓您更輕鬆地將伺服器遷移至較新版本的 Windows Server。 其提供的圖形工具可清查伺服器上的資料，然後將資料和設定轉送至較新的伺服器 — 完全不需要應用程式或使用者變更任何項目。

使用此版本的 Windows Server 來協調移轉時，我們已新增下列功能：

- 將本機使用者和群組遷移到新伺服器
- 從容錯移轉叢集遷移儲存空間
- 從使用 Samba 的 Linux 伺服器遷移儲存空間
- 使用 Azure 檔案同步，更輕鬆地同步已遷移至 Azure 的共用
- 遷移新的網路，例如 Azure

如需有關儲存空間移轉服務的詳細資訊，請參閱[儲存空間移轉服務概觀](../storage/storage-migration-service/overview.md)。

## <a name="system-insights-disk-anomaly-detection"></a>系統深入解析的磁碟異常偵測

[系統深入解析](../manage/system-insights/overview.md)是預測性分析功能，可在本機上分析 Windows Server 系統資料，並提供伺服器的功能深入解析。 其隨附許多內建功能，但從磁碟異常偵測開始，我們已新增能夠透過 Windows Admin Center 安裝額外功能的能力。

磁碟異常偵測是一項新功能，可在磁碟出現與平常「不同」  的行為時醒目提示。 雖然不同不一定是件壞事，但查看這些異常狀態有助於針對系統問題進行疑難排解。

這項功能也適用於執行 Windows Server 2019 的伺服器。

## <a name="windows-admin-center-enhancements"></a>Windows Admin Center 功能強化

新版 Windows Admin Center 已推出，並在 Windows Server 中加入新功能。 如需最新的功能資訊，請參閱 [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

## <a name="security-baseline-for-windows-10-and-windows-server"></a>適用於 Windows 10 和 Windows Server 的安全性基準

適用於 Windows 10 版本 1903 和適用於 Windows Server 版本 1903 的[安全性組態基準設定](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/)草稿版已可供使用。

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) 版本 1.4.1 已可供使用。

SetupDiag 是命令列工具，可協助診斷 Windows 更新失敗的原因。 SetupDiag 透過搜尋 Windows 安裝程式記錄檔運作。 搜尋記錄檔時，SetupDiag 會使用一組規則來比對已知的問題。 在目前版本的 SetupDiag 中，有 53 個規則包含在 rules.xml 檔案中 (此檔案會在 SetupDiag 執行時解壓縮)。 rules.xml 檔案將在有新版 SetupDiag 可供使用時更新。

## <a name="update-rollback-improvements"></a>更新復原改善

如果安裝最新驅動程式或品質更新後已引入啟動錯誤，則伺服器現在可透過移除更新，從啟動錯誤中自動復原。 現在，如果在安裝最近的驅動程式品質更新後，裝置無法正確啟動，Windows 會自動解除安裝更新，並取得裝置備份以正常執行。

這項功能需要伺服器搭配 [Windows 修復環境](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)磁碟分割來使用核心安裝。

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Windows Defender 進階威脅防護 (ATP) 的功能改善

Windows Server 包含 Microsoft Defender 進階威脅防護 (如需詳細資訊，請參閱 [Windows Server 上的 Windows Defender 防毒軟體](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016))。 本版本包含下列改善功能：

- [攻擊面縮減](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction) – IT 系統管理員可以透過進階的 Web 防護來設定裝置，讓他們能夠針對特定 URL 和 IP 位址定義允許和拒絕清單。
- [新一代保護](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) – 控制項已擴展為可抵禦勒索軟體、認證不當使用，以及透過抽取式存放裝置所傳輸的攻擊。
    - 完整性強制功能 – 啟用遠端執行階段證明。
    - 防止竄改的功能 – 使用以虛擬化為基礎的安全防護，將重大 ATP 安全性功能與 OS 和攻擊者隔離。
- Microsoft Defender ATP 的新一代保護技術：
    - **Azure Machine Learning**：使用進階的機器學習和 AI 模型進行改良，使其可以抵禦使用創新弱點入侵技術、工具和惡意程式碼的 apex 攻擊者。
    - **緊急爆發保護**：緊急爆發保護可在偵測到新爆發時，以新的智慧功能自動更新裝置。
    - **ISO 27001 認證的合規性**：確保雲端服務已分析潛在威脅、弱點和影響，且已備妥風險管理和安全性控制。
    - **地理位置支援**：在範例資料及可設定的保留原則上支援地理位置及主權範圍。