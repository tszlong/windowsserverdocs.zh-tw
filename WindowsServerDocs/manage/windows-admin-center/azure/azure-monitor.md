---
title: 監視伺服器，並使用從 Windows Admin Center 的 Azure 監視器設定警示
description: 使用 Azure 監視器整合的 Windows Admin Center （專案檀香山）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 8f7baba465071cc95ab7492037ff25c5cd58219e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280439"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>監視伺服器，並使用從 Windows Admin Center 的 Azure 監視器設定警示

[深入了解 Azure 與 Windows Admin Center 整合。](../plan/azure-integration-options.md)

[Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)是解決方案，會收集、 分析和處理程式碼從各種不同的資源，包括 Windows 伺服器和 Vm，同時對內部部署和雲端中的遙測。 Azure 監視器會從 Azure Vm 和其他 Azure 資源中提取資料，雖然這篇文章著重於 Azure 監視器與內部部署伺服器和 Vm，並特別具有 Windows Admin Center 的運作方式。 如果您想要了解如何使用 Azure 監視器取得有關超交集叢集的電子郵件警示，閱讀[使用 Azure 監視器 」 來傳送電子郵件的健全狀況服務錯誤](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor)。

## <a name="how-does-azure-monitor-work"></a>Azure 監視器的運作方式？
![i m g](../media/azure-monitor-diagram.png)從內部部署 Windows 伺服器產生的資料都收集在 Azure 監視器中的 Log Analytics 工作區。 工作區中，在中，您可以啟用各種監視解決方案，設定的邏輯，在特定案例提供深入解析。 比方說，Azure 更新管理、 Azure 資訊安全中心和適用於 Vm 的 Azure 監視器是所有監視解決方案，可以啟用工作區中。 

當您啟用 Log Analytics 工作區中的監視解決方案時，報告的所有伺服器該工作區會都開始收集資料與該方案，以便方案可以在工作區中產生的所有伺服器的深入解析。 

若要在內部部署伺服器上收集遙測資料，並將它推送到 Log Analytics 工作區，Azure 監視器會需要安裝 Microsoft Monitoring Agent 或 MMA。 某些監視解決方案也需要次要代理程式。 例如，適用於 Vm 的 Azure 監視器也取決於此解決方案提供的其他功能的 ServiceMap 代理程式。 

某些解決方案，例如 Azure 更新管理，也取決於 Azure 自動化中，可讓您集中管理跨 Azure 和非 Azure 環境的資源。 例如，Azure 更新管理會使用 Azure 自動化排程及協調集中，在從 Azure 入口網站的機器會在您的環境中，安裝更新。


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>如何確實 Windows Admin Center 可讓您使用 Azure 監視器？

從在 WAC，您可以啟用兩種監視解決方案：

- [Azure 更新管理](azure-update-management.md)（在 [更新] 工具）
- Azure 監視器的 Vm （在伺服器設定），又稱為虛擬機器的深入解析

您可以開始使用 Azure 監視器從任一工具。 Log Analytics 工作區 （和 Azure 自動化帳戶，如有需要），如果您從未使用過 Azure 監視器之前，會自動提供 WAC，安裝並設定目標伺服器上的 Microsoft Monitoring Agent (MMA)。 便會安裝到工作區對應的解決方案。 

比方說，如果您第一次移至 [更新] 工具來設定 Azure 更新管理，將會 WAC:

1. 在電腦上安裝 MMA
2. 建立 Log Analytics 工作區及 Azure 自動化帳戶 （因為 Azure 自動化帳戶必須在此情況下）
3. 在新建立的工作區中安裝的更新管理解決方案。

如果您想要加入另一個監視方案從 WAC 內相同的伺服器上，WAC 只會安裝該解決方案到該伺服器所連接的現有工作區。 此外，WAC 會安裝任何其他必要的代理程式。

如果您連接到不同的伺服器，但已經設定 Log Analytics 工作區 （無論是在 Azure 入口網站中透過 WAC 或手動），您也可以在伺服器上安裝 MMA 代理程式，並將它連接到現有的工作區。 當您將伺服器連接到工作區時，會自動啟動收集資料，並回報給該工作區中安裝的解決方案。

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Azure 監視的虛擬機器 （也稱為 虛擬機器的深入解析）
>適用於：Windows Admin Center 預覽版

當您設定 Azure 監視 Vm 的伺服器設定中時，Windows Admin Center 可讓 Azure 監視器，針對 Vm 解決方案，也就是虛擬機器的深入解析。 此解決方案可讓您監視伺服器健全狀況和事件、 建立電子郵件警示，在您的環境中，取得伺服器效能的彙總的檢視和視覺化應用程式、 系統與服務連接到指定的伺服器。

> [!NOTE]
> 儘管其名稱、 VM insights 仍適用於實體伺服器，以及虛擬機器。

使用 Azure 監視器的免費 5 GB 的資料/月/客戶額度，您可以輕鬆試用此伺服器，或兩個，而不必擔心費用。 讀入入，請參閱額外的好處，伺服器上架的 Azure 監視器，例如取得跨環境中伺服器的系統效能的彙總的檢視。

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**設定您的伺服器，以搭配 Azure 監視器**

從伺服器連接中的 概觀 頁面中，按一下 管理警示、 新增 按鈕，或移至 伺服器設定 > 監視及警示。 在此頁面上上, 架至 Azure 監視器的伺服器，依序按一下 [設定] 並完成 [設定] 窗格。 系統管理中心會負責佈建 Azure Log Analytics 工作區中，安裝必要的代理程式，且會確保 VM insights 方案設定。 完成後，您的伺服器將效能計數器資料傳送至 Azure 監視器，讓您檢視和建立此伺服器上，從 Azure 入口網站為基礎的電子郵件警示。

### <a name="create-email-alerts"></a>**建立電子郵件警示**

一旦您 Azure 監視器中連結您的伺服器之後，您可以使用智慧型的超連結，在設定內 > 監視及警示的頁面，即可瀏覽至 Azure 入口網站。 系統管理中心會自動啟用效能計數器來收集，因此您可以輕鬆地[建立新的警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)許多預先定義的查詢，而自訂或撰寫您自己。

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * 在多部伺服器中取得的合併的檢視 * *

如果您上架到 Azure 監視器中的單一 Log Analytics 工作區的多部伺服器，您可以取得從所有這些伺服器的合併的檢視[虛擬機器的深入解析解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)Azure 監視器中。  （請注意，只有效能和對應的索引標籤的虛擬機器的 Azure 監視器的深入解析會與內部部署伺服器 – 只具有 Azure Vm 的健全狀況] 索引標籤函式。）若要檢視這在 Azure 入口網站中，移至 [Azure 監視 > 虛擬機器 （在下深入解析），並瀏覽至 「 效能 」 或 「 地圖 」 索引標籤。

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**以視覺化方式檢視應用程式、 系統與服務連接到指定的伺服器**

當系統管理中心將上線到 VM 的深入解析解決方案，Azure 監視器中的伺服器，它也凸顯了一項功能稱為[服務對應](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)。 這項功能會自動探索應用程式元件，並對應服務之間的通訊，以便您可以輕易地視覺化很棒的詳細資訊，從 Azure 入口網站的伺服器之間的連線。 您可以找到此方法是前往 Azure 入口網站 > Azure 監視器 > （下深入解析） 虛擬機器，並瀏覽至 「 Maps 」 索引標籤。

> [!NOTE]
> Azure 監視器的虛擬機器的深入解析視覺效果目前提供 6 個公用區域中。  如需最新資訊，請檢查[Vm 文件的 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics)。  您必須部署 Log Analytics 工作區，其中一種支援的區域，以取得額外的好處，上面所述的虛擬機器的深入解析解決方案所提供。

## <a name="disabling-monitoring"></a>停用監視

若要完全中斷您的伺服器從 Log Analytics 工作區，解除安裝 MMA 代理程式。 這表示此伺服器將不會再傳送資料至工作區中，而且該工作區中安裝的解決方案不會再將所有收集和處理來自該伺服器的資料。 不過，這不會影響工作區中自行 – 所有回報給該工作區的資源會繼續進行此作業。 若要解除安裝內 WAC MMA 代理程式，請移至應用程式與功能，尋找 Microsoft Monitoring Agent，並按一下 解除安裝。

如果您想要關閉工作區中特定的解決方案，您必須[從 Azure 入口網站中移除的監視解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution)。 移除監視解決方案表示不會再將為產生該解決方案所建立的深入解析_任何_回報給該工作區的伺服器。 比方說，如果解除安裝 Vm 解決方案的 Azure 監視器，我將無法再看到從任何機器連線到我的工作區的 VM 或伺服器效能的相關深入解析。