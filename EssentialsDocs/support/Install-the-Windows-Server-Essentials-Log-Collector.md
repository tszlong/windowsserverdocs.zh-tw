---
title: 安裝 Windows Server Essentials 記錄檔收集器
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ade18cec590392f35e7ad6b30d9a22ccdce44dcd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837029"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>安裝 Windows Server Essentials 記錄檔收集器

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows Server Essentials 記錄檔收集器安裝精靈會將記錄檔收集器安裝為 Launchpad 增益集。 您可以在網路電腦或伺服器 (或兩者) 上安裝並使用記錄檔收集器。 安裝後，記錄檔收集器會顯示在儀表板上。  
  
###  <a name="BKMK_ToInstall"></a> 若要安裝記錄檔收集器  
  
1.  將記錄檔收集器安裝套件下載至任一伺服器或網路上的電腦。  
  
    > [!NOTE]
    >  您可以從 Microsoft [下載記錄檔收集器安裝套件](https://go.microsoft.com/fwlink/p/?LinkId=255470) 。  
  
2.  連按兩下 [記錄檔收集器] 圖示。  
  
3.  如果您是從網路電腦執行安裝，當系統提示時，請輸入您的伺服器系統管理員認證。  
  
4.  選擇接受 Microsoft 軟體授權合約。  
  
5.  如果只要在伺服器上安裝記錄檔收集器，請選取 [僅在伺服器上]  核取方塊。 如果要在所有網路電腦上安裝記錄檔收集器，請選取 [在伺服器與網路的所有電腦上]  核取方塊。  
  
6.  按一下 [安裝增益集] 。  
  
###  <a name="BKMK_Reinstall"></a> 重新安裝記錄檔收集器  
 如果需要重新安裝記錄檔收集器，您必須在伺服器和網路電腦上解除安裝記錄檔收集器後再重新安裝。 透過從儀表板解除安裝伺服器上的記錄檔收集器，所有網路電腦都將自動解除安裝記錄檔收集器。  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>解除安裝記錄檔收集器並重新安裝  
  
1.  開啟 [儀表板]。  
  
2.  按一下 [增益集]  索引標籤，從清單中選取 [記錄檔收集器]  ，然後按一下 [解除安裝] 。  
  

3.  執行先前的程序中的步驟 [To install the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall)，以下載並安裝記錄檔收集器。  

3.  執行先前的程序中的步驟 [To install the Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall)，以下載並安裝記錄檔收集器。  

  
### <a name="manually-install-the-log-collector"></a>手動安裝記錄檔收集器  
 如果安裝精靈無法安裝記錄檔收集器，您可以使用下列程序以在單一電腦上安裝記錄檔收集器。  
  
##### <a name="to-manually-install-the-log-collector"></a>手動安裝記錄檔收集器  
  
1.  重新命名成.cab.wssx 從下載的安裝檔案的副檔名。  
  
2.  按兩下安裝檔案名稱。  
  
3.  在系統提示時按一下 [確定]。  
  
4.  按兩下檔案名稱結尾 ˜.msi 並選取要將它解壓縮的資料夾。  
  
5.  瀏覽到含有解壓縮檔案的資料夾，按兩下安裝檔案以使用精靈完成安裝。
