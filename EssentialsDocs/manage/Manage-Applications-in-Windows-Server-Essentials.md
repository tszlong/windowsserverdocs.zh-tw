---
title: 管理 Windows Server Essentials 中的應用程式
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ae89c46a-0afd-4858-9150-ec97650f45a4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64c740969e65da1798b5647dd01979756af8ebb9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852851"
---
# <a name="manage-applications-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的應用程式

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
 
 已安裝 Windows server Essentials 體驗角色的 Windows Server Essentials 和 Windows Server 2012 R2 中的伺服器儀表板，可讓您執行一般系統管理工作。 若要執行這些工作，請參閱下列各節：  
  
-   [儀表板中的應用程式管理工作](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [使用儀表板安裝或移除增益集](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)  
  
##  <a name="application-management-tasks-in-the-dashboard"></a><a name="BKMK_1"></a>儀表板中的應用程式管理工作  
 [儀表板] 的 [應用程式] 管理頁面提供：  
  
- 一份已安裝的增益集清單，當中顯示：  
  
  -   線上服務或增益集的名稱  
  
  -   增益集的更新狀態  
  
  -   增益集的訂閱狀態  
  
  -   提供增益集之公司或發行者的名稱  
  
- 一個包含一組可管理所選增益集之工作的工作窗格  
  
- 一份可從 Microsoft Pinpoint 下載並安裝的增益集清單  
  
  下表說明伺服器 [儀表板] 中可用的各種增益集管理工作。 部分工作是增益集特定的工作，只有當您選取清單中的某個增益集時才能看見。  
  
|工作名稱|描述|  
|---------------|-----------------|  
|移除增益集|會將選取的增益集從伺服器及網路中的所有其他電腦中移除。|  
|在網路電腦上安裝增益集|會協助您在網路中的所有其他電腦上排定安裝選取的增益集。|  
|取得增益集的說明|會將您的網際網路瀏覽器開啟至一個網站，可供您搜尋問題解決方案，並深入了解所選取的增益集。|  
|更新增益集|會協助您下載並安裝您伺服器和網路電腦上已安裝之增益集適用的更新。|  
|更新增益集的訂閱|會將您的網際網路瀏覽器開啟至可供您更新增益集訂閱的網站。|  
|閱讀增益集的隱私權聲明|會將您的網際網路瀏覽器開啟至可供您檢視隱私權聲明的網站。|  
|我要如何安裝或移除增益集？|會將您的網際網路瀏覽器開啟至顯示主體說明主題的網頁。|  
  
##  <a name="install-or-remove-add-ins-using-the-dashboard"></a><a name="BKMK_2"></a>使用儀表板安裝或移除增益集  
 增益集是為您的伺服器提供額外特性和功能的軟體應用程式。 Microsoft 及其他獨立軟體廠商 (ISV) 皆提供越來越多的增益集。  
  
 您必須先將增益集安裝在伺服器上，才能利用增益集所提供的擴充功能。  
  
#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>安裝來自 Microsoft Pinpoint 的增益集  
  
1.  在 [伺服器儀表板] 中，按一下 [**應用程式**]，然後按一下 [ **Microsoft 定點**] 索引標籤。 可用的增益集清單隨即出現。  
  
2.  按一下您想要安裝的增益集。 增益集資訊頁面隨即出現。  
  
3.  在增益集資訊頁面上，按一下 [下載]，然後依照畫面上的指示來下載並安裝增益集。  
  
4.  依照精靈中的指示來安裝增益集。  
  
5.  安裝完成時，請重新啟動 [儀表板]，開啟伺服器 [儀表板] 的 [應用程式] 頁面，並確認增益集出現在清單檢視中。  
  
#### <a name="to-install-an-add-in-from-another-provider"></a>安裝來自其他提供者的增益集  
  
1.  開啟 [Windows 檔案總管] 並瀏覽至增益集安裝檔案的位置。  
  
2.  按兩下該檔案來執行安裝精靈。  
  
3.  依照精靈中的指示來安裝增益集。  
  
4.  安裝完成時，請重新啟動 [儀表板]，開啟 [應用程式] 頁面，並確認增益集出現在清單檢視中。  
  
#### <a name="to-remove-an-add-in"></a>移除增益集  
  
1.  開啟伺服器 [儀表板]。  
  
2.  按一下 [應用程式] 索引標籤。  
  
3.  在 [增益集] 索引標籤上，選取您想要移除的增益集，然後按一下 [移除增益集]。  
  
4.  在 [增益集移除] 視窗中，按一下 [移除]。  
  
    > [!NOTE]
    >  您可能需要重新啟動 [儀表板]，才能將增益集完全移除。  
  
## <a name="see-also"></a>另請參閱  
  
-   [儀表板總覽](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
