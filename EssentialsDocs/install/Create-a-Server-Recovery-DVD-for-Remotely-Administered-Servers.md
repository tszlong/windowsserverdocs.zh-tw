---
title: "建立伺服器復原 DVD 伺服器的遠端管理"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>建立伺服器復原 DVD 伺服器的遠端管理

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_HeadlessRecovery"></a>建立伺服器復原 DVD 伺服器的遠端管理  
 有兩個型號的原廠重設」和「伺服器修復其與依據客戶收到的硬體。  
  
 **遠端管理的伺服器**  
  
 此伺服器修復選項需要程序的 client 電腦執行相同的網路。 因為原廠重設需要特定硬體影像隨附伺服器，合作夥伴必須撰寫伺服器復原 DVD。  
  
 **本機管理的伺服器**  
  
 這是在 [伺服器] 主控台系統管理伺服器傳統模型。 伺服器安裝媒體用來執行復原。 這需要伺服器隨附檢視視訊輸出除了包括 DVD 助讀程式的能力。 客戶該伺服器的安裝媒體，用來開機，然後選取適當的復原的方法。 您不需要建立伺服器復原 DVD 本機管理的伺服器。  
  
> [!NOTE]
>  本機管理的伺服器模型，客戶可能完成原廠重設執行全新安裝。 不過，他們將會遺失合作夥伴的自訂項目。  
  
 有兩種類型的伺服器復原。  
  
 **原廠重設**  
  
 這個修復伺服器傳回伺服器出貨的原廠時的原始狀態。 原廠重設後系統會要求您執行伺服器的初次設定，就像您已關閉，第一次與所有設定自訂項目遺失。 這也稱為天 0。嗎？ 因為原廠重設需要特定硬體影像隨附伺服器，合作夥伴必須撰寫伺服器復原 DVD。  
  
 **極金屬還原**  
  
 此復原假設您伺服器備份設定和伺服器備份已成功完成之前伺服器失敗的至少一次。 極金屬還原 (BMR) 支援只從先前的伺服器備份系統和開機磁碟機復原。  
  
 BMR，依照伺服器會傳回至當時用來還原備份的狀態。 這通常是最新的備份，但有時可能較早的備份。 資料復原完成之後，系統會還原使用 [還原的檔案和資料夾精靈。 BMR 是慣用的伺服器復原的方法因為不會傳回所有設定和設定，而原廠重設會每天 0 狀態伺服器。  
  
### <a name="remotely-administered-server-recovery"></a>遠端管理伺服器復原  
 本節所需的自訂項目合作夥伴必須執行和最終媒體必須隨附每個伺服器。 在探究之前詳細資料，讓我們看看顧客的體驗。  
  
 在本案例中，客戶」度的伺服器無法再運作。 這可能是由病毒、硬碟故障或其他某些原因。 客戶中伺服器復原 DVD 插入 client 伺服器相同的網路上的電腦。 伺服器修復應用程式會引導它們復原他們伺服器的三個步驟：  
  
1.  建立可開機 USB 快閃磁碟機，用來重新伺服器修復模式。 USB 快閃磁碟機必須 8 GB 或更大。  
  
2.  偵測到現在修復模式中的伺服器。  
  
3.  客戶選擇原廠重設或還原極金屬，可讓，然後將其伺服器傳回正常運作的狀態。  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>步驟建立伺服器復原 DVD 多種語言的支援  
 有六個主要的步驟來建立伺服器復原 DVD  
  
1.  [（選擇性）更新 WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [收集的原廠重設映像和 XML 檔案](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [建立伺服器復原 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [自訂精靈](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [建立 ISO 檔案](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [測試復原 DVD](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a>步驟 1:（選擇性）更新 WinPE  
 ADK 包含了一份 Windows PE\ 自訂。 此映像會開機，它會自動時限指標 client 修復應用程式所用來連接伺服器修復模式。  
  
 Windows PE\ 需要進一步自訂新增任何特定的硬體驅動程式，例如網路或磁碟 controller 驅動程式。 從 WinPE 開機，系統硬碟需要辨識再網路必須運作。  
  
####  <a name="BKMK_Collecting"></a>步驟 2：收集的原廠重設映像和 XML 檔案  
 若要重設為原廠預設值的伺服器，下列兩個影像需要擷取：  
  
-   系統映像的磁碟機  
  
-   系統保留的磁碟分割  
  
 若要擷取這些影像，提供 GenDiskXML.exe 工具。 GenDiskXML.exe 也會收集名為 disk.xml，用來重新建立磁碟設定的修復程序的檔案。  
  
1.  依照 Sysprep，請使用任何 64 位元版本的 Windows PE\ 重新開機。  
  
2.  若要輸出.xml 和.wim 檔案到外部來源，執行`GenDiskXML /outputdir:<path>`以.xml 和.wim 任何外部來源檔案。 這些檔案會新增至下一個步驟 DVD。  
  
    > [!NOTE]
    >  分割系統.wim 檔案都能以符合 FAT32 需求的任何檔案大於 4 GB。 在過程中，所需的容量的目標是用來擷取.wim 的檔案必須大於容納分割程序 8 GB。  
  
####  <a name="BKMK_Creating"></a>步驟 3：建立伺服器復原 DVD  
 出貨的原廠每個伺服器必須伴隨伺服器復原 DVD。 ADK 工具 DVD 包含建立 DVD 所需的檔案。  
  
###### <a name="to-create-the-server-recovery-dvd"></a>若要建立伺服器復原 DVD  
  
1.  建立 ISO 最後的存放位置作為的工作資料夾。  
  
2.  從 CD 合作夥伴，複製到**伺服器復原**您在步驟 1 中建立工作資料夾的資料夾。  
  
3.  當您在 GenDiskXML.exe 來建立 disk.xml 檔案複製**重設**資料夾。  
  
4.  當您在執行時所建立的影像檔案複製**GenDiskXML.exe**以**重設**資料夾。 檔案.wim 和.swm 檔案，而且可能會不同的檔案。  
  
5.  GenDiskXML.exe 移除資料夾。 它只適用於原廠用途，並不應包含 dvd 客戶是出貨。  
  
####  <a name="BKMK_Customizing"></a>步驟 4：自訂精靈  
 伺服器修復應用程式必須自訂告訴您如何開始的特定裝置進入修復模式的文字與裝置的映像。 因為此頁面還原的檔案和資料夾精靈的特定硬體，若要開始伺服器進入修復模式的步驟會而有所不同。  
  
> [!NOTE]
>  必須符合所列的檔案名稱。  
  
1.  精靈網頁有其他協助的連結。 如果此.chm 檔案已存在，它將會覆寫 FWLink web 協助。 協助檔案位於：  
  
     < DVD Root\ > \\$OEM$\Customization\\ < 文化的群島 name\ > \RestartHelp.chm  
  
2.  此檔案中包含客戶看到精靈頁面的文字。 文字應該如何伺服器開機進入修復模式。 控制項是捲動，將在文字可新增數量實用限制。  
  
     下列檔案用來取代範例中的圖片精靈中，且主要有關商標。 必須.png 檔案。 檔案大小必須 256 個像素 x 256 像素，或時，它會顯示在精靈將會被截斷。  
  
     < DVD Root\ > \\$OEM$\Customization\\ < 文化的群島 name\ > \RestartInstructions.rtf  
  
3.  < DVD Root\ > \\$OEM$\Customization\\ < 文化的群島 name\ > \ServerImage.png  
  
 當您正在轉換伺服器復原 DVD 來支援多種語言時，請確定您下列動作：  
  
1.  您必須先有 en-us-我們資料夾。 如果伺服器修復應用程式找不到特定文化檔案符合 client 電腦執行，它會回復成 en-us-我們。  
  
2.  在您的每個文化的群島] 資料夾中新增的三個自訂檔案 (.png，.chm，以及.rtf)。  
  
3.  複製這兩個文化的群島資料夾語言 Packs\\ < CultureName\ > \Server 修復根的伺服器復原 DVD。 例如：ES 和 ES-ES 資料夾想複製到根的 DVD 支援西班牙文。  
  
4.  完成程序的 ISO 檔案。  
  
 支援文化的群島名稱包含：  

|-|-|  
|-CS-CZ<br /><br /> -DE-DE<br /><br /> -EN-US<br /><br /> -ES-ES<br /><br /> -FR-FR<br /><br /> -HU-HU<br /><br /> -IT-IT<br /><br /> -JA-JP<br /><br /> -KO-KR<br /><br /> -NL-NL |-PL-PL<br /><br /> -PT-BR<br /><br /> -PT-PT<br /><br /> -RU-RU<br /><br /> -SV-SE<br /><br /> -TR-TR<br /><br /> -ZH-CN<br /><br /> -ZH-HK<br /><br /> -ZH-TW
  
####  <a name="BKMK_CreatingISO"></a>步驟 5：建立 ISO 檔案  
 建立資料夾並所有到可以燒錄 dvd。 這是將會提供以針對他們新伺服器 DVD。  
  
####  <a name="BKMK_Testing"></a>步驟 6：測試修復 DVD  
 完成之後安裝的伺服器，設定伺服器備份、執行伺服器備份，並再測試修復 DVD。  
  
###### <a name="to-configure-and-run-a-server-backup"></a>設定並執行伺服器備份  
  
1.  打開儀表板，請按一下**裝置**索引標籤，然後按一下 [**設定的備份伺服器**中**工作**窗格。  
  
2.  請依照精靈中的指示來設定備份伺服器備份。 您的備份需要外接式硬碟。  
  
3.  按一下**開始伺服器備份]**在**工作**] 窗格中，，然後依照精靈中的指示進行。  
  
4.  備份完成時，請確認已成功。  
  
###### <a name="to-restore-a-server"></a>若要還原伺服器  
  
1.  復原您所建立的 DVD 插入 client 電腦已連接到透過中樞或切換相同的網路。  
  
2.  按兩下**setup.exe**。 伺服器修復精靈將會提示您透過客戶會依照相同的程序。  
  
3.  按一下**從備份還原伺服器**，然後依照精靈中的指示進行。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)