---
title: "安裝和 Windows Server Essentials 的設定"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cdad118c30fbf303b55ec7ea25bbe3e209c016db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="install-and-configure-windows-server-essentials"></a>安裝和 Windows Server Essentials 的設定

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_InstallConfigure"></a>   

 本文件會提供安裝及設定 Windows Server Essentials 的逐步指示。 在您開始安裝之前，檢視和完成工作中所述[之前您安裝 Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md)。  

 本文件會提供安裝及設定 Windows Server Essentials 的逐步指示。 在您開始安裝之前，檢視和完成工作中所述[之前您安裝 Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md)。  
  
 安裝與設定 Windows Server Essentials 兩個步驟：  
  

1.  [步驟 1： 安裝 Windows Server Essentials 的作業系統](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation)在此步驟，您必須安裝作業系統伺服器上。  
  
2.  [步驟 2： 設定 Windows Server Essentials 作業系統](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure)在此步驟，完成安裝提供您的公司、 網域設定和的網路系統管理員的相關資訊。 這項資訊用來讓您可以使用準備伺服器。  

1.  [步驟 1： 安裝 Windows Server Essentials 的作業系統](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation)在此步驟，您必須安裝作業系統伺服器上。  
  
2.  [步驟 2： 設定 Windows Server Essentials 作業系統](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure)在此步驟，完成安裝提供您的公司、 網域設定和的網路系統管理員的相關資訊。 這項資訊用來讓您可以使用準備伺服器。  

  
###  <a name="BKMK_ManualInstallation"></a>步驟 1： 安裝 Windows Server Essentials 的作業系統  
  
> [!IMPORTANT]
>  作業系統已安裝之後，不要自訂您的伺服器直到您完成[步驟 2： 設定 Windows Server Essentials 作業系統](#BKMK_Step2Configure)。  
  
 **估計的完成度時間：**約 30 分鐘的時間。  
  
> [!NOTE]
>  此程序完成預估的時間為基礎最低的硬體需求。  
  
##### <a name="to-install-the-operating-system"></a>若要安裝的作業系統  
  
1.  將您的電腦連接到您的網路電纜的網路。  
  
    > [!IMPORTANT]
    >  從網路中斷您的電腦，在安裝期間。 如此一來，可能會導致安裝失敗。  
  
2.  將您的電腦上與 Windows Server Essentials DVD 插入光碟機。  
  
     如果您執行的自動的安裝，連接包含您的回應檔案卸除式媒體 （例如磁片或 USB 快閃磁碟機）。 根據到您的回應檔案，您可能不會看到一些或任何下列安裝畫面。  
  
3.  您的電腦重新開機。 當訊息**請按任意鍵從 CD 或 DVD 開機**出現時，按任意鍵。  
  
    > [!NOTE]
    >  如果您的電腦不會從 DVD 開始，請確定 CD-ROM 光碟機，會先列出 BIOS 開機順序。 如需 BIOS 開機順序，查看從電腦製造商的文件。  
  
4.  選取**語言**您想要安裝時，**時間及貨幣格式**，並**鍵盤或輸入的法**，，然後按一下**下一步**。  
  
5.  按一下**立即安裝]**。  
  
6.  在**輸入 product 金鑰**，輸入 product 金鑰。  
  
7.  朗讀**授權條款**。 若接受這些條款，選取 [**我接受授權條款**核取方塊，並再按**下**。  
  
    > [!NOTE]
    >  如果您無法選擇接受授權條款，安裝不會繼續。  
  
8.  在**您要哪一種安裝類型？**，按一下 [**自訂： 只安裝 Windows （進階）**  
  
9. 在**您要安裝 Windows 何處？**，選取您想要安裝的 Windows 作業系統的硬碟機。 請確認內部硬碟的所有可安裝。  
  
    > [!IMPORTANT]
    >   Windows Server Essentials 必須先安裝為 c：磁碟區，並磁碟區大小必須至少 60 GB。 建議您兩個磁碟分割磁碟上建立您的作業系統，並使用 c:（系統磁碟分割）來儲存的任何企業資料。  
  
    > [!NOTE]
    >  如果未列出的硬碟機 （例如序列進階技術附件 (SATA) 硬碟），您必須載入硬碟的裝置驅動程式。 從製造商取得裝置驅動程式，並將它儲存到抽取式媒體 （例如磁片或 USB 快閃磁碟機）。 卸除式媒體連接到您的電腦，然後按一下**載入驅動程式**。  
  
     如果您需要 delete 及/或建立磁碟分割，請參考下列步驟：  
  
    1.  若要 delete 磁碟分割，選取磁碟分割，按一下 [**磁碟機選項 （進階），**，然後按一下 [ **Delete**。 您 delete 系統磁碟分割之後，建立新的磁碟分割中任一個步驟的指示**b**或 [逐步**c**。  
  
        > [!NOTE]
        >  之後您可以按一下**磁碟機選項 （進階），**，該選項不會再試一次。 若是如此，略過磁碟機的選項是指步驟的一部分。  
  
    2.  建立磁碟分割的分割的磁碟空間，請按一下硬碟磁碟分割，請按一下您想要**磁碟機選項 （進階），**，按一下 [**新**，然後在**大小**文字方塊中，輸入您想要建立磁碟分割大小。 例如，如果您使用的 120 gb 建議的磁碟分割大小，輸入**122880**，然後按**套用**。 建立磁碟分割之後，請按一下**下一步**。 持續在安裝前已格式化磁碟分割。  
  
    3.  建立使用的所有分割空間的磁碟分割，請按一下硬碟磁碟分割，請按一下您想要**磁碟機選項 （進階），**，按一下**新**，然後按一下 [**套用**接受預設的磁碟分割大小。 建立磁碟分割之後，請按一下**下一步**。 持續在安裝前已格式化磁碟分割。  
  
        > [!IMPORTANT]
        >  在您完成此步驟後，您無法作業系統移動到不同的磁碟分割。  
  
 安裝期間，暫存檔案複製到您的電腦、 電腦需要約 30 分鐘的時間安裝資料夾。 Windows Server Essentials 的作業系統已安裝之後，您的電腦。 現在，您已經準備好設定 Windows Server Essentials 的作業系統。  
  
###  <a name="BKMK_Step2Configure"></a>步驟 2： 設定 Windows Server Essentials 的作業系統  
  
> [!IMPORTANT]
>  如果您從先前的版本的 Windows 小型企業 Server 移轉 Windows Server essentials，您必須遵循其他處理程序。 有關移轉安裝的方法如下：  
>   
>  -   [從 Windows SBS 2003 移轉](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
> -   [從 Windows SBS 2008 移轉](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 安裝這個階段，會提示您將會解答一些關於您的組織。 這項資訊用來設定作業系統。  
  
> [!IMPORTANT]
>  在開始之前此步驟，請確定區域網路介面卡是連接到路由器或會亮起來切換且正常運作。  
  
 **估計的完成度時間：**約 30 分鐘  
  
##### <a name="to-configure-the-operating-system"></a>若要設定的作業系統  
  
1.  在**驗證日期和時間設定**頁面上，按**變更系統的日期和時間設定**以選取您的伺服器的日期、 時間和時區設定。 當您完成時，請按**下一步**。  
  
    > [!IMPORTANT]
    >  如果您要安裝 Windows Server Essentials 上一樣，請確定您選擇相同的時區設定主機作業系統來使用。 如果時區設定不同，伺服器可能繼續安裝。  
  
2.  在**選擇伺服器安裝模式**頁面上，執行下列其中一個動作：  
  
    1.  選擇 [**全新安裝**來設定 Windows Server Essentials 伺服器軟體的完整的全新安裝。  
  
    2.  選擇 [**伺服器移轉**來安裝 Windows Server Essentials 和此伺服器加入現有的 Windows 網域。  
  
3.  在**個人化您的伺服器**頁面上，輸入您的組織的名稱、 內部網域名稱和伺服器名稱。  
  
    > [!IMPORTANT]
    >  伺服器名稱必須是唯一您網路上的名稱。 在您完成此步驟後，您無法變更伺服器名稱或內部網域名稱。  
  
4.  按一下**下一步**。  
  
5.  在**系統管理員帳號資訊提供**頁面上，輸入新的系統管理員 account 的資訊。  
  
    > [!CAUTION]
    >  不為的網路管理員系統管理員或的網路系統管理員。 這些 account 名稱是由系統保留使用。  
  
6.  在**提供您標準使用者 account 資訊**頁面中，輸入新的標準使用者帳號的資訊，然後按一下 [**下**。  
  
7.  在**自動保留您的伺服器最**頁面上，選取您要接收到您的伺服器，Windows 更新，然後按一下 [**下**。  
  
8.  **更新及準備您的伺服器**頁面會顯示在最後的安裝程序的進度。 這需要時間來完成，以及您的電腦將會重新開機幾次。  
  
9. 最後一個伺服器後重新開機，**伺服器可用於**頁面隨即顯示。 按一下**關閉**。  
  
10. 按一下磚儀表板上**[開始]**畫面，並儀表板，完成**設定好我的伺服器**上工作**Home**頁面。 您的 Windows Server Essentials 安裝完成之後，您應該完成這些工作。  
  
> [!NOTE]
>  安裝完成後，您會自動登入至您在安裝期間新增新的系統管理員 account 的伺服器。 建系統管理員密碼為為新的系統管理員帳號，相同的密碼，然後建已停用。  
  
 您可以使用您最近安裝的伺服器的有限的時間 （稱為評估期間），而不輸入 product 按鍵。 評估期間之後，您必須輸入啟動伺服器或擴充評估期間 product 按鍵。 您可以擴充評估期間最多的兩次。 當您到達數上限允許評估期間時，您必須使用 product 金鑰啟動伺服器。  
  
### <a name="customize-windows-server-essentials"></a>自訂 Windows Server Essentials  
 **Home** Windows Server Essentials 儀表板中的頁面連結到**設定**工作，您必須先完成之後立即安裝您的伺服器。 藉由在執行這些工作，您可以協助保護儲存在伺服器的資訊，以及可在 Windows Server Essentials 的功能。  
  
 如果您選擇不要執行的工作，使用者不可能有一些網路功能的存取權。 若要稍後再返回這些工作，返回 [Windows Server Essentials 儀表板**Home**頁面。  
  
 下表定義項目可能會出現在清單中安裝工作。  
  
|工作|描述
|----------|-----------------|  
|取得適用於其他 Microsoft 你更新|按一下此任務存取連結執行此工具可讓您指定您要使用 Microsoft Update 自動取得適用於 Windows Server Essentials 和其他 Microsoft 你，例如 Office 的更新。  
|新增帳號|按一下 [檢視簡短資訊新增帳號這項工作。 連結執行**[新增使用者 Account 精靈**提供。 如需詳細資訊，請查看[[新增使用者 account](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。  
|新增資料夾伺服器|按一下 [檢視簡短資訊新增伺服器資料夾中，這項工作。 連結執行**[新增資料夾精靈**提供。 也提供是有關使用伺服器資料夾 online 協助主題的連結。 如需詳細資訊，請查看[新增或移動伺服器資料夾](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。 
|設定伺服器的備份|按一下此工作檢視簡短的資訊，有關使用伺服器備份，保護您的資料。 連結執行**設定伺服器備份精靈**提供。 如需詳細資訊，請查看[設定或自訂伺服器備份](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)。 
|隨處存取設定|按一下 [檢視簡短的資訊，有關的任何地方存取 」 功能在 Windows Server Essentials 這項工作。 連結，以**任何地方存取設定**頁面上提供。 如需詳細資訊，請查看[管理隨處存取](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)。 
|設定電子郵件通知|按一下此工作檢視簡短的電子郵件通知的詳細資訊。 連結執行**設定電子郵件通知的警示**提供工具。 如需詳細資訊，請查看[設定電子郵件通知的警示](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)。  
|媒體伺服器設定|按一下 [檢視簡短的資訊來共用音樂、 影片，以及影像檔案使用媒體伺服器此工作。 連結，以**媒體設定**頁面上提供。 也提供是以深入了解媒體伺服器 online 協助主題的連結。 如需詳細資訊，請查看[管理數位媒體](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)。 
|連接的電腦|按一下 [檢視簡短的資訊，了解如何將網路的電腦連接到伺服器這項工作。 如需詳細資訊，請查看[電腦連接到伺服器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。
  
## <a name="see-also"></a>也了  
  
-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)

