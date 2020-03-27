---
title: Windows Server Essentials 安裝問題疑難排解
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f8adf97011629dbdd68622b5d6ad1ba40f7bc2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318617"
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Windows Server Essentials 安裝問題疑難排解

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本主題提供安裝 Windows Server Essentials 時所發生問題的疑難排解。 其中包括下列方面的指引：  
  

-   [一般疑難排解步驟](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [疑難排解驅動程式問題](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [一般疑難排解步驟](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [疑難排解驅動程式問題](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  如需 Windows Server Essentials 社區最新的疑難排解資訊，建議您流覽[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 論壇是一個搜尋協助或詢問問題的好地方。  
  
##  <a name="general-troubleshooting-steps"></a><a name="BKMK_GeneralTroubleshootingSteps"></a>一般疑難排解步驟  
 如果 Windows Server Essentials 的安裝失敗，請採取下列步驟來協助識別造成失敗的問題。  
  
> [!IMPORTANT]
>  請務必不要在安裝 Windows Server Essentials 時手動重新開機伺服器。 在安裝和初始設定期間，伺服器會自動重新啟動幾次。 如果您在看到 [伺服器安裝成功] 訊息之前手動重新啟動伺服器，可能會中斷安裝並導致安裝失敗。  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>若要識別 Windows Server Essentials 安裝失敗的問題  
  
1.  確認您的伺服器硬體符合最低需求。 如需硬體需求的相關資訊，請參閱[Windows Server Essentials 的系統需求](../get-started/system-requirements.md)。  
  
2.  如果您從 MSDN 收到 Windows Server Essentials 安裝 DVD，請檢查 SHA1 總和來確認 DVD 是否有效。 如需詳細資訊，請參閱檔案總和[檢查碼完整性驗證器公用程式的可用性和描述](https://go.microsoft.com/fwlink/?LinkId=220495)（ https://go.microsoft.com/fwlink/?LinkId=220495)。  
  
3.  確認伺服器上的網路介面卡已透過網路纜線連線到路由器。  
  
4.  如果伺服器具有多張網路介面卡，請確認只啟用一張網路介面卡。  
  
    > [!IMPORTANT]
    >  安裝 Windows Server Essentials 時，請勿中斷網路纜線或重新開機路由器。  
  
5.  請參閱[Windows Server Essentials 版本檔中的](../get-started/release-notes.md)「伺服器安裝和部署」  
  
6.  如果您收到錯誤訊息：在安裝期間設定伺服器時發生錯誤，請使用伺服器復原 DVD 和硬體製造商所提供的指示，將伺服器還原成原廠預設設定。  
  
##  <a name="troubleshoot-driver-issues"></a><a name="BKMK_TroubleshootDrivers"></a>疑難排解驅動程式問題  
 安裝 Windows Server Essentials 最常見的問題是需要手動安裝驅動程式的存放控制器。 Windows 包含許多存放裝置控制器的驅動程式，但可能未包含特定硬體的驅動程式。  
  
 您也可能需要手動安裝特定硬體的網路卡驅動程式。  
  
###  <a name="adding-drivers-for-storage-controllers"></a><a name="BKMK_StorageDrivers"></a>新增存放裝置控制器的驅動程式  
 如果您的硬體需要不包含在 Windows Server Essentials 中的存儲裝置驅動程式，請使用下列資訊來完成安裝程式。  
  
 如果您在安裝期間看到下列訊息，您需要手動新增存放裝置控制器的驅動程式：  
  
 **Windows Server 安裝程式錯誤**  
  
 找不到能夠裝載 Windows Server Essentials 的硬碟。 是否要載入其他存放裝置驅動程式?  
  
 請使用下列程序來安裝存放裝置控制器驅動程式。  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>手動安裝存放裝置控制器驅動程式  
  
1. 找到您的存放裝置控制器的驅動程式。 這些驅動程式是由硬體製造商所提供，因此也可能會在製造商的網站上。  
  
2. 在磁碟片或 USB 快閃磁碟機上建立名為 DRIVERS 的資料夾，然後將驅動程式複製到該資料夾。  
  
3. 將含有驅動程式的磁碟片或 USB 快閃磁碟機連接到電腦。  
  
4. 從 Windows Server Essentials DVD 開機電腦。  
  
    如果遺失任何存放控制器驅動程式，則會顯示 [Windows Server Essentials 安裝程式錯誤] 對話方塊。  
  
5. 在 [Windows Server Essentials 安裝程式錯誤] 對話方塊中，按一下 **[是]** 載入其他存儲裝置驅動程式。  
  
6. 在出現 [請選取您的驅動程式的 inf 檔] 提示時，巡覽至磁碟片或 USB 快閃磁碟機上 DRIVERS 資料夾中的 .inf 檔，選取檔案，然後以滑鼠右鍵按一下檔案名稱，再按一下 [開啟]。 這會載入驅動程式。  
  
   > [!NOTE]
   >  在您嘗試載入檔案之前，請先確認副檔名 (.inf) 為小寫字母。 這項作業區分大小寫，如果副檔名有大寫字母，則不會載入驅動程式檔案。  
  
7. 出現提示時，按一下 [是]，以在安裝程式的文字模式階段期間提供存放裝置驅動程式。  
  
   安裝程式現在應該會繼續正常運作。  
  
###  <a name="adding-drivers-for-network-adapters"></a><a name="BKMK_AddingNICdrivers"></a>新增網路介面卡的驅動程式  
 如果 Windows Server Essentials 不支援電腦上的網路介面卡，則在安裝程式完成之後，您的伺服器將無法連線到網路，而且您將無法將電腦連線到您的伺服器。  
  
 在 Windows Server Essentials 安裝結束時，如果未自動安裝網路介面卡驅動程式，系統會通知您。 您也可以使用 [控制台] 中的 [網路連線]，檢查是否有任何網路介面卡驅動程式遺失。 如果看不到與您伺服器上的網路介面卡關聯的網路連線，您需要安裝驅動程式。  
  
 如果電腦遺失任何網路介面卡支援的驅動程式，您需要手動安裝正確的網路介面卡驅動程式，再重新啟動伺服器。 請使用下列程序。  
  
##### <a name="to-install-a-network-adapter-driver"></a>安裝網路介面卡驅動程式  
  
1.  向網路介面卡的製造商取得遺失的驅動程式。  
  
2.  遵循製造商的安裝指示來安裝驅動程式。  
  
3.  重新啟動電腦。  
  
    > [!IMPORTANT]
    >  如果您在安裝遺失的網路介面卡驅動程式之後未重新開機伺服器，則用戶端電腦上的 Windows Server Essentials 連接器軟體安裝可能會失敗。
