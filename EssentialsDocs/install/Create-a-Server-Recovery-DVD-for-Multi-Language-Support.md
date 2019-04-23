---
title: 針對多語言支援建立伺服器復原 DVD
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854999"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>針對多語言支援建立伺服器復原 DVD

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a> 本機管理的伺服器上建立的伺服器安裝程式和多語言支援的伺服器復原 DVD  
  
> [!NOTE]
>  中所述，您必須先建立多語系 Windows 映像[逐步解說：多語系 Windows 映像建立](https://technet.microsoft.com/library/jj126995)新增 Windows Server Essentials 語言套件加入 install.wim 之前。  
  
 安裝有兩個階段：Windows 預先安裝環境 (Windows PE) 和初始設定。 根據預設，將不會在初始設定中顯示語言選取頁面。  
  
-   對於 OEM 遠端管理的安裝或 OEM 預先安裝情況，您需要使用下列命令來新增登錄機碼，以便在初始設定中顯示語言選取頁面。  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  OEM 在實驗室中建立映像時，必須在 Windows PE 安裝階段期間選擇 **英文** 做為語言。  
  
-   針對經銷商選項套件 (ROK) 案例，客戶會收到 DVD，可能也會收到一些硬體。 客戶應該可以在 Windows PE 安裝期間選取語言，在初始設定期間不再顯示語言選取頁面。  
  
 您可以選擇提供包含多個語言的單一雙層 DVD。  
  
 本節說明如何為 Windows 安裝程式新增語言支援。 用來自訂 Windows PE 3.0 的主要工具為「部署映像服務與管理 (DISM)」，它是命令列工具。 此解決方案適用於下列情況：  
  
1.  建立多語言安裝  
  
2.  建立可散佈的媒體  
  
### <a name="prerequisites"></a>先決條件  
 若要為 Windows 安裝程式新增多語言支援，您將需要下列項目：  
  

-   可提供建立自訂 WinPE 映像所需之所有工具與來源檔案的技術人員電腦。 如需詳細資訊，請參閱 [Prepare the Technician Computer](Prepare-the-Technician-Computer.md)。  

-   可提供建立自訂 WinPE 映像所需之所有工具與來源檔案的技術人員電腦。 如需詳細資訊，請參閱 [Prepare the Technician Computer](../install/Prepare-the-Technician-Computer.md)。  

  
-   Windows Server Essentials DVD。  
  
-   Windows Server Essentials 語言套件 DVD。  
  
###  <a name="BKMK_Steps"></a> 新增多語言支援  
 您可以將多種語言支援新增至 Windows 安裝程式更新 Install.wim 來加上 Windows Server 2012 和 Windows Server Essentials 語言套件新增至它。  
  
#### <a name="update-installwim"></a>更新 Install.wim  
 在此步驟中，您將 Windows Server 2012 和 Windows Server Essentials 語言套件加入 Install.wim。  
  
> [!NOTE]
>  請確認您在 Windows Server 2012 的安裝語言套件。 如此可確保您會有適當的商標。 Windows Server 2012 多語系使用者介面語言套件，還有[Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx)。 請遵循指示中所述[逐步解說：建立多國語言的多語系 Windows 映像建立](https://technet.microsoft.com/library/jj126995.aspx)您新增 Windows Server Essentials 語言套件加入 install.wim 之前，建立多國語言 Windows 映像。  
>   
>  Windows Server Essentials 語言套件可用於在 \Language 組件語言套件媒體\\< CultureName\>。  
  
> [!NOTE]
>  並非所有語言組件可能無法使用之前的 Windows Server 2012 版本。  
  
###### <a name="to-add-language-packs-to-installwim"></a>若要將語言套件新增至 Install.wim  
  
1.  將作業系統及產品語言套件新增至 Install.wim，如下所示 (此範例使用法文)：  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  新增語言特定的檔案，以支援建立用戶端備份還原 USB 快閃磁碟機，使用程序中所述[建置多語言用戶端還原媒體](Build-Multi-Language-Client-Restore-Media.md)。  

2.  新增語言特定的檔案，以支援建立用戶端備份還原 USB 快閃磁碟機，使用程序中所述[建置多語言用戶端還原媒體](../install/Build-Multi-Language-Client-Restore-Media.md)。  

  
3.  使用 `DISM /Gen-LangINI` 命令，在可攜式儲存媒體中重新建立 Lang.ini 檔案，以反映其他語言支援，例如：  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  使用 `DISM /unmount /commit` 命令，將您的變更存回映像中，例如：  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
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

