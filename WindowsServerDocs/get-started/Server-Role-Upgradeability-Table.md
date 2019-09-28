---
title: Windows Server 2016 的伺服器角色升級和移轉矩陣
description: 顯示哪些伺服器角色可以升級或移轉至 Windows Server 2016。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 10/05/2016
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7e031a64-b1e6-4cf6-994a-e7c575835f6a
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 84500d3012eda99ff3b3c857fc4b5c154d0d0fe5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360176"
---
# <a name="server-role-upgrade-and-migration-matrix-for-windows-server-2016"></a>Windows Server 2016 的伺服器角色升級和移轉矩陣

>適用於：Windows Server 2016

這個頁面上的網格說明移至 Windows Server 2016 的特定伺服器角色升級和移轉選項。 如需個別角色移轉指南，請瀏覽[在 Windows Server 中移轉角色與功能](https://docs.microsoft.com/windows-server/get-started/migrate-roles-and-features)。 如需安裝和升級的詳細資訊，請參閱 [Windows Server Installation, Upgrade, and Migration](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade) (Windows Server 安裝、升級和移轉)。

|伺服器角色|是否可從 Windows Server 2012 R2 升級？|是否可從 Windows Server 2012 升級？|是否支援移轉？|是否可以在不停機的情況下完成移轉？|  
|-------------------|----------|--------------|--------------|----------|  
|Active Directory 憑證服務| 是|    是|    是|    否|
|Active Directory Domain Services|  是|    是|    是|    是|
|Active Directory Federation Services|  否| 否| 是|    否 (不需要新增節點至伺服器陣列)|
|Active Directory 輕量型目錄服務|   是|    是|    是|    是|
|Active Directory Rights Management Services|   是|    是|    是|    否|
|DHCP 伺服器|   是|    是|    是|    是|
|DNS 伺服器|    是|    是|    是|    否|
|容錯移轉叢集|使用包含節點 [暫停-清空]、[收回] 的 [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 程序，升級至 Windows Server 2016 並重新加入原始叢集時，則為是。 當伺服器遭升級用叢集移除後再新增至其他叢集時，則為是。|當伺服器不是叢集的一部分時，則為否。 當伺服器遭升級用叢集移除後再新增至其他叢集時，則為是。  |是|若是 Windows Server 2012 容錯移轉叢集，則為否。 若是含 Hyper-V VM 的 Windows Server 2012 R2 容錯移轉叢集，或執行向外延展檔案伺服器角色的 Windows Server 2012 R2 容錯移轉叢集，則為是。 請參閱 [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級)。|
|檔案和存放服務| 是|    是|    因子功能而異|  否|
|Hyper-V| 是。 (使用包含節點 [暫停-清空]、[收回] 的「叢集作業系統輪流升級」程序，升級至 Windows Server 2016 並重新加入原始叢集時，則為是)。|  否|   是|  若是 Windows Server 2012 容錯移轉叢集，則為否。 若是含 Hyper-V VM 的 Windows Server 2012 R2 容錯移轉叢集，或執行向外延展檔案伺服器角色的 Windows Server 2012 R2 容錯移轉叢集，則為是。 請參閱 [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級)。| 
|列印和傳真服務|    否| 否| 是 (Printbrm.exe)| 否|
|遠端桌面服務|   是，適用於所有子角色，但不支援混合模式的伺服器陣列|   是，適用於所有子角色，但不支援混合模式的伺服器陣列|   是|    否|
|Web 伺服器 (IIS)|  是|    是|    是|    否|
|Windows Server Essentials 體驗|  是|    不適用 - 新功能|  是|    否|
|Windows Server Update Services|    是|    是|    是|    否|
|工作資料夾|  是|    是|    是|    是，使用 [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (叢集作業系統輪流升級) 時從 WS 2012 R2 叢集進行。|

