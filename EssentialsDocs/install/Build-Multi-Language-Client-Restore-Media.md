---
title: 建立多語言用戶端還原媒體
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879869"
---
# <a name="build-multi-language-client-restore-media"></a>建立多語言用戶端還原媒體

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

> [!NOTE]
>  中所述，您必須先建立多語系 Windows 映像[逐步解說：多語系 Windows 映像建立](https://technet.microsoft.com/library/jj126995)新增 Windows Server Essentials 語言套件加入 install.wim 之前。  
  
 建立多語言伺服器安裝 DVD 時，將為 Server install.wim 安裝語言套件。 還原精靈的當地語系化資源將作為語言套件的一部分進行安裝。  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>若要建立多語言用戶端還原媒體  
  
1.  將 install.wim 掛接在 c:\mount 中，我們將 c:\mount\Program Files\Windows Server\bin\ClientRestore 資料夾稱為下列用戶端還原媒體 [RestoreMediaRoot] 的根目錄：  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  將用戶端還原 WIM 檔案掛接在 [RestoreMediaRoot]\Sources\Boot.wim 中 (對於 boot_x86.wim，也需要執行相同的步驟)。 命令行為：  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  透過執行以下命令將 WinPE-Setup.cab 套件新增至還原媒體：  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  使用記事本編輯 c:\mountRestore\windows\system32\winpeshl.ini，填寫下列內容：  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  將語言套件新增至還原媒體。 可執行下列命令來新增語言套件：  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     需要新增下列語言套件：  
  
    1.  WinPE 語言套件 (lp.cab)  
  
    2.  WinPE-Setup 語言套件 (WinPE-Setup_[lang].cab，例如，WinPE-Setup_en-us.cab)  
  
    3.  亞洲字型包括 zh-cn、zh-tw、zh-hk、ko-kr、ja-jp，並需要安裝其他字型套件 (winpe-fontsupport-[lang].cab，例如 winpe-fontsupport-zh-cn.cab)  
  
6.  透過執行下列命令產生新的 Lang.ini 檔案：  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  透過執行下列命令認可並取消掛接映像：  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  對 [RestoreMediaroot]\Sources\Boot_x86.wim 重複步驟 2 至步驟 7。  
  
9. 透過執行下列命令認可並取消掛接映像：  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>另請參閱  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](../install/Additional-Customizations.md)   
 [準備用於部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

