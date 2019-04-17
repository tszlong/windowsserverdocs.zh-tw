---
title: "疑難排解安裝 Windows Server Essentials"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 293b392203269a65efffcefb3744bedc659f71c9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>疑難排解安裝 Windows Server Essentials

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本主題提供適用於安裝 Windows Server Essentials 時，就會發生的問題進行疑難排解。 指導方針提供下列幾個方面：  
  

-   [一般疑難排解步驟](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [驅動程式問題的疑難排解](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [一般疑難排解步驟](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [驅動程式問題的疑難排解](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  針對 Windows Server Essentials 社群貢獻的最新疑難排解資訊，我們建議您瀏覽[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 論壇是一個很好的協助，或是詢問問題。  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a>一般疑難排解步驟  
 如果無法安裝 Windows Server Essentials，進行下列步驟來協助找出造成失敗的問題。  
  
> [!IMPORTANT]
>  請務必您在安裝 Windows Server Essentials 時無法手動重新您的伺服器。 伺服器重新開機自動數次期間設定與初始設定。 如果您重新前手動啟動伺服器看到**安裝伺服器成功**訊息，且可能會有中斷安裝造成失敗。  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>找出中的 Windows Server Essentials 安裝失敗的問題  
  
1.  確認您的伺服器硬體符合最低需求。 硬體需求的相關資訊，請查看[系統需求適用於 Windows Server Essentials](../get-started/system-requirements.md)。  
  
2.  如果您收到 MSDN Windows Server Essentials 安裝 DVD，請確認 DVD 有效檢查 SHA1 總和。 如需詳細資訊，請查看[可用性和描述檔案檢查值驗證工具公用程式的](https://go.microsoft.com/fwlink/?LinkId=220495)(https://go.microsoft.com/fwlink/?LinkId=220495)。  
  
3.  確認伺服器上的網路介面卡連接至路由器，網路電纜。  
  
4.  如果伺服器有多個網路介面卡，請確認該只有一個網路介面卡支援。  
  
    > [!IMPORTANT]
    >  請勿中斷網路電纜或路由器重新安裝 Windows Server Essentials。  
  
5.  檢視中的 [伺服器安裝並部署][發行 Windows Server Essentials 的文件](../get-started/release-notes.md)  
  
6.  如果您收到錯誤訊息時在安裝期間，您的伺服器設定使用伺服器修復 DVD 和您的硬體製造商所提供的指示來還原為原廠預設設定伺服器時發生錯誤。  
  
##  <a name="BKMK_TroubleshootDrivers"></a>驅動程式問題的疑難排解  
 安裝 Windows Server Essentials 的最常見的問題會儲存控制器需要手動安裝驅動程式。 Windows 包含許多儲存控制器驅動程式，但它可能不包含您的特定硬體驅動程式。  
  
 您也可能需要手動安裝網路卡驅動程式，您的特定硬體。  
  
###  <a name="BKMK_StorageDrivers"></a>新增儲存空間控制器驅動程式  
 如果您的硬體需要不包含 Windows Server Essentials 的存放裝置驅動程式，請使用下列資訊，以完成設定。  
  
 如果您在設定期間看到以下訊息，您必須手動新增儲存空間控制器驅動程式：  
  
 **Windows Server 安裝錯誤**  
  
 找不到硬碟裝載 Windows Server Essentials 的功能。 您想要載入其他存放裝置驅動程式  
  
 若要安裝的儲存空間 controller 驅動程式，使用下列程序。  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>若要手動安裝的儲存空間 controller 驅動程式  
  
1.  尋找您的儲存空間控制器驅動程式。 這些製造商所提供的硬體，以及它們可能也可以在製造商的網站上。  
  
2.  建立資料夾在磁碟片稱為「驅動程式或 USB 快閃磁碟機，再複製到資料夾中的 [驅動程式。  
  
3.  連接到電腦的磁碟機或驅動程式的 USB 快閃磁碟機  
  
4.  將電腦從 Windows Server Essentials DVD 開機。  
  
     如果遺失任何儲存 controller 驅動程式，會顯示 Windows Server Essentials 安裝程式錯誤對話方塊。  
  
5.  在 Windows Server Essentials 的安裝程式錯誤] 對話方塊中，按一下 [ **[是]**來載入額外的儲存空間的驅動程式。  
  
6.  在**請選取 [驅動程式 inf 檔案**提示，瀏覽至 [驅動程式] 資料夾中的.inf 檔案磁片或 USB 快閃磁碟機上，選取檔案，檔案名稱，以滑鼠右鍵按一下，然後按一下**公開**。 這會載入驅動程式。  
  
    > [!NOTE]
    >  您想要載入檔案之前，請確認檔案名稱擴充功能 (.inf) 是小寫字母。 這項操作有區分大小寫，和驅動程式檔案不會載入副檔名有大寫字母。  
  
7.  在命令提示字元中，按一下 [ **[是]**若要安裝的文字模式階段使用儲存裝置驅動程式。  
  
 現在應該通常會繼續安裝。  
  
###  <a name="BKMK_AddingNICdrivers"></a>新增網路介面卡驅動的程式  
 如果不支援 Windows Server Essentials 在電腦上的網路介面卡，您的伺服器不會有網路連接安裝完成之後，以及您無法將電腦連接到您的伺服器。  
  
 Windows Server Essentials 安裝結尾，會通知您若未自動安裝網路介面卡驅動程式。 您也可以使用**網路連接**在 [控制台] 來檢查是否有遺失的網路介面卡驅動程式。 如果您看不到網路連接相關的伺服器上的網路介面卡，您必須安裝驅動程式。  
  
 電腦遺漏任何網路介面卡的支援驅動程式，如果您要手動安裝網路介面卡驅動程式，然後重新開機伺服器。 使用下列程序。  
  
##### <a name="to-install-a-network-adapter-driver"></a>若要安裝網路介面卡驅動程式  
  
1.  從網路介面卡製造商取得遺失的驅動程式。  
  
2.  請依照製造商的安裝指示安裝驅動程式。  
  
3.  電腦重新開機。  
  
    > [!IMPORTANT]
    >  如果您無法重新執行伺服器安裝缺少的網路介面卡驅動程式之後，安裝 Windows Server Essentials 連接器軟體 client 電腦可能會失敗。
