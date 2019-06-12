---
title: 執行 Windows Server Essentials 記錄檔收集器
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5654f28aeda3c231376ed888a8aa04bc0cf3d000
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432497"
---
# <a name="run-the-windows-server-essentials-log-collector"></a>執行 Windows Server Essentials 記錄檔收集器
您可以在網路上，從伺服器或電腦執行 Windows Server Essentials Log Collector。 如果從伺服器執行記錄檔收集器，您只能收集伺服器中的記錄檔。 如果從網路電腦執行記錄檔收集器，則除了該電腦的記錄檔以外，您還可選擇收集伺服器中的記錄檔。  
  
 您必須有適當的管理權限才能執行記錄檔收集器。 如果您要收集伺服器的記錄檔，您必須是伺服器系統管理員；如果您要收集網路電腦上的記錄檔，您必須是該電腦的用戶端系統管理員。  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>使用精靈在伺服器上執行記錄檔收集器  
  
1. 在上**開始**頁面上的伺服器上，按一下**Windows Server Essentials Log Collector**。  
  
   > [!NOTE]
   > - 如果記錄檔收集器程式未出現在**開始**頁面上，瀏覽至 **%system%\Program 檔案 (x86) \Windows Server Essentials Log Collector**，然後按兩下**LogCollector**.  
   >   -   如果您不是以具有系統管理權限的身分登入伺服器，記錄檔收集器會提示您輸入您的認證。  
  
2. 當系統提示您儲存所收集的記錄檔的位置時，您可以選擇預設位置，  **\\ \\< 伺服器名稱\>\logs**，或是指定其他位置。 如果接受預設位置，請按一下 [下一步]  。 如果要變更位置，請按一下 [瀏覽]  ，瀏覽至您要儲存記錄檔的資料夾，然後按一下 [儲存]  。  
  
   > [!NOTE]
   >  您不需提供記錄檔的檔案名稱。 記錄收集器會藉由串連的電腦名稱和檔案的時間戳記命名 zip 檔案集合。  
  
3. 收集記錄檔時，會顯示一個進度列。  
  
4. 如果要檢視記錄檔集合檔案的內容，請選取 [開啟儲存記錄檔的檔案位置]  核取方塊，然後按一下 [關閉]  來關閉精靈並開啟記錄檔集合檔案。  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>使用精靈在網路電腦上執行記錄檔收集器  
  
1.  瀏覽至 **%system%\Program 檔案 (x86) \Windows Server Essentials Log Collector**，然後按兩下檔案**LogCollector.exe**。  
  
    > [!NOTE]
    >  如果您不是以具有系統管理權限的身分登入網路電腦，當系統提示時，請輸入您的使用者名稱和密碼，然後按 [下一步]  。  
  
2.  選取您想要收集的記錄檔，如下所示：  
  
    1.  選取 [伺服器記錄檔]  核取方塊可收集伺服器上的記錄檔。  
  
    2.  預設會選取 [用戶端電腦記錄檔 (這台電腦)]  核取方塊，表示記錄檔收集器會從執行的網路電腦收集記錄檔。 如果您想收集伺服器記錄檔，請清除 [用戶端電腦記錄檔 (這台電腦)]  核取方塊。  
  
    3.  按一下 [下一步]  。  
  
3.  出現提示時，輸入伺服器系統管理員的使用者名稱和密碼，然後按一下 [下一步]  。  
  
4.  輸入或瀏覽至您要儲存記錄檔的位置，然後按一下 [下一步]  。  
  
    > [!NOTE]
    >  您不需提供記錄檔的檔案名稱。 記錄收集器會藉由串連的電腦名稱和檔案的時間戳記命名 zip 檔案集合。  
  
5.  收集記錄檔時，會顯示一個進度列。  
  
6.  如果要檢視記錄檔集合檔案的內容，請選取 [開啟儲存記錄檔的檔案位置]  核取方塊，然後按一下 [關閉]  來關閉精靈並開啟記錄檔集合檔案。  
  
### <a name="running-the-log-collector-manually"></a>手動執行記錄檔收集器  
 安裝記錄檔收集器之後，會建立一個排定的工作以執行工具。 如果啟動精靈發生問題，您之後就可從 [排定的工作管理員]  執行記錄檔收集器而不需使用精靈。  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>在伺服器上手動執行記錄檔收集器  
  
1.  以直接或遠端方式登入伺服器。  
  
2.  開啟 [工作排程器]  。  
  
3.  在 [工作排程器程式庫]  的根目錄中，瀏覽至名為 [LogCollector]  的排程工作。  
  
4.  以滑鼠右鍵按一下 [LogCollector]  ，然後按一下 [執行]  。 記錄收集器會記錄檔放在預設資料夾的伺服器上，  **\\ \\< 伺服器名稱\>\Logs**。 如果您沒有在資料夾的寫入權限或資料夾不存在，將記錄檔放在 **< temp\>** 子目錄。  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>在網路電腦上手動執行記錄檔收集器  
  
1.  以直接或遠端方式登入網路電腦。  
  
2.  開啟 [工作排程器]  。  
  
3.  在 [工作排程器程式庫]  的根目錄中，瀏覽至名為 [LogCollector]  的排程工作。  
  
4.  以滑鼠右鍵按一下 [LogCollector]  ，然後按一下 [執行]  。 記錄收集器會放在記錄檔 **< temp\>** 網路電腦上的資料夾。
