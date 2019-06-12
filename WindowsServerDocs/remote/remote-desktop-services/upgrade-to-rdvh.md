---
title: 升級您的遠端桌面虛擬化主機到 Windows Server 2016
description: 本文說明如何升級現有的遠端桌面服務部署至 Windows Server 2016。
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
ms.openlocfilehash: 9e624517e5e7910a32a68d1ebc38b3f8d5ab8459
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805216"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>升級您的遠端桌面虛擬化主機到 Windows Server 2016

>適用於：Windows Server （半年通道），Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安裝 RDS 角色支援 OS 升級
只能從 Windows Server 2012 R2 和 Windows Server 2016 TP5 支援至 Windows Server 2016 的升級。

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Vm 儲存在本機部署中的 rd 工作階段主機伺服器
這些伺服器應該全部一次升級。 請遵循下列步驟來升級：

1. 登出所有使用者。
1. 每一部主機開啟或關閉儲存所有虛擬機器。 
1. 升級至 Windows Server 2016 的伺服器。 
1. 升級完成之後，所有集合應該都是可用且正常運作。      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Vm 儲存在叢集共用磁碟區 (CSV) 部署中的 rd 工作階段主機伺服器 

1. 判斷的升級策略，其中部分 RDVH 伺服器將會升級，而某些會繼續在 Windows Server 2012 R2 上的主機 Vm。  
2. 隔離一或多個 RDVH 伺服器，針對第一回合的升級，將所有的 Vm 移轉到其他 ' 不是，尚未升級' RDVH 伺服器將會保留原始的 2012 R2 叢集的一部分。
    1. 開啟 [容錯移轉叢集管理員]。 
    1. 按一下 [角色]  。 
    1. 選取一或多個 Vm。 按一下滑鼠右鍵開啟操作功能表。 
    1. 按一下 **移動**選擇  **Live**或**快速移轉**將 Vm 移到一或多個不屬於初始升級 RD 虛擬化主機伺服器。 使用**Live**或是**快速**移轉根據硬體相容性或線上需求等因素而定。 
3. 收回 RDVH 伺服器，準備好升級，從原始的叢集。 
4. 隔離的 RDVH 伺服器都升級。 
5. 已成功升級目標的 RDVH 伺服器之後，請建立新的叢集和 CSV，必須是完全不同的 SAN 磁碟區上。
6. 將所有升級的 RDVH 伺服器加入新的叢集。 
7. 建立新的 csv 檔案，以模擬現有的 csv 檔案中的現有資料夾結構中的資料夾結構。 這包括集合資料夾和每個 VM 的最上層的層級子資料夾。 
8. 從原始的 CSV 上不同的 VM 集合資料夾，將複製的 /IMGS 資料夾和內容到新的集合資料夾上新增 CSV 的相同位置中。 
9. 在來源 RDVH 電腦上，使用叢集管理員來移除高可用性的虛擬機器的組態：
    1. 啟動 [叢集管理員]。 
    1. 按一下 [角色]  。 
    1. 以滑鼠右鍵按一下 VM 物件，然後按一下**移除**。 
10. 一部非升級 RDVH 伺服器上，使用 HYPER-V 管理員將所有的 Vm 移到其中一個新的叢集 CSV 與升級的 RDVH 伺服器：
    1. 開啟 \[Hyper-V 管理員\]。 
    2. 選取其中一個未升級 RDVH 伺服器。 
    3. 以滑鼠右鍵按一下其中一個 Vm 來移動，然後按一下**移動**。 
    4. 選擇**移動虛擬機器**，然後按一下**下一步**。 
    5. 提供目標升級的 RDVH 伺服器名稱**指定目的地電腦**頁面，然後再按一下**下一步**。 
    6. 選擇**虛擬機器的資料移到單一位置**，然後按一下**下一步**。 
    7. 瀏覽至的目的地位置。 
       > [!IMPORTANT]
       > 請確定這個路徑是空的資料夾，為特定的 VM。 

       > [!NOTE]
       > 如所述，您必須已經建立在此步驟之前的新目的地子資料夾。 [選取資料夾] 對話方塊將不允許您在此步驟中建立的子資料夾。 
    
       按一下 [下一步]  ，然後按 [完成]  。 
11. 一旦重新放置 Vm，會將他們新增為叢集**高可用性**物件：
     1. 開啟已升級的 RD 虛擬化主機伺服器上的容錯移轉叢集管理員。 
     1. 以滑鼠右鍵按一下**角色**節點，然後再按一下**設定角色**。 按一下 [**下一步**上**開始**高可用性精靈] 頁面。 
     1. 選擇**虛擬機器**從清單中可用的角色，然後再按一下**下一步**。 會顯示一份未設定的 Vm。 
     1. 選取所有的 Vm。 按一下 **下一步** ，然後按一下**下一步**再次在 確認 頁面來啟動組態工作。  
12. 一旦您已重新定位的所有 Vm，請將剩餘的 RDVH 伺服器升級。 您可以使用上述的步驟進行平衡 VM 為適當的位置。

> [!NOTE]  
> 不支援異質性叢集中的 HYPER-V 伺服器。 
