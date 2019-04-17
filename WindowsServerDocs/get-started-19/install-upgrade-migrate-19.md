---
title: 安裝 |升級 |移轉到 Windows Server 2019
description: 全新安裝、 就地升級或移轉到 Windows Server 2019 的方式。
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
ms.openlocfilehash: 58c363fc0a1e336519bc6ec4276651345cc2b5eb
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121387"
---
# 安裝 |升級 |移轉到 Windows Server 2019

>適用於： Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

> [!IMPORTANT]
> Windows Server 2008 R2 與 Windows Server 2008 的延伸支援於 2020 年 1 月結束。 [深入了解您的升級選項](http://aka.ms/upgradecenter)。

是該移轉至較新 Windows Server 版本的時候了嗎？ 根據您目前正在執行的版本，會有許多選項供您用來完成此作業。

## 全新安裝
如果您想要從舊版 Windows 伺服器以相同的硬體上的 Windows Server 2019 移動，您應該執行**全新安裝**，只安裝新版作業系統上使用相同的硬體，因此刪除舊的一個直接先前的作業系統。 這是最簡單的方法，但是您必須先備份資料並規劃重新安裝應用程式。 有幾個事項需要注意，例如系統需求，所以請務必檢查適用於[Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124)、 [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558)、 [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418)和[Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)的詳細資料。

## 就地升級
如果您想要保留相同硬體和您已經不需要將伺服器設定的所有伺服器角色，您會想要執行**就地升級**，所依據的您從瀏覽較舊的作業系統到較新的一個，讓您設定、 伺服器角色並不變的資料。 例如，如果您的伺服器執行 Windows Server 2012 R2，您可以將它升級至 Windows Server 2016 或 Windows Server 2019。 不過，並非所有的舊版作業系統都途徑可以轉到每一個較新版本。 下圖為可用的升級路徑，請參閱：

![Windows Server 就地升級路徑圖表](media/upgrade-paths.png)

如需逐步升級的詳細指引，請瀏覽[Windows 伺服器升級中心](http://aka.ms/upgradecenter)：

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Windows Server 升級中心"></a>

## 叢集作業系統輪流升級
叢集作業系統輪流升級可讓系統管理員升級叢集節點的作業系統從 Windows Server 2012 R2 和 Windows Server 2016，而不需停止 HYPER-V 或 「 向外延展檔案伺服器 」 工作負載。 這項功能可讓您避免可能影響服務等級協定的停機時間。 [Cluster operating system rolling upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 會更詳細地討論這項新功能。

## 移轉

Windows Server 移轉時，您會將一個角色或功能一次從另一部執行 Windows Server 相同或較新版本的目的地電腦來執行 Windows Server 的來源電腦。 針對上述用途，我們將「移轉」定義為移動一個角色或功能及其資料到不同的電腦，而不是升級同一台電腦上的功能。 

## 授權轉換
在某些作業系統版本中，您可使用簡單的命令與適當的授權金鑰執行一個步驟，就能將特定的版本轉換成另一種版本。 此方式稱為**授權轉換**。 例如，如果您的伺服器執行 Windows Server 2016 Standard，可以將其轉換為 Windows Server 2016 Datacenter。 在某些版本的 Windows Server 中，您也可以使用相同的命令和適當的金鑰，免費在 OEM、大量授權和零售版本之間轉換。


 
 
