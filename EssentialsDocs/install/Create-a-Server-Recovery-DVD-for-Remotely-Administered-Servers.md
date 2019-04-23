---
title: 針對遠端管理的伺服器建立伺服器復原 DVD
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7bfe1686ac84962cdb4ab1cde8d6ca5226cb9d44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844349"
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>針對遠端管理的伺服器建立伺服器復原 DVD

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_HeadlessRecovery"></a> 建立伺服器復原 DVD 的遠端管理的伺服器  
 原廠重設及伺服器復原有兩個模式，兩者會隨客戶收到的硬體而有所不同。  
  
 **從遠端管理的伺服器**  
  
 此伺服器復原選項需要從相同網路的用戶端電腦執行程序。 由於原廠重設需要硬體特定映像隨附於伺服器，合作夥伴必須編寫伺服器復原 DVD。  
  
 **在本機管理的伺服器**  
  
 這是在伺服器主控台中管理伺服器的傳統模式。 伺服器安裝媒體是用來執行復原。 這需要伺服器除了包含 DVD 讀取程式之外，還需要具有檢視視訊輸出的功能。 客戶會從該伺服器安裝媒體開機，然後選擇適當的復原方法。 您不需要針對本機管理的伺服器建立伺服器復原 DVD。  
  
> [!NOTE]
>  針對本機管理的伺服器模式，客戶可以藉由執行新安裝而完成原廠重設。 不過他們將會遺失合作夥伴自訂項目。  
  
 伺服器復原有兩個類型。  
  
 **恢復出廠預設值**  
  
 此復原會將伺服器回復成從原廠出貨時的原始狀態。 在進行原廠重設之後，系統會要求您執行與您第一次將伺服器開機時相同的初始設定，而所有的設定與自訂項目都會遺失。 這也稱為第 0 天。 嗎？ 由於原廠重設需要硬體特定映像隨附於伺服器，合作夥伴必須編寫伺服器復原 DVD。  
  
 **裸機還原**  
  
 此復原假設您已設定伺服器備份，且在伺服器故障之前至少順利完成過一次伺服器備份。 裸機還原 (BMR) 僅支援從先前的伺服器備份來復原系統與開機磁碟機。  
  
 在進行 BMR 之後，伺服器會回復成用於進行還原之備份當時的狀態。 這通常會是最新的備份，但在某些情況下也有可能是更早的備份。 使用「還原檔案與資料夾精靈」還原系統後，資料復原即完成。 BMR 是較偏好的伺服器復原方法，因為所有設定與組態都可回復，不像原廠重設只會將伺服器回復至「第 0 天」狀態。  
  
### <a name="remotely-administered-server-recovery"></a>遠端管理的伺服器復原  
 本節說明合作夥伴必須執行的自訂項目，以及必須隨附於每個伺服器的最終媒體。 在深入瞭解之前，讓我們看看客戶體驗。  
  
 在此案例中，客戶 」 狀況的伺服器不再運作。 這可能是由病毒、硬碟故障或某些其他原因所導致。 客戶在與伺服器位於相同網路的用戶端電腦上，放入伺服器復原 DVD。 伺服器復原應用程式會執行三個步驟，逐步復原伺服器：  
  
1.  建立可開機 USB 快閃磁碟機，用來將伺服器重新開機至復原模式。 此 USB 快閃磁碟機的大小不可低於 8 GB。  
  
2.  偵測目前處於復原模式的伺服器。  
  
3.  讓客戶選擇原廠重設或裸機還原，然後使其伺服器回復為運作的狀態。  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>針對多語言支援建立伺服器復原 DVD 的步驟  
 建立伺服器復原 DVD 有六個主要步驟  
  
1.  [（選擇性）更新 WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [收集原廠重設映像與 XML 檔案](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [建立伺服器復原 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [自訂精靈](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [建立 ISO 檔案](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [測試復原 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a> 步驟 1:(選用) 更新 WinPE  
 ADK 包含一份已自訂的 Windows PE。 當此映像開機時會自動啟動指標，用戶端復原應用程式會利用該指標，來連線至處於復原模式的伺服器。  
  
 您必須新增任何的硬體特定驅動程式 (如網路或磁碟控制卡驅動程式)，進一步自訂 Windows PE。 從 WinPE 開機後，必須可辨識系統上的硬碟，同時網路必須正常運作。  
  
####  <a name="BKMK_Collecting"></a> 步驟 2:收集原廠重設映像與 XML 檔案  
 若要將伺服器重設為原廠預設值，則必須擷取下列兩個映像：  
  
-   系統磁碟機映像  
  
-   系統保留磁碟分割  
  
 為了擷取這些映像，我們提供 GenDiskXML.exe 工具。 GenDiskXML.exe 也會收集復原程序用來重新建立磁碟設定且名為 disk.xml 的檔案。  
  
1.  在 Sysprep 之後，使用任何 64 位元版本的 Windows PE 將系統重新開機。  
  
2.  若要將 .xml 及 .wim 檔案輸出至外部來源，請執行 `GenDiskXML /outputdir:<path>` 將 .xml 及 .wim 檔案輸出至任何外部來源。 檔案會在下一個步驟中新增至 DVD。  
  
    > [!NOTE]
    >  會分割系統 .wim 檔案，使其符合檔案不超過 4 GB 的 FAT32 需求。 在此程序期間，用來擷取 .wim 檔案的目標所需容量必須大於 8 GB，才能應付分割程序。  
  
####  <a name="BKMK_Creating"></a> 步驟 3:若要建立伺服器復原 DVD  
 原廠出貨的每部伺服器都必須隨附伺服器復原 DVD。 您的 ADK 工具 DVD 包含建立 DVD 所需要的檔案。  
  
###### <a name="to-create-the-server-recovery-dvd"></a>建立伺服器復原 DVD  
  
1.  建立一個工作資料夾，做為儲存最終 ISO 的位置。  
  
2.  將合作夥伴 CD 中的 **Server Recovery** 資料夾內容複製到您在步驟 1 中建立的工作資料夾。  
  
3.  將您在執行 GenDiskXML.exe 時所建立的 disk.xml 檔案複製至 **Reset** 資料夾。  
  
4.  將您在執行 **GenDiskXML.exe** 時所建立的映像檔，複製到 **Reset** 資料夾。 檔案包括 .wim 及 .swm 檔案，而檔案數目則可能有所不同。  
  
5.  從資料夾中移除 GenDiskXML.exe。 此檔案僅供原廠處理之用，並且不應納入要出貨給客戶的 DVD 中。  
  
####  <a name="BKMK_Customizing"></a> 步驟 4:自訂精靈  
 伺服器復原應用程式必須使用裝置的影像，以及說明如何將特定裝置啟動至復原模式的文字進行自訂。 「還原檔案與資料夾精靈」的這個頁面會隨硬體而有所不同，因此將伺服器啟動至復原模式的步驟也會隨之不同。  
  
> [!NOTE]
>  列出的檔案名稱必須完全相符。  
  
1.  精靈頁面上會有其他說明的連結。 若此 .chm 檔案存在，則會覆寫網頁說明的 FWLink。 說明檔案位於：  
  
     < DVD 根目錄\>\\$OEM$ \Customization\\< 文化特性名稱\>\RestartHelp.chm  
  
2.  此檔案包含客戶在精靈頁面上所看見的文字。 這些文字會說明如何將伺服器開機至復原模式。 其控制項是可捲動的，它會對可新增的文字數量施以實用的限制。  
  
     下列檔案是用來取代精靈中的範例圖片，主要與商標有關。 它必須是 .png 檔案。 檔案大小必須是 256 像素 x 256 像素，否則會在於精靈中顯示時遭到截斷。  
  
     < DVD 根目錄\>\\$OEM$ \Customization\\< 文化特性名稱\>\RestartInstructions.rtf  
  
3.  < DVD 根目錄\>\\$OEM$ \Customization\\< 文化特性名稱\>\ServerImage.png  
  
 轉換伺服器復原 DVD 以支援多個語言時，請確定執行下列作業：  
  
1.  您一律必須有 en-us 資料夾。 伺服器復原應用程式若找不到與其執行所在之用戶端電腦相符的文化特性專屬檔案，則會切換至 en-us。  
  
2.  在您所建立的每個文化特性資料夾中，新增三個自訂檔案 (.png、.chm 及 .rtf)。  
  
3.  複製這兩個文化特性資料夾從 語言套件\\< CultureName\>\Server Recovery 伺服器復原 DVD 的根目錄。 例如: ES 及 ES-ES 這兩個資料夾將會複製到 DVD 的根目錄，以支援西班牙文。  
  
4.  完成 ISO 檔案。  
  
 支援的文化特性名稱包括：  

|-|-|  
|- cs-CZ<br /><br /> -DE-DE<br /><br /> -英文<br /><br /> - es-ES<br /><br /> - fr-FR<br /><br /> -HU-HU<br /><br /> -IT-IT<br /><br /> -若為 JA-JP<br /><br /> - ko-KR<br /><br /> - nl-NL|- pl-PL<br /><br /> - pt-BR<br /><br /> - pt-PT<br /><br /> -RU-RU<br /><br /> - sv-SE<br /><br /> - tr-TR<br /><br /> - zh-CN<br /><br /> - zh-HK<br /><br /> - zh-TW
  
####  <a name="BKMK_CreatingISO"></a> 步驟 5:建立 ISO 檔案  
 建立的資料夾與所有內容都可以燒錄至 DVD。 這是會隨新伺服器提供給客戶的 DVD。  
  
####  <a name="BKMK_Testing"></a> 步驟 6:測試復原 DVD  
 完成伺服器安裝之後，請設定伺服器備份，並執行伺服器備份，然後測試復原 DVD。  
  
###### <a name="to-configure-and-run-a-server-backup"></a>若要設定及執行伺服器備份  
  
1.  開啟 [儀表板]，按一下 **[裝置]** 索引標籤，然後按一下 **[工作]** 窗格中的 **[設定伺服器的備份]** 。  
  
2.  依照精靈中的指示設定備份伺服器備份。 在備份時需用到外接式硬碟。  
  
3.  按一下 **[工作]** 窗格中的 **[開始伺服器的備份]** ，然後遵循精靈中的指示作業。  
  
4.  備份完成時，請確認備份是否成功。  
  
###### <a name="to-restore-a-server"></a>若要還原伺服器  
  
1.  在透過集線器或交換器連線至伺服器所在之相同網路的用戶端電腦上，放入您所建立的復原 DVD。  
  
2.  按兩下 **setup.exe**。 伺服器復原精靈會提示您執行客戶將會進行的相同程序。  
  
3.  按一下 **[從備份還原伺服器]**，然後遵循精靈中的指示作業。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)