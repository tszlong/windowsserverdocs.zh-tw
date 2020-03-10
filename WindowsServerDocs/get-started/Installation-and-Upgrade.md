---
title: Windows Server 安裝與升級
description: 如何安裝、升級或移轉到較新版本的 Windows Server。
ms.prod: windows-server
ms.date: 05/14/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 140f67a9dab5cf1f10cdb0c5c51a031a0dfb9dd3
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371588"
---
# <a name="windows-server-installation-and-upgrade"></a>Windows Server 安裝與升級

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

想要使用 Windows Server 2019？ 請參閱[安裝、升級或移轉到 Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md)。

> [!IMPORTANT]
> Windows Server 2008 R2 與 Windows Server 2008 的延伸支援於 2020 年 1 月結束。 [了解您的升級選項](#upgrading-from-windows-server-2008-r2-or-windows-server-2008)。

是該移轉至較新 Windows Server 版本的時候了嗎？ 根據您目前正在執行的版本，會有許多選項供您用來完成此作業。

## <a name="installation"></a>安裝

如果您想要在相同硬體上移轉至較新版本的 Windows Server，有一個一律有效的方法就是**全新安裝**，您只要直接在同一個硬體的舊版作業系統上安裝新版作業系統即可，這樣也會刪除先前的作業系統。 這是最簡單的方法，但您必須先備份資料並規劃重新安裝應用程式。 有幾個事項需要注意，例如系統需求，因此請務必檢查 [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558)、[Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) 和 [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx) 的詳細資料。

從任何發行前版本 (例如 Windows Server 2016 Technical Preview) 移轉至發行版本 (Windows Server 2016) 時，一律要進行全新安裝。

## <a name="migration-recommended-for-windows-server-2016"></a>移轉 (建議使用 Windows Server 2016)

Windows Server 移轉文件可協助您一次將一個角色或功能從執行 Windows Server 的來源電腦移轉至另一部執行相同或較新版本 Windows Server 的目的地電腦。 針對上述用途，我們將移轉定義為移動一個角色或功能及其資料到不同的電腦，而不是升級同一部電腦上的功能。 這是用來將現有工作負載及資料移至較新 Windows Server 版本的建議方式。 若要開始進行此作業，請查看 Windows Server 的[伺服器角色升級和移轉矩陣](https://go.microsoft.com/fwlink/?LinkId=828595)。

## <a name="cluster-os-rolling-upgrade"></a>叢集作業系統輪流升級
叢集作業系統輪流升級是 Windows Server 2016 中的新功能，可讓系統管理員將叢集節點的作業系統從 Windows Server 2012 R2 升級至 Windows Server 2016，而不需停止 Hyper-V 或「向外延展檔案伺服器」工作負載。 這項功能可讓您避免可能影響服務等級協定的停機時間。 [Cluster operating system rolling upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 會更詳細地討論這項新功能。

## <a name="license-conversion"></a>授權轉換
在某些作業系統版本中，您可以使用簡單命令與適當的授權金鑰執行一個步驟，將版本特定版次轉換成相同版本的另一個版次。 此方式稱為**授權轉換**。 例如，如果您的伺服器執行 Windows Server 2016 Standard，則可以將其轉換為 Windows Server 2016 Datacenter。 在某些版本的 Windows Server 中，您也可以使用相同命令和適當的金鑰，免費在 OEM、大量授權和零售版本之間轉換。

## <a name="upgrade"></a>升級
如果您想要在不使伺服器平階化的情況下保留相同硬體和所有伺服器角色，**升級**是一種選擇，但還有許多方式可以達到此目的。 在傳統型升級過程中，您會從舊版作業系統轉變到新版本，而您的設定、伺服器角色和資料則維持不變。 例如，如果伺服器執行 Windows Server 2012 R2，您可以將其升級至 Windows Server 2016。 不過，並非所有的舊版作業系統都有路徑可以轉到每一個較新版本。
 
>[!NOTE]
>虛擬機器中的升級效果最佳，因為虛擬機器不需要特定 OEM 硬體驅動程式，就能成功升級。
 
您可以從評估版的作業系統升級到零售版，或從較舊的零售版升級到較新的版本，而在某些情況下，您甚至可以從大量授權版本的作業系統升級到一般的零售版。

在您開始進行升級之前，請查看本頁面上表格以了解如何從您目前版本轉換到您想要的版本。

如需 Windows Server 2016 Technical Preview 可用安裝選項之間差異的資訊 (包括每個選項所安裝的功能，以及安裝後提供的管理選項)，請參閱 [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598)。

>[!NOTE]
>每當您移轉或升級到任何 Windows Server 版本時，都應該檢閱和了解該版本的[支援週期原則](https://support.microsoft.com/lifecycle)與時間範圍，並進行適當規劃。 您可以針對感興趣的特定 Windows Server 版本[搜尋週期資訊](https://support.microsoft.com/lifecycle)。
 
 
## <a name="upgrading-to-windows-server-2016"></a>升級至 Windows Server 2016
如需詳細資訊 (包括升級的重要注意事項和限制、Windows Server 2016 版本之間的授權轉換，以及評估版到零售版的轉換)，請參閱 [Supported Upgrade Paths for Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602) (Windows Server 2016 支援的升級路徑)。
 
>[!NOTE]
>注意：不支援將 Server Core 安裝切換到含桌面的伺服器安裝，以及將含桌面的伺服器安裝切換到 Server Core 安裝的升級。 如果您要升級或轉換的舊版作業系統為 Server Core 安裝，則結果仍然會是新版作業系統的 Server Core 安裝。
 
從舊版 Windows Server 零售版升級至 Windows Server 2016 零售版的支援路徑快速參考表：


|如果您執行下列版本和版次：|您可以升級到這些版本和版次：|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard 或 Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (使用叢集作業系統輪流升級功能)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### <a name="license-conversion"></a>授權轉換
您可以將 Windows Server 2016 Standard (零售) 轉換為 Windows Server 2016 Datacenter (零售)。

您可以將 Windows Server 2016 Essentials (零售) 轉換為 Windows Server 2016 Standard (零售)。

您可以將 Windows Server 2016 Standard 評估版轉換為 Windows Server 2016 Standard (零售) 或 Datacenter (零售)。

您可以將 Windows Server 2016 Datacenter 評估版轉換為 Windows Server 2016 Datacenter (零售)。
 
## <a name="upgrading-to-windows-server-2012-r2"></a>升級至 Windows Server 2012 R2
如需詳細資料 (包括升級的重要注意事項和限制、Windows Server 2012 R2 版本之間的授權轉換，以及評估版到零售版的轉換)，請參閱 [Upgrade Options for Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx) (Windows Server 2012 R2 升級選項)。

從舊版 Windows Server 零售版升級至 Windows Server 2012 R2 零售版的支援路徑快速參考表：

|如果您執行：|您可以升級到這些版本：|
|-------------------------|---------------------------|
|Windows Server 2008 R2 Datacenter (含 SP1)|Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Enterprise (含 SP1)|Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Standard (含 SP1)|Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter|
|Windows Web Server 2008 R2 (含 SP1)|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### <a name="license-conversion"></a>授權轉換
您可以將 Windows Server 2012 Standard (零售) 轉換為 Windows Server 2012 Datacenter (零售)。

您可以將 Windows Server 2012 Essentials (零售) 轉換為 Windows Server 2012 Standard (零售)。

您可以將 Windows Server 2012 Standard 評估版轉換為 Windows Server 2012 Standard (零售) 或 Datacenter (零售)。

## <a name="upgrading-to-windows-server-2012"></a>升級至 Windows Server 2012
如需詳細資料 (包括升級的重要注意事項和限制，以及評估版到零售版的轉換)，請參閱 [Evaluation Versions and Upgrade Options for Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx) (Windows Server 2012 評估版和升級選項)。
 
從舊版 Windows Server 零售版升級至 Windows Server 2012 零售版的支援路徑快速參考表：

|如果您執行：|您可以升級到這些版本：|
|--------------------------|--------------------------|
|Windows Server 2008 Standard (含 SP2) 或 Windows Server 2008 Enterprise (含 SP2)|Windows Server 2012 Standard、Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter (含 SP2)|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server 2008 R2 Standard (含 SP1) 或 Windows Server 2008 R2 Enterprise (含 SP1)|Windows Server 2012 Standard、Windows Server 2012 Datacenter|
|Windows Server 2008 R2 Datacenter (含 SP1)|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>授權轉換
您可以將 Windows Server 2012 Standard (零售) 轉換為 Windows Server 2012 Datacenter (零售)。

您可以將 Windows Server 2012 Essentials (零售) 轉換為 Windows Server 2012 Standard (零售)。

您可以將 Windows Server 2012 Standard 評估版轉換為 Windows Server 2012 Standard (零售) 或 Datacenter (零售)。

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>從 Windows Server 2008 R2 或 Windows Server 2008 升級

如[升級 Windows Server 2008 與 Windows Server 2008 R2](modernize-windows-server-2008.md) 中所述，Windows Server 2008 R2/Windows Server 2008 的延伸支援於 2020 年 1 月結束。 為了確保支援不間斷，您必須升級為支援的 Windows Server 版本，或透過移至[專用的 Windows Server 2008 R2 VM](uploading-specialized-WS08-image-to-azure.md) 來重新裝載在 Azure 中。 如需規劃移轉/升級的資訊和考量，請參閱 [Migration Guide for Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) (Windows Server 移轉指南)。

針對內部部署伺服器，沒有從 Windows Server 2008 R2 直接升級到 Windows Server 2016 或更新版本的路徑。 相反地，請先升級為 Windows Server 2012 R2，再[升級為 Windows Server 2016](#upgrading-to-windows-server-2016)。

當您規劃升級時，請注意升級為 Windows Server 2012 R2 中間步驟的下列方針。

  - 您無法從 32 位元架構就地升級為 64 位元架構，也無法從某個組建類型就地升級為另一個組建類型 (例如 fre 到 chk)。

  - 僅支援相同語言的就地升級。 您無法從某種語言升級為另一種語言。

  - 您無法從 Windows Server 2008 Server Core 安裝移轉到含有伺服器 GUI 的 Windows Server 2012 R2 (在 Windows Server 中稱為「含有完整桌面的伺服器」)。 您可以將升級的 Server Core 安裝切換到含有完整桌面的伺服器，但僅限在 Windows Server 2012 R2 上。 Windows Server 2016 和更新版本「不」  支援從 Server Core 切換到完整桌面，因此請在升級為 Windows Server 2016 之前進行切換。
  
如需詳細資訊，請參閱 [Evaluation Versions and Upgrade Options for Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)) (Windows Server 2012 評估版和升級選項)，其中包含角色特定的升級詳細資料。

