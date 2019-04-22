---
title: 變更媒體串流設定
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823399"
---
# <a name="change-media-streaming-settings"></a>變更媒體串流設定

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

有多個選項可供您變更媒體串流設定。 可用的選項如下：  
  
-   [隱藏遠端媒體串流增益集](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [設定媒體櫃名稱](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [設定視訊串流品質](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [以程式設計方式啟用或停用媒體串流處理](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a> 隱藏遠端媒體串流增益集  
 您可以在登錄中新增項目，以隱藏遠端媒體串流。  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>若要隱藏遠端媒體串流增益集  
  
1.  在伺服器上，將滑鼠移至畫面的右上角，然後按一下 **[搜尋]**。  
  
2.  在 [搜尋] 方塊中，輸入 **regedit**，然後按一下 [Regedit] 應用程式。  
  
3.  在左窗格中，往下展開至下列登錄項目：  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  在 **DisabledAddIns**上按一下滑鼠右鍵，並指向 **[新增]**，然後按一下 **[DWORD 值]**。  
  
5.  新值輸入 **497796c6-9cc7-43be-aa26-4e6b5695370d**，也就是遠端媒體串流處理增益集的識別碼，然後按 **Enter**。  
  
6.  在值上按一下滑鼠右鍵，然後按一下 **[修改]**。  
  
7.  針對數值資料輸入 **1**，然後按一下 **[確定]**。  
  
##  <a name="BKMK_LibraryName"></a> 設定媒體櫃名稱  
 您可以使用 Windows Server 解決方案 SDK 中的類別來設定媒體櫃的名稱。 若要設定媒體櫃的名稱，您可以使用 **Microsoft.WindowsServerSolutions.MediaStreaming** 命名空間 **MediaStreamingManager** 類別的 **SetMediaLibraryName** 方法。 下列範例顯示如何設定媒體櫃的名稱：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 如需詳細資訊，請參閱 [Windows Server 解決方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
##  <a name="BKMK_StreamingQuality"></a> 設定視訊串流品質  
 取得 WinSAT CPU 分數，然後建立並安裝含有 WinSAT 分數資訊的 .xml 檔案，即可設定視訊串流品質。 如果含有 WinSAT 分數資訊的 .xml 檔案是在初始設定執行之前所安裝，則客戶看不到用來設定視訊品質的使用者介面。 如需詳細資訊，請參閱 [Set the WinSAT Score on the Server](Set-the-WinSAT-Score-on-the-Server.md)。  
  
##  <a name="BKMK_Program"></a> 以程式設計方式啟用或停用媒體串流處理  
 您可以使用 Windows Server 解決方案 SDK 中的類別，以程式設計方式啟用或停用媒體串流。 若要啟用或停用媒體串流，您可以使用 **Microsoft.WindowsServerSolutions.MediaStreaming** 命名空間 **MediaStreamingManager** 類別中的 **SetMediaStreamingEnabled** 方法。 下列程式碼範例顯示如何啟用媒體串流：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 下列程式碼範例顯示如何停用媒體串流：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)