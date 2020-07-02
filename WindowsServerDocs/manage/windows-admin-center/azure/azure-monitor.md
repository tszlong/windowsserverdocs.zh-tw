---
title: 使用 Windows 管理中心的 Azure 監視器監視伺服器和設定警示
description: Windows 系統管理中心（Project 檀香山）與 Azure 監視器整合
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 03/24/2019
ms.openlocfilehash: d6c18e3c4ef052b2e2d274f491762d8e31de4758
ms.sourcegitcommit: c40c29683d25ed75b439451d7fa8eda9d8d9e441
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85833311"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>使用 Windows 管理中心的 Azure 監視器監視伺服器和設定警示

[深入瞭解 Azure 與 Windows 管理中心的整合。](../plan/azure-integration-options.md)

[Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)是一種解決方案，可從各種不同的資源（包括內部部署和雲端的 Windows 伺服器和 vm）收集、分析及採取遙測。 雖然 Azure 監視器會從 Azure Vm 和其他 Azure 資源提取資料，本文著重于 Azure 監視器如何與內部部署伺服器和 Vm 搭配使用，特別是 Windows 系統管理中心。 如果您有興趣瞭解如何使用 Azure 監視器來取得有關超交集叢集的電子郵件警示，請參閱[使用 Azure 監視器傳送電子郵件以健全狀況服務錯誤](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor)。

## <a name="how-does-azure-monitor-work"></a>Azure 監視器如何運作？
![](../media/azure-monitor-diagram.png)從內部部署 Windows 伺服器產生的 img 資料會收集在 Azure 監視器的 Log Analytics 工作區中。 在工作區中，您可以啟用各種監視解決方案 (多組提供特定案例深入解析的邏輯)。 例如，Azure 更新管理、Azure 資訊安全中心和適用於 VM 的 Azure 監視器，都是可以在工作區中啟用的監視解決方案。 

當您在 Log Analytics 工作區中啟用監視解決方案時，向該工作區回報的所有伺服器都會開始收集與該解決方案相關的資料，讓解決方案可以針對工作區中的所有伺服器產生深入解析。 

若要在內部部署伺服器上收集遙測資料，並將其推送至 Log Analytics 工作區，Azure 監視器需要安裝 Microsoft Monitoring Agent 或 MMA。 此外，某些監視解決方案需要次要代理程式。 例如，適用於 VM 的 Azure 監視器也需依賴 ServiceMap 代理程式，才能取得此解決方案所提供的其他功能。 

某些解決方案 (例如 Azure 更新管理) 也需依賴 Azure 自動化，來讓您集中管理 Azure 和非 Azure 環境中的資源。 例如，Azure 更新管理會使用 Azure 自動化，從 Azure 入口網站集中安排和協調您環境中機器的更新安裝。


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>Windows 管理中心如何讓您使用 Azure 監視器？

在 WAC 中，您可以啟用兩個監視解決方案：

- [Azure 更新管理](azure-update-management.md) (位於更新工具中)
- 適用於 VM 的 Azure 監視器（在 [伺服器設定] 中），虛擬機器深入解析

您可以從這些工具中的任一個開始使用 Azure 監視器。 如果您從未使用過 Azure 監視器，WAC 會自動布建 Log Analytics 工作區（並視需要 Azure 自動化帳戶），並在目標伺服器上安裝和設定 Microsoft Monitoring Agent （MMA）。 然後，在工作區中安裝對應的解決方案。 

比方說，如果您第一次移至 [更新] 工具來設定 Azure 更新管理，WAC 將會：

1. 在電腦上安裝 MMA
2. 建立 Log Analytics 工作區和 Azure 自動化帳戶（因為在此情況下，Azure 自動化帳戶是必要的）
3. 在新建立的工作區中安裝更新管理解決方案。

如果您想要在相同伺服器的 WAC 內新增另一個監視解決方案，WAC 只會將該解決方案安裝到該伺服器所連接的現有工作區中。 WAC 會另外安裝任何其他必要的代理程式。

如果您連線至不同的伺服器，但已設定 Log Analytics 工作區（透過 WAC 或在 Azure 入口網站中手動安裝），您也可以在伺服器上安裝 MMA 代理程式，並將它連線到現有的工作區。 當您將伺服器連線到工作區時，伺服器會自動開始收集資料，並向安裝在該工作區中的解決方案回報。

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>適用於虛擬機器的 Azure 監視器 (也稱為 虛擬機器深入解析)
>適用于： Windows 管理中心預覽

當您在 [伺服器設定] 中設定適用於 VM 的 Azure 監視器時，Windows 系統管理中心會啟用適用於 VM 的 Azure 監視器解決方案（也稱為虛擬機器深入解析）。 此解決方案可讓您監視伺服器健康情況和事件、建立電子郵件警示、取得整個環境中伺服器效能的彙總檢視，以及將連線到指定伺服器的應用程式、系統和服務視覺化。

> [!NOTE]
> 除了其名稱，VM insights 也適用于實體伺服器和虛擬機器。

Azure 監視器免費提供每位客戶每月 5 GB 的資料額度，您可以輕鬆地在一部或兩部伺服器上試用，而不必擔心會產生費用。 請繼續閱讀，以了解將伺服器上架到 Azure 監視器的其他優點，例如取得您環境中各伺服器上的系統效能彙總檢視。

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**設定伺服器以與 Azure 監視器搭配使用**

從伺服器連接的 [總覽] 頁面，按一下 [管理警示] 新按鈕，或移至 [伺服器設定] > 監視和警示]。 在此頁面中，按一下 [設定] 並完成 [安裝] 窗格，將您的伺服器上線至 Azure 監視器。 系統管理中心會負責布建 Azure Log Analytics 工作區、安裝必要的代理程式，並確保已設定 VM insights 解決方案。 完成後，您的伺服器會將效能計數器資料傳送至 Azure 監視器，讓您可以在 Azure 入口網站中，檢視及建立以這部伺服器為基礎的電子郵件警示。

### <a name="create-email-alerts"></a>**建立電子郵件警示**

將伺服器連結至 Azure 監視器之後，您就可以使用 [設定 > 監視和警示] 頁面中的智慧型超連結，流覽至 Azure 入口網站。 系統管理中心會自動啟用效能計數器的收集，因此您可以藉由自訂許多預先定義的查詢之一或自行撰寫，輕鬆[建立新的警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)。

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>* * 在多部伺服器之間取得匯總的觀點 * *

如果您將多部伺服器上線至 Azure 監視器內的單一 Log Analytics 工作區，您可以從 Azure 監視器內的[虛擬機器 Insights 解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)，取得所有這些伺服器的匯總視圖。  （請注意，只有虛擬機器 Insights for Azure 監視器的 [效能] 和 [對應] 索引標籤可用於內部部署伺服器– [健康情況] 索引標籤僅適用于 Azure Vm）。若要在 Azure 入口網站中查看此功能，請移至 Azure 監視器 > 虛擬機器（在 [深入解析] 底下），然後流覽至 [效能] 或 [對應] 索引標籤。

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**將連接到指定伺服器的應用程式、系統和服務視覺化**

當系統管理中心將伺服器將上線到 Azure 監視器內的 VM insights 解決方案中時，它也會亮起一項稱為[服務對應](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)的功能。 這項功能會自動探索應用程式元件，並對應服務之間的通訊，讓您可以輕鬆地從 Azure 入口網站中，以絕佳的詳細資料視覺化伺服器之間的連線。 您可以移至 [Azure 入口網站] > Azure 監視器 > 虛擬機器] （在 [深入解析] 底下），然後流覽至 [對應] 索引標籤，即可找到此資訊。

> [!NOTE]
> 適用于 Azure 監視器的虛擬機器深入解析的視覺效果目前會在6個公開區域中提供。  如需最新資訊，請參閱[適用於 VM 的 Azure 監視器文件](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics)。  您必須在其中一個支援的區域中部署 Log Analytics 工作區，以取得上述虛擬機器 Insights 解決方案所提供的其他好處。

## <a name="disabling-monitoring"></a>停用監視

若要完全中斷伺服器與 Log Analytics 工作區的連線，請卸載 MMA 代理程式。 這表示此伺服器將不會再將資料傳送到工作區，而安裝在該工作區中的所有解決方案都將無法再收集和處理來自該伺服器的資料。 不過，這不會影響工作區本身–向該工作區回報的所有資源都會繼續執行此動作。 若要在 Windows 系統管理中心內卸載 MMA 代理程式，請連接到伺服器，然後移至 [**已安裝的應用程式**]，尋找 Microsoft Monitoring Agent，然後選取 [**移除**]。

如果您想要關閉工作區中的特定解決方案，您將需要[從 Azure 入口網站移除監視解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution)。 若移除監視解決方案，表示將不再為向該工作區報告的「任何」伺服器產生該解決方案所建立的深入解析。 例如，如果我卸載適用於 VM 的 Azure 監視器解決方案，我就不會再從連線到 [我的工作區] 的任何電腦，看到 VM 或伺服器效能的深入解析。
