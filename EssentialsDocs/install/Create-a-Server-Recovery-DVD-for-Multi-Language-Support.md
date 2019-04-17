---
title: "建立伺服器復原 DVD 多語言的支援"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
4author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac547f97b48e4cd0ebf87e0935cadc2c539b4d0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>建立伺服器復原 DVD 多語言的支援

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_MLHeadedRecovery"></a>在本機管理的伺服器上建立的伺服器設定和伺服器修復多個語言支援 DVD  
  
> [!NOTE]
>  中所述，您必須先建立多種語言的 Windows 映像[逐步解說：多種語言的 Windows 映像建立](https://technet.microsoft.com/library/jj126995)您新增到 install.wim Windows Server Essentials langauage 套件之前。  
  
 有兩個階段的設定：Windows 預先安裝環境 (Windows PE\) 與初始設定。 根據預設，不會顯示在初始設定中的選取項目頁面語言。  
  
-   OEM 的遠端管理安裝或 OEM 預先安裝案例，您需要新增登錄按鍵使用下列命令以在初始設定中顯示語言選取項目頁面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  當 Oem 實驗室中建立映像時，必須選擇**英文**以安裝 Windows PE\ 階段的語言。  
  
-   經銷商選項套件 (ROK) 案例中，針對接收 DVD 和或許，某些硬體。 客戶應該可以在 [Windows PE\ 設定] 中選取語言，並在初始設定不會再顯示 [選擇語言] 頁面。  
  
 您也可以在出貨含有多種語言的單一 dual 層 DVD。  
  
 本章節告訴您如何將 Windows 設定中新增語言支援。 自訂 Windows PE\ 3.0 工具主要是部署映像服務與管理 (DISM)，此命令列工具。 此方案可讓如下：  
  
1.  建立多語系安裝  
  
2.  建立媒體散發  
  
### <a name="prerequisites"></a>必要條件  
 若要新增支援多種語言設定，您需要：  
  

-   提供的所有工具和來源檔案都需要建立自訂的 WinPE 映像的技術人員電腦。 如需詳細資訊，請查看[準備技術電腦](Prepare-the-Technician-Computer.md)。  

-   提供的所有工具和來源檔案都需要建立自訂的 WinPE 映像的技術人員電腦。 如需詳細資訊，請查看[準備技術電腦](../install/Prepare-the-Technician-Computer.md)。  

  
-   Windows Server Essentials DVD。  
  
-   Windows Server Essentials 語言套件 DVD。  
  
###  <a name="BKMK_Steps"></a>新增多個語言支援  
 若要將多種語言支援 Windows 安裝至您更新 Install.wim 加入 Windows Server 2012 和 Windows Server Essentials 的語言套件，以將它。  
  
#### <a name="update-installwim"></a>更新 Install.wim  
 在此步驟，您將 Windows Server 2012 和 Windows Server Essentials 的語言套件 Install.wim 加入。  
  
> [!NOTE]
>  請確認您的 Windows Server 2012 安裝語言套件。 這樣可確保您收到的適當的商標。 Windows Server 2012 多語系使用者介面語言套件可以使用[Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx)。中所述，請依照指示執行[逐步解說：上建立多語系多種語言的 Windows 映像建立](https://technet.microsoft.com/library/jj126995.aspx)上建立多語系 Windows 映像，才能您新增到 Windows Server Essentials 的語言套件 Install.wim。  
>   
>  Windows Server Essentials 的語言套件，可在語言套件，\Language Packs\\ < CultureName\ > 媒體。  
  
> [!NOTE]
>  並非所有的語言套件可能無法使用之前版本的 Windows Server 2012。  
  
###### <a name="to-add-language-packs-to-installwim"></a>若要加入 Install.wim 語言套件  
  
1.  [新增到 Install.wim 作業系統和 product 語言套件，如下所示（此範例中使用法文）：  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  [新增語言的特定檔案支援建立 Client 的備份還原 USB 快閃磁碟機，使用此程序中所述[還原媒體組建多語言 Client](Build-Multi-Language-Client-Restore-Media.md)。  

2.  [新增語言的特定檔案支援建立 Client 的備份還原 USB 快閃磁碟機，使用此程序中所述[還原媒體組建多語言 Client](../install/Build-Multi-Language-Client-Restore-Media.md)。  

  
3.  Lang.ini 中的檔案鬆散以反映的其他語言支援使用媒體重新建立`DISM /Gen-LangINI`命令，例如：  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  儲存變更回映像使用`DISM /unmount /commit`命令，例如：  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
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

