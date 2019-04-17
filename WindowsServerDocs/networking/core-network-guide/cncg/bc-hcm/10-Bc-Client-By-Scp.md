---
title: 設定服務連接點所 Client 自動裝載快取探索
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b12fa6f9e11c8816d74c9013dd80b3fa38d0a478
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>設定服務連接點所 Client 自動裝載快取探索

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用此程序中，您可以使用群組原則讓並執行下列 BranchCache\ 能力的 Windows 作業系統 domain\ 加入的電腦上設定 BranchCache 裝載快取模式。

- Windows 10 企業版
- Windows 10 教育版
- Windows 8.1 企業版
- Windows 8 企業版

> [!NOTE]  
> 若要設定 [加入網域的電腦是執行 Windows Server 2008 R2 或 Windows 7，查看 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)。

資格在**網域系統管理員**，或相當於的最低需求才能執行此程序。

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>若要使用群組原則，將裝載快取模式的伺服器設定

1. 在電腦上安裝所在的 Active Directory Domain Services 伺服器角色，開放伺服器管理員，選取 [本機伺服器，按一下 [**工具**，然後按**群組原則管理**。 群組原則管理主控台開啟。

2. 在群組原則管理主控台中，展開下列路徑：**樹系：***corp.contoso.com*、**網域**、*corp.contoso.com*，**群組原則物件**，其中*corp.contoso.com* BranchCache client 電腦帳號您想要設定的網域名稱。

3. Right\ 按一下**群組原則物件**，然後按一下 [**新增]**。 **新的 GPO**對話方塊。 在**名稱**，輸入名稱的新的群組原則物件 \(GPO\)。 如果您想要的物件 BranchCache Client 電腦的名稱，例如，輸入**BranchCache Client 電腦**。 按一下**[確定]**。

4. 在群組原則管理主控台中，確定**群組原則物件**已選取狀態，並在詳細資料窗格 right\-，按一下您剛 GPO 建立。 例如，如果您名 GPO BranchCache Client 部電腦上，按一下 right\ **BranchCache Client 電腦**。 按一下**編輯**。 群組原則編輯器] 管理主控台開啟。

5. 在群組原則編輯器] 管理主控台中，展開下列路徑：**電腦設定**，**原則**、**系統管理範本：原則定義 \(ADMX files\) 擷取的本機電腦**、**網路**、**BranchCache**。

6. 按一下**BranchCache**，然後在詳細資料窗格中，double\ 按**打開 BranchCache**。 **上 BranchCache 關閉**對話方塊。
  
7.  在**上 BranchCache 關閉**對話方塊中，按一下 [**啟用**，，然後按一下 [ **[確定]**。

8. 在群組原則編輯器] 管理主控台中，確定**BranchCache**仍然選取，然後在詳細資料窗格 double\ 按**讓自動裝載快取探索服務連接點來**。 原則設定對話方塊。

9. 在**讓自動裝載快取探索服務連接點，**對話方塊中，按**啟用**，，然後按一下 [ **[確定]**。

10. 若要讓 client 電腦下載及快取的 BranchCache content 檔案 server\ 型內容伺服器：在群組原則編輯器] 管理主控台中，確定**BranchCache**仍然選取，然後在詳細資料窗格 double\ 按**BranchCache 網路的檔案**。 **設定 BranchCache 網路檔案的**對話方塊。 
11. 在**設定 BranchCache 網路檔案的**對話方塊中，按**啟用**。 在**選項**，輸入數字，最大來回網路延遲時間，毫秒，然後按**[確定]**。
  
    > [!NOTE]
    > 根據預設，client 電腦快取 content 檔案伺服器的如果超過 80 毫秒來回網路延遲。
  
12. 若要設定的配置 BranchCache 快取的每個 client 電腦上的硬碟空間：在群組原則編輯器] 管理主控台中，確定**BranchCache**仍然選取，然後在詳細資料窗格 double\-中，按一下**設定的使用 client 電腦快取的磁碟空間**。 **設定用於 client 電腦快取的磁碟空間的**對話方塊。 按一下**啟用**，然後在**選項**輸入數字，表示百分比的每個 client 電腦上使用 BranchCache 快取的硬碟空間值。 按一下**[確定]**。

13. 天的區段是有效 client 電腦上 BranchCache 資料快取中指定的預設年代，：在群組原則編輯器] 管理主控台中，確定**BranchCache**仍然選取，然後在詳細資料窗格中，double\-按一下**區段的年齡設定資料快取中**。 **設定年齡區段的快取的資料在**對話方塊。 按一下**啟用**，然後在詳細資料窗格中輸入您想要的天數。 按一下**[確定]**。

14. 視您的部署設定 client 電腦的其他 BranchCache 原則設定。

15. 分支 office client 電腦上重新整理群組原則來執行命令**gpupdate /force**，或 client 電腦重新開機一次。

您 BranchCache 裝載快取模式部署現已完成。

本指南技術的其他資訊，請查看[其他資源](11-Bc-Hcm-additional-resources.md)。