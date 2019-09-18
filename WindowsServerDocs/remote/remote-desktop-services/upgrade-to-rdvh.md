---
title: 將遠端桌面虛擬主機升級至 Windows Server 2016
description: 本文說明如何將現有的遠端桌面服務部署升級至 Windows Server 2016。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: a58231d908ff1ac32eca7d4ba3f1d5a6a18dd7fe
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870574"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>將遠端桌面虛擬主機升級至 Windows Server 2016

>適用於：Windows Server (半年度管道)、Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安裝 RDS 角色且受支援的作業系統升級
目前僅支援從 Windows Server 2012 R2 和 Windows Server 2016 TP5 升級至 Windows Server 2016。

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>將 VM 儲存於本機的部署中的 RD 虛擬主機伺服器
這些伺服器應同時升級。 請依照下列步驟進行升級：

1. 登出所有使用者。
1. 關閉或儲存每個主機上的所有虛擬機器。 
1. 將伺服器升級至 Windows Server 2016。 
1. 升級完成後，所有集合應該都可供使用且正常運作。      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>將 VM 儲存於叢集共用磁碟區 (CSV) 的部署中的 RD 虛擬主機伺服器 

1. 決定讓部分 RDVH 伺服器進行升級，部分伺服器繼續在 Windows Server 2012 R2 上託管 VM 的升級策略。  
2. 隔離一或多個 RDVH 伺服器，使其進行第一輪的升級；方法是將所有 VM 移轉至其他「還不要升級」、且將保留在原始 2012 R2 叢集中的 RDVH 伺服器。
    1. 開啟 [容錯移轉叢集管理員]。 
    1. 按一下 [角色]  。 
    1. 選取一或多個 VM。 按一下滑鼠右鍵以開啟操作功能表。 
    1. 按一下 [移動]  並選擇 [即時移轉]  或 [快速移轉]  ，將 VM 移至一或多個未納入初始升級中的 RD 虛擬主機伺服器。 根據硬體相容性或線上需求等因素，使用**即時**或**快速**移轉。 
3. 從原始叢集中收回準備好要升級的 RDVH 伺服器。 
4. 升級隔離的 RDVH 伺服器。 
5. 在預定的 RDVH 伺服器已成功升級後，請建立必須位於不同 SAN 磁碟區上的新叢集和 CSV。
6. 將所有升級的 RDVH 伺服器加入新的叢集。 
7. 在新的 CSV 檔案中資料夾結構，模擬現有 CSV 檔案中的現有資料夾結構。 這包括集合資料夾和每個 VM 最上層的子資料夾。 
8. 從原始 CSV 上的各個 VM 集合資料夾，將 /IMGS 資料夾和內容複製到新 CSV 上位於相同位置的新集合資料夾中。 
9. 在來源 RDVH 機器上，使用叢集管理員移除 VM 的高可用性設定：
    1. 啟動 [叢集管理員]。 
    1. 按一下 [角色]  。 
    1. 以滑鼠右鍵按一下 VM 物件，然後按一下 [移除]  。 
10. 在一個未升級的 RDVH 伺服器上，使用 Hyper-V 管理員將所有 VM 移至其中一個已升級的 RDVH 伺服器和新的叢集 CSV：
    1. 開啟 \[Hyper-V 管理員\]。 
    2. 選取其中一個未升級 RDVH 伺服器。 
    3. 以滑鼠右鍵按一下其中一個要移動的 VM，然後按一下 [移動]  。 
    4. 選擇 [移動虛擬機器]  ，然後按 [下一步]  。 
    5. 在 [指定目的地電腦]  頁面上提供已升級的目標 RDVH 伺服器名稱，然後按 [下一步]  。 
    6. 選擇 [將虛擬機器的資料移動到單一位置]  ，然後按 [下一步]  。 
    7. 瀏覽至目的地位置。 
       > [!IMPORTANT]
       > 請確定此路徑對應至特定 VM 的空資料夾。 

       > [!NOTE]
       > 如前所述，執行此步驟之前，您必須已建立新的目的地子資料夾。 [選取資料夾] 對話方塊將不允許您在此步驟中建立子資料夾。 
    
       按一下 [下一步]  ，然後按 [完成]  。 
11. VM 重新放置後，請將其新增為叢集的**高可用性**物件：
     1. 在已升級的 RD 虛擬主機伺服器上開啟 [容錯移轉叢集管理員]。 
     1. 以滑鼠右鍵按一下 [角色]  節點，然後按一下 [設定角色]  。 在「高可用性精靈」的 [開始]  頁面上按 [下一步]  。 
     1. 從可用的角色清單中選擇 [虛擬機器]  ，然後按 [下一步]  。 此時會顯示未設定的 VM 清單。 
     1. 選取所有 VM。 按 [下一步]  ，然後在確認頁面上再按一次 [下一步]  ，以啟動設定工作。  
12. 在重新放置所有 VM 後，請升級其餘的 RDVH 伺服器。 您可以視需要使用前述步驟來平衡 VM 位置。

> [!NOTE]  
> 不支援叢集中的異質性 Hyper-V 伺服器。 
