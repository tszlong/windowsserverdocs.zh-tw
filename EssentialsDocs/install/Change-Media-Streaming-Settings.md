---
title: "媒體串流處理設定的變更"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dec690d2-f80c-4b09-99d6-3bba41331972
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d34d60e792fcda4d71a4509fe3d1b95fc6e74d0e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="change-media-streaming-settings"></a>媒體串流處理設定的變更

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

多個選項可讓您將媒體串流處理設定。 使用下列選項︰  
  
-   [隱藏遠端媒體串流增益集](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [設定媒體櫃名稱](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [將影片的串流品質]](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [以程式設計方式可讓或停用串流媒體](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a>隱藏遠端媒體串流增益集  
 您可以隱藏遠端媒體登錄中新增項目串流增益集。  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>若要隱藏遠端增益集串流媒體  
  
1.  在伺服器上，將滑鼠移到畫面的右上角，然後按一下**搜尋**。  
  
2.  在**搜尋**方塊中，輸入**regedit**，然後按**Regedit**應用程式。  
  
3.  在左窗格中，展開下登錄下列項目：  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  以滑鼠右鍵按一下**DisabledAddIns**，指向 [**新**，然後按一下 [ **DWORD 值**。  
  
5.  新的值，輸入**497796c6-9cc7-43be-aa26-4e6b5695370d**，這是識別碼遠端增益集串流媒體，然後按**輸入**。  
  
6.  以滑鼠右鍵按一下值，然後按一下**修改]**。  
  
7.  輸入**1**的數值資料，並再按**[確定]**。  
  
##  <a name="BKMK_LibraryName"></a>設定媒體櫃名稱  
 您可以使用在 Windows Server 方案 SDK 課程設定媒體櫃的名稱。 若要設定媒體櫃的名稱，您可以使用**SetMediaLibraryName**的方法**MediaStreamingManager**在**Microsoft.WindowsServerSolutions.MediaStreaming**命名空間。 下例示範如何設定媒體櫃的名稱︰  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 如需詳細資訊，請查看[Windows Server 方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
##  <a name="BKMK_StreamingQuality"></a>將影片的串流品質]  
 視訊品質串流取得 WinSAT CPU 分數和建立並安裝包含 WinSAT 分數資訊之.xml 檔案設定。 如果.xml 檔案，包含 WinSAT 分數資訊安裝之前，請先執行初始設定，設定視訊品質的使用者介面客戶將不會出現。 如需詳細資訊，請查看[設定伺服器 WinSAT 分數](Set-the-WinSAT-Score-on-the-Server.md)。  
  
##  <a name="BKMK_Program"></a>以程式設計方式可讓或停用串流媒體  
 您可以使用在 Windows Server 方案 SDK 課程以程式設計方式可讓或停用串流媒體。 若要讓或停用將媒體串流處理，您可以使用**SetMediaStreamingEnabled**的方法**MediaStreamingManager**在**Microsoft.WindowsServerSolutions.MediaStreaming**命名空間。 下列範例顯示如何讓串流媒體：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 下列範例顯示如何停用串流媒體：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)