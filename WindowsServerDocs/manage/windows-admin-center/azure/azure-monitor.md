---
title: 監視伺服器，並使用 Windows Admin Center 從 Azure 監視器設定警示
description: 與 Azure 監視器整合的 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 6ada708bf7dd8cd08e1bc2620be5244a07beac7d
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296932"
---
# 監視伺服器，並使用 Windows Admin Center 從 Azure 監視器設定警示

[深入了解 Azure 與 Windows Admin Center 整合。](../plan/azure-integration-options.md)

[Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)是會收集、 分析，並據此採取遙測各種不同的資源，包括 Windows 伺服器及虛擬機器，這兩個內部部署和雲端解決方案。 雖然 Azure 監視器拖動資料從 Azure 虛擬機器，以及其他 Azure 的資源，這篇文章將焦點放在 Azure 監視與內部部署伺服器和虛擬機器，特別是使用 Windows Admin Center 的運作方式。 如果您有興趣了解如何使用 Azure 監視器，以取得關於您超融合式叢集的電子郵件警示，閱讀有關[使用 Azure 監視器，若要傳送電子郵件適用於健全狀況服務錯誤](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor)。

## Azure 監視器如何運作？
![img](../media/azure-monitor-diagram.png)從內部部署的 Windows 伺服器所產生的資料會收集在監視器 Azure Log Analytics 工作區中。 內 \] 工作區中，您可以啟用各種監視解決方案 — 設定的特定案例提供的深入資訊的邏輯。 例如，Azure 更新管理、 Azure 資訊安全中心和 Azure vm 的監視器是可以啟用 \] 工作區中的所有監視解決方案。 

當您啟用監視解決方案中 Log Analytics 工作區中，所有報告給該工作區的伺服器，將會開始收集資料的相關的解決方案，以便方案可以產生的所有伺服器的深入資訊 \] 工作區中。 

若要在內部部署伺服器上收集遙測資料和它推送到 Log Analytics 工作區，Azure 監視器需要安裝 Microsoft Monitoring Agent 或 MMA。 某些監視解決方案也會要求次要代理程式。 例如，Vm 的 Azure 監視器也取決於此解決方案所提供的額外功能的 ServiceMap 代理程式。 

某些解決方案，例如 Azure 更新管理，也取決於 Azure 自動化，可讓您集中管理跨 Azure 和非 Azure 環境的資源。 例如，Azure 更新管理會使用 Azure 自動化來排程並協調集中，在從 Azure 入口網站在您的環境中的電腦上安裝的更新。


## 如何沒有 Windows Admin Center 可讓您使用 Azure 監視器？

您可以在 WAC，內啟用兩個監視解決方案：

- [Azure 更新管理](azure-update-management.md)（在更新工具）
- Azure 虛擬機器 （伺服器設定） 的監視器，又名虛擬機器的深入資訊

您可以開始使用 Azure 監視器從這其中任這些工具。 如果以前從未用過之前 Azure 監視器，WAC 將會自動佈建 Log Analytics 工作區中 （以及 Azure 自動化帳戶，如有需要），並上安裝及設定 Microsoft Monitoring Agent (MMA) 目標伺服器。 然後將會在工作區安裝對應的方案。 

例如，如果您第一次移到安裝 Azure 更新管理更新工具，將會 WAC:

1. 在電腦上安裝 MMA
2. 建立 Log Analytics 工作區和 Azure 自動化帳戶 （因為在此情況下 Azure 自動化帳戶是必要的）
3. 安裝更新管理解決方案中的新建立的工作區。

如果您想要將相同的伺服器上新增另一個監視解決方案從 WAC 內，WAC 只需將會安裝該方案到該伺服器連線的現有的工作區。 此外，WAC 將會安裝任何其他必要的代理程式。

如果您連線到不同的伺服器，但已設定 Log Analytics 工作區 （無論是透過 WAC 或手動在 Azure 入口網站中），您也可以在伺服器上安裝 MMA 代理程式，並將它連線至現有的工作區。 當您將伺服器連線到工作區中時，會自動開始收集資料，並報告給安裝在該工作區中的解決方案。

## Azure 監視的虛擬機器 （亦即 虛擬機器的深入資訊）
>適用於： Windows Admin Center 預覽版

當您設定 Azure 監視 Vm 的伺服器設定中時，Windows Admin Center 可讓 Azure 監視器 Vm 解決方案，也稱為虛擬機器的深入資訊。 這個解決方案可讓您監視伺服器健康情況和事件、 建立電子郵件警示、 您的環境間取得空間以提供統一的伺服器效能檢視和視覺化應用程式、 系統和服務連線到指定的伺服器。

> [!NOTE]
> 它的名稱，儘管 VM 的深入資訊的運作方式的實體伺服器，以及虛擬機器。

使用 Azure 監視器免費 5 GB 的資料/月/客戶允許使用時間，您可以輕鬆地試用這伺服器或兩個，而不必擔心的取得收費。 例如在您的環境中的伺服器上取得系統效能的彙總的檢視，給額外的權益，請參閱上架伺服器的讀到 Azure 監視器。

### **設定您的伺服器，以搭配 Azure 監視器**

從伺服器連線的概觀頁面，按一下 [新增] 按鈕 「 管理警示 」，或移至伺服器設定 > 監視及警示。 在此頁面上上, 架到 Azure 監視器的伺服器，按一下 「 設定 」 並完成設定窗格。 系統管理員中心所負責的佈建 Azure Log Analytics 工作區中，安裝必要的代理程式，並確保 VM 的深入解析解決方案設定。 完成之後，您的伺服器會傳送效能計數器資料至 Azure 監視器，讓您能夠檢視及建立電子郵件根據此伺服器上，從 Azure 入口網站的警示。

### **建立電子郵件警示**

一旦您已連接至 Azure 監視器時您的伺服器，您可以使用內設定 > 監視及警示頁面智慧型的超連結瀏覽至 Azure 入口網站。 系統管理員中心會自動啟用是收集，因此您可以輕鬆地[建立新的警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)自訂的許多預先定義的查詢，其中一個，或撰寫您自己的效能計數器。

### * * 跨多個伺服器取得彙總的檢視 \] * *

如果您上架到單一 Log Analytics 工作區 Azure 監視器內的多個伺服器，您可以取得提供統一的所有這些伺服器檢視從 Azure 監視器內[的虛擬機器的深入資訊的解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)。  （請注意只的效能和對應索引標籤 Azure 監視器的虛擬機器深入資訊將會使用內部部署伺服器 – 只使用 Azure Vm 的健康情況] 索引標籤函式）。若要檢視這 Azure 入口網站中，移至 Azure 監視器 > 虛擬機器 （底下的深入解析），並瀏覽至 「 效能 」 或 「 地圖 」 的索引標籤。

### **視覺化應用程式、 系統和服務連線到指定的伺服器**

當系統管理員中心 onboards 於 Azure 監視器內的 VM 的深入解析解決方案的伺服器，它也凸顯了功能，稱為[地圖服務](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)。 這項功能會自動發現應用程式元件和地圖服務之間的通訊，以便您可以輕鬆地視覺化提供絕佳的細節，從 Azure 入口網站伺服器之間的連線。 您可以找到此移至 Azure 入口網站 > Azure 監視器 > 虛擬機器 （底下的深入解析），並瀏覽至 「 地圖 」 索引標籤。

> [!NOTE]
> 虛擬機器深入了解 Azure 監視器的視覺效果目前提供 6 個公用的區域中。  最新的資訊，請檢查[Azure 監視 Vm 文件](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics)。  您必須部署支援區域，以取得額外的權益上文所述的虛擬機器的深入解析解決方案所提供的其中一種 Log Analytics 工作區。

## 停用監視

若要完全中斷 Log Analytics 工作區您的伺服器，請解除安裝 MMA 代理程式。 這表示此伺服器不會再傳送資料至工作區，且所有已安裝在該工作區中的解決方案將不會再收集和處理來自該伺服器的資料。 不過，這不會影響的工作區本身 – 所有報告給該工作區的資源將會繼續執行這項操作。 若要解除安裝 MMA 代理程式 WAC 內，移至應用程式 & 功能、 尋找 Microsoft Monitoring Agent 並按一下 [解除安裝。

如果您想要關閉工作區中的特定方案，您將需要[移除從 Azure 入口網站監視解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution)。 移除監視解決方案表示的深入資訊建立的解決方案不會再產生的_任何_報告給該工作區的伺服器。 例如，如果我解除安裝 Azure Vm 解決方案監視器，我會不再看到從任何電腦連線到我 \] 工作區的 VM 或伺服器效能的相關的深入資訊。