---
title: "組建多語言 Client 還原媒體"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ad934d297c3092050bd6adbb6bb0f50d1ec6f36
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="build-multi-language-client-restore-media"></a>組建多語言 Client 還原媒體

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

> [!NOTE]
>  中所述，您必須先建立多種語言的 Windows 映像[逐步解說：多種語言的 Windows 映像建立](https://technet.microsoft.com/library/jj126995)您新增到 install.wim Windows Server Essentials langauage 套件之前。  
  
 當建置多語言伺服器安裝 DVD，將會的伺服器 install.wim 安裝語言套件。 還原精靈的當地語系化的資源將會安裝語言套件的一部分。  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>若要還原媒體建置多語言 client  
  
1.  雷 install.wim c:\mount，在撥號 c:\mount\Program Files\Windows Server\bin\ClientRestore 資料夾作為 client 還原媒體根: [RestoreMediaRoot] 如下：  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  雷 client 還原 WIM 檔案，在 [RestoreMediaRoot]\Sources\Boot.wim（相同的步驟需要太執行 boot_x86.wim）。 命令列是：  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  新增至還原媒體 WinPE-Setup.cab 套件執行：  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  若要編輯 c:\mountRestore\windows\system32\winpeshl.ini，使用下列 content 填滿使用「記事本」:  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  若要還原媒體新增語言套件。 新增語言套件可透過執行下列命令：  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     下列語言套件，就必須先加入：  
  
    1.  WinPE 語言套件 (lp.cab)  
  
    2.  WinPE-安裝語言套件 (WinPE Setup_ [語言].cab，例如 WinPE-Setup_en-us.cab)  
  
    3.  必須安裝額外字型套件 Asian 字型，包括曆法-data-cn、曆法-新、曆法-香港、ko-kr、ja-jp，(winpe-fontsupport-[語言].cab，例如 winpe-fontsupport-zh-cn.cab)  
  
6.  執行產生新 Lang.ini 檔案中：  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  確認並解映像下執行,：  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  重複步驟 2 執行「步驟 7 [RestoreMediaroot]\Sources\Boot_x86.wim。  
  
9. 確認並解映像下執行,：  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>也了  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

