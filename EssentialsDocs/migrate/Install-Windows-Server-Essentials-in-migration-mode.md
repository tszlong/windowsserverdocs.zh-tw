---
title: 在遷移 mode1 中安裝 Windows Server Essentials
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d253a550763f34409f25223e6319607b9abcd8e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852581"
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>在遷移 mode1 中安裝 Windows Server Essentials

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您的網路上只能有一部執行 Windows Server Essentials 的伺服器，而且該伺服器必須是網路的網域控制站。  
  
 當您以移轉模式安裝 Windows Server Essentials 時，安裝精靈會執行下列工作：  
  
1.  在目的地伺服器上安裝和設定 Windows Server Essentials 伺服器軟體。  
  
2.  將網域架構更新至最新版本。  
  
3.  將目的地伺服器加入現有網域。 移轉程序完成之前，來源伺服器與目的地伺服器可以是相同網域的成員。 移轉完成之後，您必須在 21 天內從網路中移除來源伺服器。  
  
    > [!WARNING]
    >  在您從網路移除來源伺服器之前的 21 天寬限期內，每天都會有一則錯誤訊息新增至事件記錄檔。 訊息文字為：「FSMO 角色檢查偵測到您的環境中有一個條件不符合授權原則。 管理伺服器必須具有網域主控站和網域命名主要 Active Directory 角色。 請立即將 Active Directory 角色移至管理伺服器。 如果從第一次偵測到這個情況後的 21 天內未修正此問題，將會自動關閉這部伺服器。」 21 天寬限期之後，會關閉來源伺服器。  
  
4.  從來源伺服器將操作主機 (也稱為彈性單一主機操作或 FSMO) 角色轉移到目的地伺服器。 操作主機角色是特殊化的網域控制站工作，主要用於不適合使用標準資料傳送與更新方法的情況。 當目的地伺服器成為網域控制站時，它必須擁有操作主機角色。  
  
5.  將目的地伺服器設定成通用類別目錄伺服器。 通用類別目錄伺服器是負責管理分散式資料儲存機制的網域控制站。 它包含 Active Directory 樹系中每個網域之每個物件的可搜尋、部分表示。  
  
6.  將目的地伺服器設定成站台授權伺服器。  
  
##  <a name="install-windows-server-essentials-on-the-destination-server"></a><a name="BKMK_Install"></a>在目的地伺服器上安裝 Windows Server Essentials  
 若要在目的地伺服器上以移轉模式安裝和設定 Windows Server Essentials，請執行下列程式。  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>在目的地伺服器上安裝 Windows Server Essentials  
  
1. 開啟目的地伺服器，並將 Windows Server Essentials DVD1 插入 DVD 光碟機。 如果您看到一則訊息，詢問您是否要從 CD 或 DVD 開機，請按任一鍵以執行這項操作。  
  
   > [!NOTE]
   >  如果目的地伺服器支援從 USB 快閃磁片磁碟機開機，您可以使用**windows 7 USB/DVD 下載工具**，從 Windows SERVER Essentials ISO 檔案建立可開機的 Usb 快閃磁片磁碟機。 使用 USB 快閃磁碟機可以明顯加速安裝程序，因為快閃磁碟機讀取資料比 DVD-ROM 光碟機更快。 建立可開機的 USB 快閃磁碟機之後，您可以將回應檔案新增到快閃磁碟機。 您可以在 Microsoft Store 網站免費[下載 Windows 7 USB/DVD 下載工具](https://go.microsoft.com/fwlink/p/?LinkId=248282)。  
  
   > [!NOTE]
   >  如果目的地伺服器不會從 DVD 開機，請重新啟動電腦並檢查 BIOS 設定，以確保開機順序中會先列出 [DVD-ROM]。 如需如何變更 BIOS 設定開機順序的相關詳細資訊，請參閱硬體製造商的說明文件。  
  
2. 按一下 [新的安裝]。  
  
3. 如果您有未顯示在清單中的內部硬碟，請按一下 [載入驅動程式]，並在繼續前先安裝必要的驅動程式。  
  
4. 選取核取方塊，確認主要硬碟上所有的檔案與資料夾都會被刪除，然後按一下 [安裝]。  
  
5. 在 [選擇伺服器的安裝模式] 頁面上，按一下 [伺服器移轉]，然後提供必要的移轉資訊。  
  
6. 當 [已成功移轉您的伺服器] 訊息出現，請按一下 [關閉]。  
  
   安裝完成後，會自動以您在移轉回應檔案中提供的系統管理員使用者帳戶與密碼登入。  
  
> [!NOTE]
>  若要在 Windows Server Essentials 安裝時解除鎖定桌面，請使用內建的系統管理員帳戶，並將密碼保留空白。  
  
##  <a name="verify-the-health-of-the-domain-controller"></a><a name="BKMK_VerifyTheHealthOfDC"></a>確認網域控制站的健全狀況  
 繼續進行遷移之前，您應該確定網域控制站和 Windows Server Essentials 網路的狀況良好。  
  
 下表列出您可以用來診斷目的地伺服器、網路和網域問題的工具：  
  
|工具|描述|  
|----------|-----------------|  
|Netdiag|協助隔離網路和連線問題。 如需詳細資訊及下載資訊，請參閱 [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388)。|  
|Dcdiag.exe|分析樹系或企業的網域控制站狀態，然後回報問題以協助您疑難排解。 如需詳細資訊及下載資訊，請參閱 [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389)。|  
|Repadmin.exe|協助您診斷網域控制站間的複寫問題。 這個工具需要命令列參數才能執行。 如需詳細資訊及下載資訊，請參閱 [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387)。|  
  
 您應該修正這些工具回報的所有問題，然後再繼續移轉作業。  
  
> [!NOTE]
>  如果您打算將電子郵件遷移至另一個內部部署 Exchange 伺服器，請參閱[整合內部部署 Exchange server 與 Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) ，以取得如何設定內部部署 exchange server 的相關資訊。
