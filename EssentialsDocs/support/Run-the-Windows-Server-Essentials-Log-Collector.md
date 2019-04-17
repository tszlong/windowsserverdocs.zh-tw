---
title: "執行 Windows Server Essentials 登入行程"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 6b49fee7ca4a19d5a501cf96c1ce356f8242c81f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="run-the-windows-server-essentials-log-collector"></a>執行 Windows Server Essentials 登入行程
您可以在網路上伺服器或電腦的執行 Windows Server Essentials 登入行程。 如果您在登入行程執行伺服器，您可以僅限收集登伺服器。 如果您從網路的電腦執行的登入行程，您可以選擇收集登的伺服器，除了登該電腦。  
  
 您必須執行登入行程適當的系統管理員權限。 如果您收集登入檔案伺服器，您必須由伺服器管理員。如果您收集登入電腦上的檔案網路，您必須是 Client 管理員該電腦。  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>若要使用精靈伺服器上執行，登入行程  
  
1.  在**[開始]**頁面上的伺服器上，按一下 [ **Windows Server Essentials 登入行程**。  
  
    > [!NOTE]
    >  -   如果登入行程程式未顯示在**[開始]**頁面中，瀏覽至**%system%\Program 檔案 (x86) \Windows Server Essentials 登入行程**，然後按兩下 [ **LogCollector**。  
    > -   如果您無法登入以系統管理員權限的伺服器，登入行程會提示您輸入您的認證。  
  
2.  當系統提示您收集登入，檔案儲存的位置時，您可以選擇預設位置，**\\\ < ServerName\ > \logs**，或指定另一個位置。 若要接受預設的位置，請按一下**下一步**。 若要變更位置，請按一下**瀏覽]**，瀏覽至您想要登入檔案，儲存的資料夾，然後按一下 [**儲存**。  
  
    > [!NOTE]
    >  您不需要提供登入檔案的檔名。 登入行程名稱 zip 檔案集合串連電腦名稱，且該檔案的頻率。  
  
3.  會收集登時，會顯示進度列。  
  
4.  若要檢視到收集登入檔案，請選取 [**開放登已儲存的檔案位置**核取方塊，然後按**關閉**以關閉精靈，並左登入收集檔案。  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>若要使用精靈網路的電腦上執行，登入行程  
  
1.  瀏覽] **%system%\Program 檔案 (x86) \Windows Server Essentials 登入行程**，然後按兩下檔案**LogCollector.exe**。  
  
    > [!NOTE]
    >  如果您無法登入的網路系統管理員權限的電腦，請輸入您的使用者名稱和密碼提示時，請然後按一下**下一步**。  
  
2.  選取您想要如下所示會收集哪些登：  
  
    1.  選取 [**伺服器的檔案登入**核取方塊以收集登入伺服器上的檔案。  
  
    2.  **（電腦）用電腦登入檔案**核取方塊已選取預設指示登入行程，會收集登從網路電腦正在執行。 如果您只想要收集伺服器登，清除**（電腦）用電腦登入檔案**核取方塊。  
  
    3.  按一下**下一步**。  
  
3.  出現提示時的 [由伺服器管理員中，輸入的使用者名稱和密碼，然後按一下 [**下一步**。  
  
4.  輸入或瀏覽至您想要儲存的登入檔案，然後再按一下位置**下一步**。  
  
    > [!NOTE]
    >  您不需要提供登入檔案的檔名。 登入行程名稱 zip 檔案集合串連電腦名稱，且該檔案的頻率。  
  
5.  會收集登時，會顯示進度列。  
  
6.  若要檢視到收集登入檔案，請選取 [**開放登已儲存的檔案位置**核取方塊，然後按**關閉**以關閉精靈，並左登入收集檔案。  
  
### <a name="running-the-log-collector-manually"></a>手動執行登入行程  
 登入行程安裝之後，才能執行該工具建立排程的工作。 接下來，您可以執行從登入行程**排程工作管理員]**不用精靈中，如果有問題的起始精靈。  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>手動執行伺服器上的 [在登入行程  
  
1.  登入直接或從遠端伺服器。  
  
2.  開放**工作排程器**。  
  
3.  在 [根的**工作排程器程式庫**，瀏覽至排定的工作名為**LogCollector**。  
  
4.  以滑鼠右鍵按一下**LogCollector**，然後按**執行**。 登入行程地點登預設伺服器上的資料夾，在**\\\ < ServerName\ > \Logs**。 如果您尚未寫入權限的資料夾，或資料夾不存在，請登位於**< temp\ >**子目錄。  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>若要手動網路的電腦上執行，登入行程  
  
1.  登入直接或遠端電腦的網路。  
  
2.  開放**工作排程器**。  
  
3.  在 [根的**工作排程器程式庫**，瀏覽至排定的工作名為**LogCollector**。  
  
4.  以滑鼠右鍵按一下**LogCollector**，然後按**執行**。 登入行程地點登入**< temp\ >**在網路的電腦上的資料夾。
