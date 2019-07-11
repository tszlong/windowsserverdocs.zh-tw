---
title: 安裝 | 升級 | 移轉到 Windows Server 2019
description: 如何全新安裝、就地升級或移轉到 Windows Server 2019。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 1fd955a640832eb161666f74b93d91bb2c3eff11
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810815"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>安裝 | 升級 | 移轉到 Windows Server 2019

>適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

> [!IMPORTANT]
> Windows Server 2008 R2 與 Windows Server 2008 的延伸支援於 2020 年 1 月結束。 [了解您的升級選項](http://aka.ms/upgradecenter)。

是該移轉至較新 Windows Server 版本的時候了嗎？ 根據您目前正在執行的版本，會有許多選項供您用來完成此作業。

## <a name="clean-install"></a>全新安裝
如果您想要在相同硬體上將舊版 Windows Server 移轉至 Windows Server 2019，則應該執行**全新安裝**，您只要直接在同一個硬體的舊版作業系統上安裝新版作業系統即可，這樣也會刪除先前的作業系統。 這是最簡單的方法，但您必須先備份資料並規劃重新安裝應用程式。 有幾個事項需要注意，例如系統需求，因此請務必檢查 [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124)、[Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558)、[Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) 和 [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx) 的詳細資料。

## <a name="in-place-upgrade"></a>就地升級

如果您想要在不使伺服器平階化的情況下保留相同硬體和所有已設定的伺服器角色，則需要執行**就地升級**，透過此方法，您會從舊版作業系統轉變到新版本，而您的設定、伺服器角色和資料則維持不變。 例如，如果您的伺服器執行 Windows Server 2012 R2，則可以將它升級到 Windows Server 2016 或 Windows Server 2019。 不過，並非所有的舊版作業系統都有路徑可以轉到每一個較新版本。 請參閱下圖，以取得可用的升級路徑：

![Windows Server 就地升級路徑圖表](media/upgrade-paths.png)

如需升級的逐步指引，請瀏覽 [Windows Server 升級中心](http://aka.ms/upgradecenter)：

[![Windows Server 升級中心的螢幕擷取畫面](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>叢集作業系統輪流升級

叢集作業系統輪流升級可讓系統管理員將叢集節點的作業系統從 Windows Server 2012 R2 和 Windows Server 2016 升級，而不需停止 Hyper-V 或「向外延展檔案伺服器」工作負載。 這項功能可讓您避免可能影響服務等級協定的停機時間。 [Cluster operating system rolling upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 會更詳細地討論這項新功能。

## <a name="migration"></a>移轉

Windows Server 移轉是指一次將一個角色或功能從執行 Windows Server 的來源電腦移轉至另一台執行相同或較新版本 Windows Server 的目的地電腦。 針對上述用途，我們將移轉定義為移動一個角色或功能及其資料到不同的電腦，而不是升級同一台電腦上的功能。 

## <a name="license-conversion"></a>授權轉換
在某些作業系統版本中，您可以使用簡單的命令與適當的授權金鑰執行一個步驟，將版本的特定版次轉換成相同版本的另一個版次。 此方式稱為**授權轉換**。 例如，如果您的伺服器執行 Windows Server 2016 Standard，可以將其轉換為 Windows Server 2016 Datacenter。 請記住，雖然您可以從 Server 2016 Standard 上移至 Server 2016 Datacenter，但無法反轉程序從 Datacenter 移至 Standard。 在某些版本的 Windows Server 中，您也可以使用相同的命令和適當的金鑰，免費在 OEM、大量授權和零售版本之間轉換。


 
 
