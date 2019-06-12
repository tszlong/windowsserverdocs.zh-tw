---
title: 安裝 |升級 |移轉到 Windows Server 2019
description: 如何將全新安裝、 就地升級或移轉至 Windows Server 2019。
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
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810815"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>安裝 |升級 |移轉到 Windows Server 2019

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

> [!IMPORTANT]
> Windows Server 2008 R2 與 Windows Server 2008 的延伸支援於 2020 年 1 月結束。 [深入了解您的升級選項](http://aka.ms/upgradecenter)。

是該移轉至較新 Windows Server 版本的時候了嗎？ 根據您目前正在執行的版本，會有許多選項供您用來完成此作業。

## <a name="clean-install"></a>清除安裝
如果您想要從較舊版本的 Windows 伺服器移至 Windows Server 2019 的相同硬體上，您應該進行**全新安裝**，其中您只會直接透過舊的密碼，在相同的硬體上安裝較新的作業系統因此，刪除之前的作業系統。 這是最簡單的方法，但是您必須先備份資料並規劃重新安裝應用程式。 有幾件事需要注意，例如系統需求，因此請務必檢查的詳細資料[Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124)， [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558)， [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418)與[Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。

## <a name="in-place-upgrade"></a>就地升級：

如果您想要保留相同的硬體，而不需要壓平合併伺服器已設定的所有伺服器角色，您會想要**就地升級**、 由您移從舊版作業系統至較新的分支，保留您的設定，伺服器角色和資料保持不變。 例如，如果您的伺服器執行 Windows Server 2012 R2，您可以將它升級到 Windows Server 2016 或 Windows Server 2019。 不過，並非所有的舊版作業系統都途徑可以轉到每一個較新版本。 請參閱下圖中的 可用的升級路徑：

![Windows Server 就地升級路徑圖表](media/upgrade-paths.png)

如需升級的逐步指引，請瀏覽[Windows Server 升級中心](http://aka.ms/upgradecenter):

[![Windows Server 升級中心的螢幕擷取畫面](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>叢集作業系統輪流升級

叢集 OS 輪流升級可讓系統管理員可以從 Windows Server 2012 R2 和 Windows Server 2016 升級叢集節點的作業系統，而不需停止 HYPER-V 或向外延展檔案伺服器工作負載。 這項功能可讓您避免可能影響服務等級協定的停機時間。 [Cluster operating system rolling upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 會更詳細地討論這項新功能。

## <a name="migration"></a>移轉

Windows Server 移轉時，一次中將一個角色或功能移到另一部執行 Windows Server，相同或較新版本的目的地電腦執行 Windows Server 的來源電腦。 針對上述用途，我們將「移轉」定義為移動一個角色或功能及其資料到不同的電腦，而不是升級同一台電腦上的功能。 

## <a name="license-conversion"></a>授權轉換
在某些作業系統版本中，您可使用簡單的命令與適當的授權金鑰執行一個步驟，就能將特定的版本轉換成另一種版本。 此方式稱為**授權轉換**。 例如，如果您的伺服器執行 Windows Server 2016 Standard，可以將其轉換為 Windows Server 2016 Datacenter。 請記住，雖然您可以向上移動從 Server 2016 Standard Server 2016 資料中心，便無法反轉程序，然後從資料中心移至標準。 在某些版本的 Windows Server 中，您也可以使用相同的命令和適當的金鑰，免費在 OEM、大量授權和零售版本之間轉換。


 
 
