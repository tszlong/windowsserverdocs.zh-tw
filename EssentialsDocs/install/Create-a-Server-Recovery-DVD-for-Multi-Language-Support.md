---
title: 針對多語言支援建立伺服器復原 DVD
description: 瞭解如何在本機管理的伺服器上建立伺服器安裝和伺服器復原 DVD，以提供多種語言支援。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
author: daveba
ms.author: daveba
ms.openlocfilehash: d17a7103233b25553eb67a2c7d025d4647cbbbc5
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711323"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>針對多語言支援建立伺服器復原 DVD

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="create-a-server-setup-and-server-recovery-dvd-for-multiple-language-support-on-locally-administered-servers"></a><a name="BKMK_MLHeadedRecovery"></a> 在本機管理的伺服器上建立伺服器安裝和伺服器復原 DVD，以提供多種語言支援

> [!NOTE]
>  您必須先建立多語系 Windows 映像，如逐步解說：在將 Windows Server Essentials 語言 pack 新增至 install 之前，建立 [多語系 Windows 映像](/previous-versions/windows/it-pro/windows-8.1-and-8/jj126995(v=win.10)) 所述。

 安裝有兩個階段：Windows 預先安裝環境 (Windows PE) 和初始設定。 根據預設，將不會在初始設定中顯示語言選取頁面。

- 對於 OEM 遠端管理的安裝或 OEM 預先安裝情況，您需要使用下列命令來新增登錄機碼，以便在初始設定中顯示語言選取頁面。

  ```
  %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f
  ```

  > [!IMPORTANT]
  >  OEM 在實驗室中建立映像時，必須在 Windows PE 安裝階段期間選擇 **英文** 做為語言。

- 針對經銷商選項套件 (ROK) 案例，客戶會收到 DVD，可能也會收到一些硬體。 客戶應該可以在 Windows PE 安裝期間選取語言，在初始設定期間不再顯示語言選取頁面。

  您可以選擇提供包含多個語言的單一雙層 DVD。

  本節說明如何為 Windows 安裝程式新增語言支援。 用來自訂 Windows PE 3.0 的主要工具為「部署映像服務與管理 (DISM)」，它是命令列工具。 此解決方案適用於下列情況：

1.  建立多語言安裝

2.  建立可散佈的媒體

### <a name="prerequisites"></a>必要條件
 若要為 Windows 安裝程式新增多語言支援，您將需要下列項目：


-   可提供建立自訂 WinPE 映像所需之所有工具與來源檔案的技術人員電腦。 如需詳細資訊，請參閱[準備技術人員電腦](Prepare-the-Technician-Computer.md)。

-   可提供建立自訂 WinPE 映像所需之所有工具與來源檔案的技術人員電腦。 如需詳細資訊，請參閱[準備技術人員電腦](../install/Prepare-the-Technician-Computer.md)。


-   Windows Server Essentials DVD。

-   Windows Server Essentials 語言套件 DVD。

###  <a name="adding-multiple-language-support"></a><a name="BKMK_Steps"></a> 新增多個語言支援
 若要將多個語言支援新增至 Windows 安裝程式您必須將 Windows Server 2012 和 Windows Server Essentials 語言套件新增至其中，以更新 .wim。

#### <a name="update-installwim"></a>更新 Install.wim
 在此步驟中，您會將 Windows Server 2012 和 Windows Server Essentials 語言套件新增至安裝 .wim。

> [!NOTE]
>  確認您已安裝適用于 Windows Server 2012 的語言套件。 如此可確保您會有適當的商標。 Windows Server 2012 多語系消費者介面語言套件可在 [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx)上取得。 請依照逐步解說 [：在](/previous-versions/windows/it-pro/windows-8.1-and-8/jj126995(v=win.10)) 建立多語系 windows 映射建立多語系 windows 映射建立多語系 windows 映射，然後將 Windows Server Essentials 語言套件新增至安裝 .wim 中所述的指示進行。
>
>  Windows Server Essentials 語言套件適用于 \Language Pack 的語言套件媒體 \\<CultureName \> 。

> [!NOTE]
>  在 Windows Server 2012 發行之前，並非所有語言套件都可供使用。

###### <a name="to-add-language-packs-to-installwim"></a>若要將語言套件新增至 Install.wim

1.  將作業系統及產品語言套件新增至 Install.wim，如下所示 (此範例使用法文)：

    ```
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount
    Mkdir c:\temp_Scratch
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab

    ```


2.  使用「 [建立多語言用戶端還原媒體](Build-Multi-Language-Client-Restore-Media.md)」中所述的程式，新增語言特定檔案以支援建立用戶端備份還原 USB 快閃磁片磁碟機。

2.  使用「 [建立多語言用戶端還原媒體](../install/Build-Multi-Language-Client-Restore-Media.md)」中所述的程式，新增語言特定檔案以支援建立用戶端備份還原 USB 快閃磁片磁碟機。


3.  使用 `DISM /Gen-LangINI` 命令，在可攜式儲存媒體中重新建立 Lang.ini 檔案，以反映其他語言支援，例如：

    ```
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution

    ```

4.  使用 `DISM /unmount /commit` 命令，將您的變更存回映像中，例如：

    ```
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit
    ```

## <a name="see-also"></a>另請參閱

 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)