---
title: "安裝 Windows Server Essentials 移轉模式 1"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 808a4b1e120fa559d603b34ad006b18de6b94378
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>安裝 Windows Server Essentials 移轉模式 1

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以在您網路上執行的 Windows Server Essentials，讓只有一個伺服器，必須網路的網域控制站伺服器。  
  
 當您安裝 Windows Server Essentials 移轉模式時，在安裝精靈會執行下列工作：  
  
1.  安裝和 Windows Server Essentials 伺服器軟體設定目的伺服器上。  
  
2.  更新至最新版本的網域結構描述。  
  
3.  加入網域現有目的伺服器。 來源伺服器和目的地伺服器可以已經成員相同的網域之前完成此移轉程序。 移轉完成之後，您必須從網路移除來源伺服器 21 天中。  
  
    > [!WARNING]
    >  錯誤訊息會新增至事件登入期間 21 日優惠直到您移除您的網路來源伺服器每一天。 將文字的訊息會顯示 「 FSMO 角色檢查偵測到條件授權原則遵守退出的環境中。 管理伺服器必須按住主要網域控制站和網域命名主要 Active Directory 角色。 請移至管理伺服器的 Active Directory 角色現在。 此伺服器將會自動關閉如果中的第一次此條件偵測到的時間 21 天無法修正問題。 」 21 日優惠期間之後來源伺服器會關閉。  
  
4.  從來源伺服器的操作 （也稱為彈性的單一主機操作或 FSMO） 主機傳送到目的伺服器。 操作主機角色是標準資料傳輸與更新程式方法不足時使用的特定的網域控制站工作。 網域控制站伺服器目的地時，它必須按住作業主機角色。  
  
5.  設定為通用伺服器的目的地的伺服器。 管理分散式的資料存放庫網域控制站伺服器通用。 它包含每個物件 Active Directory 森林中的每個網域中的搜尋、 部分表示。  
  
6.  設定目標伺服器網站授權伺服器。  
  
##  <a name="BKMK_Install"></a>目的地伺服器上安裝 Windows Server Essentials  
 若要安裝 Windows Server Essentials 設定目標伺服器移轉模式中，執行下列程序。  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>目的地伺服器上安裝 Windows Server Essentials  
  
1.  關閉目的伺服器上與 Windows Server Essentials DVD1 插入 DVD 光碟機。 如果您看到的訊息，詢問您是否想要從 CD 或 DVD 開機，請按任意鍵來執行動作。  
  
    > [!NOTE]
    >  如果目的伺服器支援的 USB 快閃磁碟機開機，您可以使用**Windows 7 USB/DVD 下載工具**來建立可開機的 USB 快閃磁碟機從 Windows Server Essentials 的 ISO 檔案。 使用 USB 快閃磁碟機可以大幅加速，安裝程序因為快閃磁碟機朗讀資料更快地比 DVD-ROM 光碟機。 建立可開機的 USB 快閃磁碟機之後，您可以新增回應檔案到快閃磁碟機。 您可以[下載 Windows 7 USB/DVD 下載工具](https://go.microsoft.com/fwlink/p/?LinkId=248282)Microsoft 網上商店網站可用。  
  
    > [!NOTE]
    >  如果目的伺服器不會從 DVD 開機時，電腦重新開機，並檢查以確保 BIOS 設定**DVD-ROM**中列出的第一次開機順序。 如需如何變更 BIOS 設定開機順序，查看您的硬體製造商。  
  
2.  按一下**安裝新**。  
  
3.  如果您不會顯示在清單中內部硬碟，請按一下**載入驅動程式**並安裝必要的驅動程式，才能繼續。  
  
4.  選取核取方塊，確認所有檔案和資料夾主要硬碟繼續嗎、，然後按一下**安裝**。  
  
5.  在**選擇伺服器安裝模式**頁面上，按一下 [**伺服器移轉**，然後提供移轉所需的資訊。  
  
6.  當**您伺服器順利**訊息會出現，按一下 [**關閉**。  
  
 安裝完成後，您會自動登入的使用者管理員和您所提供的移轉回應檔案中的密碼。  
  
> [!NOTE]
>  若要解除鎖定的桌面，安裝 Windows Server Essentials 時，請使用建管理員並留密碼。  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a>請確認網域控制站的健康狀態  
 移轉的之前，您應該確定都良好的網域控制站和 Windows Server Essentials 的網路。  
  
 下表列出的工具，可供您診斷問題目的伺服器上網路並網域中：  
  
|工具|描述|  
|----------|-----------------|  
|Netdiag|可協助找出網路與連接的問題。 如需詳細資訊，並下載，請查看[Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388)。|  
|Dcdiag.exe|分析網域控制站森林或企業版中的狀態，並報告來協助您進行疑難排解的問題。 如需詳細資訊，並下載，請查看[Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389)。|  
|Repadmin.exe|可協助您診斷複寫網域控制站之間的問題。 這項工具會需要執行的命令列參數。 如需詳細資訊，並下載，請查看[Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387)。|  
  
 您應該會修正移轉進行之前，請先這些工具報告所有的問題。  
  
> [!NOTE]
>  如果您打算移轉到另一個先 Exchange server 的電子郵件，請查看[和 Windows Server Essentials 整合 On 先 Exchange Server](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md)的資訊，了解如何設定您在場所 Exchange server。
