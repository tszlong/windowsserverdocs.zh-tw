---
title: 安裝 Windows Server Essentials 記錄檔收集器
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 75faeb3fe1c9ced32a37f2ac96ffedbadb2db76f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318680"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>安裝 Windows Server Essentials 記錄檔收集器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

Windows Server Essentials 記錄檔收集器安裝精靈會將記錄檔收集器安裝為啟動控制板增益集。 您可以在網路電腦或伺服器 (或兩者) 上安裝並使用記錄檔收集器。 安裝後，記錄檔收集器會顯示在儀表板上。  
  
###  <a name="to-install-the-log-collector"></a><a name="BKMK_ToInstall"></a>若要安裝記錄檔收集器  
  
1.  將記錄檔收集器安裝套件下載至任一伺服器或網路上的電腦。  
  
    > [!NOTE]
    > [下載 Windows Server Essentials 記錄檔收集器安裝套件](https://www.microsoft.com/download/details.aspx?id=34821)。  
  
2.  連按兩下 [記錄檔收集器] 圖示。  
  
3.  如果您是從網路電腦執行安裝，當系統提示時，請輸入您的伺服器系統管理員認證。  
  
4.  選擇接受 Microsoft 軟體授權合約。  
  
5.  如果只要在伺服器上安裝記錄檔收集器，請選取 [僅在伺服器上] 核取方塊。 如果要在所有網路電腦上安裝記錄檔收集器，請選取 [在伺服器與網路的所有電腦上] 核取方塊。  
  
6.  按一下 [安裝增益集]。  
  
###  <a name="reinstalling-the-log-collector"></a><a name="BKMK_Reinstall"></a>重新安裝記錄檔收集器  
 如果需要重新安裝記錄檔收集器，您必須在伺服器和網路電腦上解除安裝記錄檔收集器後再重新安裝。 透過從儀表板解除安裝伺服器上的記錄檔收集器，所有網路電腦都將自動解除安裝記錄檔收集器。  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>解除安裝記錄檔收集器並重新安裝  
  
1.  開啟 [儀表板]。  
  
2.  按一下 [增益集] 索引標籤，從清單中選取 [記錄檔收集器]，然後按一下 [解除安裝]。  
  

3.  執行先前的程序[安裝記錄檔收集器](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall)中的步驟以下載並安裝記錄檔收集器。  

3.  執行先前的程序[安裝記錄檔收集器](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall)中的步驟以下載並安裝記錄檔收集器。  

  
### <a name="manually-install-the-log-collector"></a>手動安裝記錄檔收集器  
 如果安裝精靈無法安裝記錄檔收集器，您可以使用下列程序以在單一電腦上安裝記錄檔收集器。  
  
##### <a name="to-manually-install-the-log-collector"></a>手動安裝記錄檔收集器  
  
1.  將下載之安裝檔案的副檔名從. onlinebackupaddin.wssx 重新命名為 .cab。  
  
2.  按兩下安裝檔案名稱。  
  
3.  在系統提示時按一下 [確定]。  
  
4.  按兩下以 ' .msi ' 結尾的檔案名，然後選取要在其中解壓縮的資料夾。  
  
5.  瀏覽到含有解壓縮檔案的資料夾，按兩下安裝檔案以使用精靈完成安裝。
