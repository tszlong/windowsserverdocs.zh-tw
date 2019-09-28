---
title: 使用 Azure 備份從 Windows 系統管理中心備份 Windows 伺服器
description: 使用 Windows 系統管理中心（Project 檀香山）以 Azure 備份備份 Windows Server
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server
ms.openlocfilehash: 77171e638fa1eb8c612bdc168868d8286ed23bb6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357400"
---
# <a name="backup-your-windows-servers-from-windows-admin-center-with-azure-backup"></a>使用 Azure 備份從 Windows 系統管理中心備份 Windows 伺服器

>適用於：Windows 管理中心預覽, Windows 系統管理中心

[深入瞭解 Azure 與 Windows 管理中心的整合。](../plan/azure-integration-options.md)

Windows 系統管理中心可簡化將 Windows Server 備份至 Azure 的程式，並防止意外或惡意的刪除、損毀，甚至是勒索軟體。 若要將安裝工作自動化，您可以將 Windows Admin Center 閘道連線至 Azure。

使用下列資訊來設定 Windows Server 的備份，並建立備份原則來備份您的伺服器磁片區和 Windows 系統狀態（來自 Windows 管理中心）。

## <a name="what-is-azure-backup-and-how-does-it-work-with-windows-admin-center"></a>什麼是 Azure 備份？它如何與 Windows 管理中心搭配運作？ 

**Azure 備份**是以 Azure 為基礎的服務，可用來備份（或保護）和還原 Microsoft cloud 中的資料。 Azure 備份以雲端式解決方案取代您現有的內部部署或異地備份解決方案，這是可靠、安全且更具成本效益的解決方案。
[深入瞭解 Azure 備份](https://docs.microsoft.com/azure/backup/backup-overview)。

Azure 備份提供您在適當的電腦、伺服器或雲端上下載和部署的多個元件。 您部署的元件或代理程式取決於您想要保護的內容。 所有 Azure 備份元件（無論您是否保護內部部署或 Azure 中的資料）都可以用來將資料備份到 Azure 中的復原服務保存庫。

Windows 管理中心的 Azure 備份整合，非常適合用來備份磁片區和 Windows 系統狀態內部部署 Windows 實體或虛擬伺服器。 這可提供完整的機制來備份檔案伺服器、網域控制站和 IIS 網頁伺服器。

Windows 管理中心會透過原生**備份**工具公開 Azure 備份整合。 **備份**工具提供了設定、管理和監視體驗，可以快速開始備份伺服器、執行一般備份和還原作業，以及監視 Windows 伺服器的整體備份健全狀況。

## <a name="prerequisites-and-planning"></a>先決條件和規劃

- 至少具有一個有效訂用帳戶的 Azure 帳戶
- 您想要備份的目標 Windows 伺服器必須能夠從網際網路存取 Azure
- [將您的 Windows 管理中心閘道連線至 Azure](azure-integration.md)

若要啟動工作流程來備份您的 Windows Server，請開啟伺服器連線，按一下 [**備份**] 工具，然後依照下列步驟進行。

## <a name="setup-azure-backup"></a>安裝 Azure 備份
當您針對尚未啟用 Azure 備份的伺服器連接按一下 [**備份**] 工具時，您會看到 [**歡迎使用 Azure 備份**] 畫面。 按一下 [**設定 Azure 備份**] 按鈕。 這會開啟 [Azure 備份安裝程式]。 遵循嚮導中的下列步驟來備份您的伺服器。

如果已設定 Azure 備份，按一下 [**備份**] 工具將會開啟 [**備份] 儀表板**。 請參閱（[管理和監視](#management-and-monitoring)）一節，以探索可以從儀表板執行的作業和工作。

### <a name="step-1-login-to-microsoft-azure"></a>步驟 1:登入 Microsoft Azure
登入您的 Azure 帳戶。 

> [!NOTE]
> 如果您已將 Windows 管理中心閘道連線至 Azure，您應該會自動登入 Azure。 您可以按一下 [**登出**]，以不同的使用者身分進一步登入。

### <a name="step-2-set-up-azure-backup"></a>步驟 2:設定 Azure 備份
為 Azure 備份選取適當的設定，如下所述

 - **訂用帳戶識別碼：** 您想要使用的 Azure 訂用帳戶將您的 Windows Server 備份至 Azure。 所有 Azure 資產（例如 Azure 資源群組）都會在選取的訂用帳戶中建立復原服務保存庫。
 - **保險櫃**將儲存伺服器備份的復原服務保存庫。 您可以從現有的保存庫中選取，否則 Windows 管理中心會建立新的保存庫。  
 - **資源群組：** Azure 資源群組是資源集合的容器。 復原服務保存庫會建立或包含在指定的資源群組中。 您可以從現有的資源群組中選取，或 Windows 管理中心會建立一個新的。
 - **位置:** 將在其中建立復原服務保存庫的 Azure 區域。 建議您選取最接近 Windows Server 的 Azure 區域。

### <a name="step-3-select-backup-items-and-schedule"></a>步驟 3：選取備份專案和排程

- 選取您想要從伺服器備份的內容。 Windows 管理中心可讓您從**磁片**區和**Windows 系統狀態**的組合中挑選，同時為您提供針對備份選取的預估資料大小。

> [!NOTE]
> 第一個備份是所有選取之資料的完整備份。 不過，後續的備份本質上是累加的，而且只會傳輸自上次備份之後的資料變更。

- 針對您的系統狀態和/或磁片區，選取多個預設的**備份**排程。

### <a name="step-4-enter-encryption-passphrase"></a>步驟 4：輸入加密複雜密碼

- 輸入您選擇的**加密複雜密碼**（最少16個字元）。  **Azure 備份**使用使用者設定和使用者管理的加密複雜密碼來保護您的備份資料。 從 Azure 備份復原資料時，需要加密複雜密碼。

> [!NOTE]
> 複雜密碼必須儲存在安全的異地位置，例如另一部伺服器或[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal)。 Microsoft 不會儲存複雜密碼，如果遺失或忘記複雜密碼，就無法加以抓取或重設。

- 檢查所有設定，**然後按一下 [** 套用]

Windows 管理中心接著會執行下列作業

1. 建立 Azure 資源群組（如果尚未存在）
2. 依照指定的方式建立 Azure 復原服務保存庫
3. 安裝 Microsoft Azure 復原服務代理程式並將其註冊到保存庫
4. 依據選取的選項建立備份和保留排程，並將它們與 Windows Server 建立關聯。

## <a name="management-and-monitoring"></a>管理和監視

成功設定 Azure 備份之後，當您開啟現有伺服器連接的備份工具時，就會看到 [**備份] 儀表板**。 您可以從 [**備份] 儀表板**執行下列工作

- **存取 Azure 中的保存庫：** 您可以在 [**備份] 儀表板**的 [**總覽**] 索引標籤中按一下 [復原**服務保存庫**] 連結，以移至 Azure 中的保存庫以執行一[組豐富的管理作業](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **執行臨機操作備份：** 按一下 [**立即備份**] 以進行臨機操作備份。 
- **監視作業並設定警示通知：** 流覽至 [儀表板] 的 [**工作**] 索引標籤，以監視進行中或過去的作業，並[設定警示通知](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts)以接收任何失敗作業或其他備份相關警示的電子郵件。
- **查看復原點並復原資料：** 按一下儀表板的 [**復原點**] 索引標籤來查看復原點，然後按一下 [**復原資料**]，以取得從 Azure 復原資料的步驟。