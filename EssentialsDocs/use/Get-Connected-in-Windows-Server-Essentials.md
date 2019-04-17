---
title: "在 Windows Server Essentials 連接"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 05/07/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1d7f3c33f1254c8dbe4af8bdf5baa4144c134248
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="get-connected-in-windows-server-essentials"></a>在 Windows Server Essentials 連接

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
  
 您可以使用連接器軟體與 Windows Server Essentials 伺服器連接您的電腦。 使用電腦連接到 [伺服器] 精靈伺服器連接電腦時，已安裝的連接器軟體。 您可以將此精靈開始輸入**http://<servername\>/connect**，其中**< servername\ >**伺服器的名稱。  
  
 本主題：  
  

-   [準備伺服器連接的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [伺服器連接的電腦使用軟體連接器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [使用 Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [準備伺服器連接的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [伺服器連接的電腦使用軟體連接器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [使用 Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

  
##  <a name="BKMK_A"></a>準備伺服器連接的電腦  
 本節中討論連接器軟體，Windows Server Essentials、必要的工作，必須先完成之前的伺服器來連接您的電腦和伺服器對電腦當您執行的連接器軟體所做的變更會支援的作業系統。  
  

-   [連接器軟體概觀](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [伺服器連接電腦時的必要元件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [必要條件網路來連接 Mac 電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [支援的作業系統 client 的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [伺服器 client 電腦所做的變更](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [網路使用者名稱和密碼資訊](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [伺服器的管理員](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [移除 Windows 網域的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>連接器軟體概觀  
 Windows Server Essentials 作業系統的連接器軟體連接您的網路中電腦與 Windows Server Essentials 的伺服器。 當您伺服器連接電腦時，將連接器軟體可讓您自動備份的電腦及監視他們 health。 連接器軟體也可讓您設定，並從遠端管理 Windows Server Essentials 伺服器。 當您 client 電腦連接到 server 安裝連接器軟體。 Windows Server Essentials 伺服器連接 client 電腦的相關詳細的指示，請查看[伺服器連接電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)本主題中的更新版本。  

-   [連接器軟體概觀](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [伺服器連接電腦時的必要元件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [必要條件網路來連接 Mac 電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [支援的作業系統 client 的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [伺服器 client 電腦所做的變更](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [網路使用者名稱和密碼資訊](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [伺服器的管理員](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [移除 Windows 網域的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>連接器軟體概觀  
 Windows Server Essentials 作業系統的連接器軟體連接您的網路中電腦與 Windows Server Essentials 的伺服器。 當您伺服器連接電腦時，將連接器軟體可讓您自動備份的電腦及監視他們 health。 連接器軟體也可讓您設定，並從遠端管理 Windows Server Essentials 伺服器。 當您 client 電腦連接到 server 安裝連接器軟體。 Windows Server Essentials 伺服器連接 client 電腦的相關詳細的指示，請查看[伺服器連接電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)本主題中的更新版本。  

  
###  <a name="BKMK_2"></a>伺服器連接電腦時的必要元件  
 之前網路來連接的電腦必須符合下列需求︰  
  
-   Windows Server Essentials 的安裝完畢之後，並伺服器正在執行。 如果是無法與伺服器連接器軟體結束安裝。  
  

-   Client 電腦正在執行支援的作業系統。 如需詳細資訊，請查看[client 電腦支援的作業系統](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)。

  
-   Client 的電腦必須配備具有正確連接到網際網路。  
  
-   Client 的電腦位於相同 IP 子網路 client 電腦時伺服器相同的網路上執行 Windows Server Essentials 的伺服器。  
  
-   Client 的電腦已安裝的.NET Framework 4.5。 連接器軟體會自動安裝該電腦上。  
  
-   Client 的電腦符合下列最低系統需求︰  
  
    -   1.4 GHz 或更快的處理器  
  
    -   1 gb 的 RAM  
  
    -   1 GB 的可用硬碟空間（此區域的部分釋出之後安裝）  
  
-   開機磁碟分割（也就是 windows 安裝所在的磁碟分割）格式化為 NTFS 檔案系統。  
  
-   電腦名稱不包含超過 15 個字元。  
  
-   電腦名稱會包括底線 (_)。  
  
-   電腦 s 的日期和時間設定對齊伺服器上的設定。  
  
-   在任何時候 client 電腦可以連接只有一個 Windows Server Essentials 的伺服器。  
  
-   已加入到另一個 Active Directory 網域 client 電腦不能加入 Windows Server Essentials 的網域。  
  
> [!NOTE]

>  在 Windows Server Essentials 或 Windows Server Essentials 先 client 的部署，您可以而不將它們新增到 Windows Server Essentials 網域的電腦連接到伺服器。 這個法不適用於所有支援的 client 作業系統和功能，例如 [群組原則和 virtual 私人網路 (Vpn)，這需要在連接電腦時，會網域，已無法使用。 需求和指示進行，查看[連接到 Windows Server Essentials 伺服器的電腦未加入網域](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  
  
 逐步電腦連接到執行 Windows Server Essentials 的伺服器，查看[電腦連接到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

>  在 Windows Server Essentials 或 Windows Server Essentials 先 client 的部署，您可以而不將它們新增到 Windows Server Essentials 網域的電腦連接到伺服器。 這個法不適用於所有支援的 client 作業系統和功能，例如 [群組原則和 virtual 私人網路 (Vpn)，這需要在連接電腦時，會網域，已無法使用。 需求和指示進行，查看[連接到 Windows Server Essentials 伺服器的電腦未加入網域](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  
  
 逐步電腦連接到執行 Windows Server Essentials 的伺服器，查看[電腦連接到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

  
###  <a name="BKMK_3"></a>必要條件網路來連接 Mac 電腦  
 之前網路來連接 Mac 的電腦必須符合下列需求︰  
  
-   伺服器作業系統安裝完成，且伺服器執行。 如果無法與 server 連接器軟體會安裝。  
  
-   電腦執行 Mac 作業系統 X 10.5（豹紋）或更新版本。  
  
-   電腦就相同 IP 子網路伺服器上。  
  
-   電腦必須配備具有正確連接到網際網路。  
  
-   請確定電腦符合下列最低系統需求︰  
  
    -   1.4 GHz 或更快的處理器  
  
    -   1 gb 的 RAM  
  
    -   1 GB 的可用硬碟空間（此區域的部分釋出之後安裝）  
  
-   在任何時候，可以將 client 電腦連接到只有一個伺服器。  
  
###  <a name="BKMK_4"></a>支援的作業系統 client 的電腦  
 Windows Server Essentials 所有支援的 client 電腦提供的功能。 這些功能包括加入網域，Launchpad，以及 client 端健康通知。  
  
> [!IMPORTANT]
>  Windows Server Essentials 不支援加入網域執行 Home、簡易版或 Media Center 的 Windows 版本的電腦。 此外，您無法使用遠端網頁存取將這些電腦連接。  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  本節適用於已安裝 Windows Server Essentials 體驗角色執行 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter server 或到執行 Windows Server Essentials 的伺服器。 下列電腦作業系統的支援︰  
  
 **Windows 7 的作業系統**  
  
-    Windows 7 家用入門的 SP1 (x 86 和 x64）  
  
-    Windows 7 家用進階版 SP1 (x 86 和 x64）  
  
-    Windows 7 專業 SP1 (x 86 和 x64）  
  
-    Windows 7 旗艦版 SP1 (x 86 和 x64）  
  
-    Windows 7 企業版 SP1 (x 86 和 x64）  
  
-    Windows 7 簡易版 SP1 (x86)  
  
 **Windows 8 作業系統**  
  
-   Windows 8  
  
-   Windows 8 專業版  
  
-   Windows 8 企業版  
  
 **Windows 8.1 作業系統**  
  
-   Windows 8.1  
  
-   Windows 8.1 專業版  
  
-   Windows 8.1 企業版  
  
 **Windows 10 作業系統**  
  
-   Windows 10  
  
-   Windows 10 專業版  
  
-   Windows 10 企業版  
  
-   Windows 10 教育版  
  
 **Mac client 電腦**  
  
-   Mac 作業系統 X v10.5 豹紋  
  
-   Mac 作業系統 X v10.6 雪豹紋  
  
-   Mac 作業系統 X v10.7 開普敦  
  
-   Mac 作業系統 X v10.8 美洲獅  
  
> [!NOTE]
>  您可以檢視健康備份 Mac 電腦從 Windows Server Essentials 儀表板的狀態。 不過，您無法設定的備份的電腦或從頭儀表板上建立的備份。 此外，您無法使用遠端 Web 存取連接 Mac 電腦。  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  本節適用於執行 Windows Server Essentials 的伺服器。 下列電腦作業系統的支援︰  
  
 **Windows 7 的作業系統**  
  
-    Windows 7 Home Basic (x86 and x64)  
  
-    Windows 7 Home Premium (x86 and x64)  
  
-    Windows 7 Professional (x86 and x64)  
  
-    Windows 7 Ultimate (x86 and x64)  
  
-    Windows 7 Enterprise (x86 and x64)  
  
-    Windows 7 Starter (x86)  
  
 **Windows 8 作業系統**  
  
-   Windows 8  
  
-   Windows 8 專業版  
  
-   Windows 8 企業版  
  
 **Windows 10 作業系統**  
  
-   Windows 10  
  
-   Windows 10 專業版  
  
-   Windows 10 企業版  
  
-   Windows 10 教育版  
  
 **Mac client 電腦**  
  
-   Mac 作業系統 X v10.5 豹紋  
  
-   Mac 作業系統 X v10.6 雪豹紋  
  
-   Mac 作業系統 X v10.7 開普敦  
  
> [!NOTE]
>  您可以檢視健康備份 Mac 電腦從 Windows Server Essentials 儀表板的狀態。 不過，您無法設定的備份的電腦或從頭儀表板上建立的備份。 此外，您無法使用遠端 Web 存取連接 Mac 電腦。  
  
###  <a name="BKMK_5"></a>伺服器 client 電腦所做的變更  
 當您伺服器連接電腦時，Windows Server Essentials 的軟體做一些變更的電腦讓您的電腦與伺服器可一起運作。  
  
 「軟體會執行下列動作︰  
  
-   在電腦上安裝軟體連接器  
  
-   如果尚未安裝，請在電腦上安裝 Microsoft.NET Framework 4.5  
  
-   建立儀表板並 Launchpad 電腦 s 桌面的快速鍵  
  
-   設定 Windows 防火牆連接埠允許下列功能才能在電腦上︰  
  
    -   Core 網路  
  
    -   遠端桌面服務  
  
-   為了備份的電腦上，進行下列變更︰  
  
    -   建立執行自動備份到排定的工作  
  
    -   安裝的服務管理備份操作與伺服器  
  
    -   安裝 virtual 裝置驅動程式時使用的檔案和資料夾還原處理程序  
  
-   安裝 Health 代理程式來偵測問題，並建立相對應的警示通知  
  
-   週期性 health 評估並進行同步處理 health 提醒定義電腦上建立排定的工作  
  
-   新增到電腦，通訊 server 與其他 Windows Server Essentials 功能的電腦使用的服務  
  
-   在執行 Windows 戶端允許遠端桌面服務執行 client 電腦開啟 TCP 連接埠 3389  
  
-   部署 VPN client 電腦上，如果 VPN 功能可以在 Windows Server Essentials，或如果 VPN 功能可以在 Windows Server Essentials 提供 auto-connect 體驗，提供只要按一下即可體驗  
  

 伺服器連接您的電腦的相關資訊，請查看[電腦連接到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
###  <a name="BKMK_6"></a>網路使用者名稱和密碼資訊  
 從管理您的伺服器的人員，您可以取得您網路使用者名稱和密碼的資訊。 您可以使用這些認證從伺服器連接您的電腦伺服器和存取的資訊。  
  
###  <a name="BKMK_6"></a>網路使用者名稱和密碼資訊  
 從管理您的伺服器的人員，您可以取得您網路使用者名稱和密碼的資訊。 您可以使用這些認證從伺服器連接您的電腦伺服器和存取的資訊。 

  
 如果您是系統管理員 server，您可以建立網路認證新增的使用者帳號**使用者**儀表板中的索引標籤。 如需帳號，查看[管理使用儀表板的使用者帳號](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)。  
  
###  <a name="BKMK_7"></a>伺服器的管理員  
 您必須提供系統管理員以來的網路名稱和密碼，才能安裝連接器軟體。 網路系統管理員的 account 可讓使用者以管理您的組織所管理的區域網路，並協助管理和維持網路的裝置，例如與路由器。  
  
 使用的網路系統管理員帳號可執行的工作可包含︰  
  
-   安裝網路應用程式和其他軟體  
  
-   管理伺服器上的儲存空間  
  
-   散發的軟體更新  
  
-   執行日常備份  
  
-   網路上的監視日常活動  
  
 在 Windows Server Essentials、Windows Server Essentials，以及已安裝 Windows Server Essentials 體驗角色 Windows Server 2012 R2，您可以指定的網路系統管理員任何使用者以來層級的存取。 這會授與所需的權限，才能執行網路系統管理員工作。 當使用者指派的網路系統管理員存取的等級，**使用者存取控制**提示開啟任何需要系統管理員權限的工作。  
  
###  <a name="BKMK_8"></a>移除 Windows 網域的電腦  
 若要移除它的網域的電腦，您將會提示輸入使用者名稱和密碼核對。  
  
##### <a name="to-remove-a-computer-from-a-windows-domain"></a>若要將電腦從 Windows 網域移除  
  
1.  按一下**[開始]**，以滑鼠右鍵按一下**電腦**，然後按一下 [**屬性**。  
  
2.  在**電腦名稱、網域及工作群組設定**，按一下 [**變更設定**。  
  
    > [!NOTE]
    >  如果系統提示您的系統管理員密碼或進行確認，請輸入密碼網域，或提供確認。  
  
3.  在 [**系統屬性**對話方塊中，按一下 [**電腦名稱**索引標籤，然後按一下 [**變更**。  
  
4.  中**電腦網域名稱/變更**對話方塊的**的成員**、按一下**群組**、然後執行下列其中之一︰  
  
    1.  若要加入現有的工作群組中，輸入您想要加入，然後按一下 [群組的名稱**確定**。  
  
    2.  若要建立工作群組中，輸入您想要建立，然後按一下 [群組的名稱**確定**。  
  
        > [!NOTE]
        >  您的電腦將會從之網域中移除，您的網域的電腦帳號將加以關閉。  
  
##  <a name="BKMK_B"></a>伺服器連接的電腦使用軟體連接器  
 本章節提供程序和資訊可協助您安裝的連接器軟體、的伺服器來連接您的電腦，以及疑難排解伺服器連接電腦的存取權。  
  

-   [伺服器連接的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [連接 Windows Server Essentials 伺服器未加入網域的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [安裝連接器軟體](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [將電腦的資料，並設定手動](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [在電腦部署傳輸多的使用者設定檔](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [解除安裝的連接器軟體](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [中斷連接您的電腦或重新連接您的電腦 server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [如何備份運作的睡眠和休眠模式](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [伺服器連接的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [連接 Windows Server Essentials 伺服器未加入網域的電腦](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [安裝連接器軟體](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [將電腦的資料，並設定手動](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [在電腦部署傳輸多的使用者設定檔](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [解除安裝的連接器軟體](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [中斷連接您的電腦或重新連接您的電腦 server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [如何備份運作的睡眠和休眠模式](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

  
###  <a name="BKMK_9"></a>伺服器連接的電腦  
 當您正在執行 Windows Server Essentials 伺服器或 Windows Server 2012 R2 連接的電腦已安裝 Windows Server Essentials 體驗角色時，請確定您 client 的電腦已正確連接到網際網路。  
  
 完成所有 client 電腦，將它們連接到您的伺服器上以下程序。  
  
 若要完成此程序，您需要下列資訊︰  
  
-   使用者名稱和密碼會使用該電腦上新的網路的人  
  
-   使用者名稱和密碼電腦 s 本機管理員  
  
> [!NOTE]

>  在 Windows Server Essentials 或 Windows Server Essentials 先 client 的部署，您可以而不將它們新增到 Windows Server Essentials 網域的電腦連接到伺服器。 這個法不適用於所有支援的 client 作業系統和功能，例如 [群組原則和 virtual 私人網路 (Vpn)，這需要在連接電腦時，會網域，已無法使用。 需求和指示進行，查看[連接到 Windows Server Essentials 伺服器的電腦未加入網域](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  

>  在 Windows Server Essentials 或 Windows Server Essentials 先 client 的部署，您可以而不將它們新增到 Windows Server Essentials 網域的電腦連接到伺服器。 這個法不適用於所有支援的 client 作業系統和功能，例如 [群組原則和 virtual 私人網路 (Vpn)，這需要在連接電腦時，會網域，已無法使用。 需求和指示進行，查看[連接到 Windows Server Essentials 伺服器的電腦未加入網域](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。  

  
##### <a name="to-connect-a-client-computer-to-the-server"></a>若要 client 電腦連接到 server  
  
1.  登入至您想要伺服器連接的電腦。  
  
    > [!NOTE]
    >  如果此電腦有多個帳號，使用使用者帳號的文件、圖片及個人您想要保留之後，您將電腦連接到 server 的喜好設定來登入。  
  
2.  打開網際網路瀏覽器，例如 Internet Explorer。  
  
3.  在 [位址列中，輸入**http://<servername\>/Connect**，然後按 Enter 鍵。  
  
    > [!NOTE]
    >  如果您的電腦位於遠端和 Windows Server Essentials 網路位置，若要執行連接電腦伺服器精靈中，輸入**http://<domainname\>/connect**網址列中的網頁瀏覽器（< domain\ > 是您的組織所管理的網域名稱）。 您可以從您的網路系統管理員取得您的網域名稱資訊。  
  
4.  **伺服器連接您的電腦**頁面隨即顯示。 執行下列其中一個動作：  
  
    -   針對執行 Windows 作業系統的電腦，按一下 [**下載適用於 Windows 的軟體**。  
  
    -   適用於電腦的執行 Mac OS X 或更新版本中，按一下 [**下載 Mac 軟體**。  
  
5.  在 [檔案下載安全性警告訊息，按一下 [**執行**。  
  
6.  如果出現使用者以來控制項訊息，請按一下 [**是**或畫面出現提示時輸入當地的使用者名稱和密碼]。  
  
7.  連接電腦的伺服器精靈會出現。 執行下列步驟來完成精靈︰  
  
    1.  接受授權合約。  
  
    2.  在 [**尋找我的伺服器**首頁，自動偵測到的區域網路中，選取您想要連接到伺服器。 或者，如果您有資訊，您可以手動輸入伺服器的網域或地址。  
  
    3.  在 [**輸入您的新的網路使用者名稱和密碼**頁面上，執行下列動作︰  
  
        -   如果這是您連接至伺服器，第一部電腦，這是使用您管理伺服器的電腦，使用您在設定期間建立管理員。  
  
        -   所有其他電腦，第一次建立網路使用者的 account 伺服器上使用儀表板。 以系統管理員或標準建立帳號使用者權限，根據來使用電腦的人所執行的工作。  
  
    4.  如果您的電腦執行 Windows 8、Windows 8.1 或 Windows 10，請略過此步驟。 如果您的電腦執行 Windows 7 中，而且如果您有文件，圖片或個人喜好設定（例如桌面背景、螢幕保護裝置或 Internet Explorer 我的最愛）您想要保留之後，您將電腦加入到新的網路上**選擇是否要將現有資料與設定**頁面精靈中，選取**將我的資料及設定移至我新的網路帳號**。  
  
    5.  選擇您要自動喚醒電腦上建立的備份**選擇是否要建立的備份到此電腦喚醒**頁面。  
  
    6.  請依照精靈中的其餘指示加入您的電腦在網路。  
  
8.  您可以加入您的電腦在網路之後，使用您新的使用者名稱和密碼來登入電腦。  
  
    > [!NOTE]
    >  當您的電腦執行的 Windows 8 的第一次伺服器連接之後，請使用您的網路帳號，登入時，則會出現移轉的檔案，並從舊的使用者帳號應用程式的指示。 依照指示上**如何執行我的檔案和應用程式從移轉我舊帳號？**移轉的所有檔案與網路使用者的 account 應用程式] 頁面。  
  
9. 成功伺服器連接電腦之後，連接器 TrayApp 和伺服器儀表板快速鍵出現在 [開始] 功能表，可以會使用，如下所示（如果您的電腦執行 Windows 8、Windows 8.1 或 Windows 10、儀表板及連接器 TrayApp 就可以從電腦的開始畫面）︰  
  
    -   如果您的電腦執行 Windows 8、Windows 8.1 或 Windows 10，儀表板及連接器 TrayApp 將會搜尋與應用程式。  
  
    -   您可以從連接器 TrayApp，讓或停用**讓我從遠端連接**功能。 您也可以按兩下開始 Launchpad TrayApp。 從 Launchpad，可以存取的共用資料夾快速鍵、設定的備份的電腦，位址警示，與打開遠端網站存取的網站。  
  
    -   從**儀表板**連結，您可以使用管理您的伺服器。  
  
###  <a name="BKMK_10"></a>連接 Windows Server Essentials 伺服器未加入網域的電腦  
 此主題告訴您如何加入 Windows 7、Windows 8、Windows 8.1 或 Windows 10 的電腦不需要將電腦加入 Windows Server Essentials 網域先 client 部署 Windows Server Essentials 網路。 在 Windows Server Essentials 與 Windows Server Essentials 支援此連接的方法。  
  
 這是另一個常見的方法，需要加入 Windows Server Essentials 網域的電腦。 方法，以如果電腦是在另一部網域中，就必須先移除從該網域加入 Windows Server Essentials 網域。  
  
#### <a name="feature-limits"></a>功能限制  
 Windows Server Essentials 網域未加入 client 電腦時，某些功能僅限︰  
  
-   所有的功能需要能加入網域的電腦？包括網域認證、群組原則和 virtual 私人網路 (Vpn)？已無法使用。  
  
-   任何第三方附加元件，必須能加入網域的電腦將無法正常運作。  
  
-   這個方法無法用來伺服器連接功能場所的電腦。  
  
#### <a name="prerequisites"></a>必要條件  
  
-   電腦必須配備具有實體區域網路來連接。  
  
-   必須在電腦上安裝下列作業系統其中一項︰  
  
    -   Windows 10 專業版、Windows 10 企業版  
  
    -    Windows 8.1 專業版、Windows 8.1 企業版、Windows 8 專業版、Windows 8 企業版  
  
    -    Windows 7 專業（x86 及 x64）、Windows 7 企業版（x86 及 x64）、Windows 7 旗艦版（x86 及 x64）  
  

-   電腦必須符合在 Windows Server Essentials client 電腦的所有其他的需求。 如需詳細資訊，請查看[伺服器連接電腦的必要條件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)。  

  
-   未加入網域，致力於連接功能，您必須登入的電腦上為 [本機的成員。  
  
-   Windows Server Essentials 伺服器連接的電腦，您將需要帳號下列資訊︰  
  
    -   在 Windows Server Essentials（洽詢）帳號以系統管理員權限。  
  
    -   使用者名稱和密碼會使用該電腦的人員核對。 核對也必須系統管理員權限在 Windows Server Essentials 伺服器上。  
  
#### <a name="connect-to-a-windows-server-essentials-network"></a>Windows Server Essentials 網路來連接  
 確認已符合所有必要條件後，請在 Windows Server Essentials 網路連接電腦。  
  
###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Windows Server Essentials 網路來連接在不同的網域的電腦  
  
1.  登入 client 電腦以是本機的成員。  
  
2.  打開以系統管理員權限的命令提示字元。  
  
    -   在 Windows 10 中，按一下 [**開始**按鈕，選取**所有應用程式** -> **Windows 系統工具** -> **命令提示字元**命令提示字元中，按一下滑鼠右鍵，然後選擇**以系統管理員身分執行**。  
  
    -   在 Windows 8 上**開始**頁面上，輸入**命令**，然後按 Enter。 在結果中，請以滑鼠右鍵按一下**命令提示字元**，然後按一下 [**以系統管理員身分執行**。  
  
    -   在 Windows 7 上**開始**功能表中，輸入**命令**的搜尋方塊中，請以滑鼠右鍵按一下**命令提示字元**，，然後按一下 [**以系統管理員身分執行**。  
  
3.  在命令提示字元中，輸入下列命令，並按一下 Enter:  
  
    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  
  

4.  完成中的步驟[電腦連接到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

  
####  <a name="BKMK_SecondServer"></a>第二個伺服器加入該網路  
  
###### <a name="to-join-a-second-server-to-the-network"></a>第二個伺服器加入該網路  
  
1.  登入至您想要在 Windows Server Essentials 網路連接的伺服器。  
  
2.  打開網際網路瀏覽器，並在 [位址列中，輸入**http://<servername\>/Connect**，其中*< servername\ >*是執行 Windows Server Essentials，伺服器的名稱，然後按 Enter。  
  
3.  如果您嘗試在 Windows Server Essentials 網路連接、完成動作; 的伺服器上為支援的 Internet Explorer 增強安全性設定或者，略過此步驟。  
  
    1.  若要接受封鎖訊息，請按一下 [**關閉**。  
  
    2.  新增**http://<servername\>/Connect**網站，以受信任的網站，如下所示︰  
  
        1.  在瀏覽器瀏覽窗格中，按一下 [**工具**，然後按一下 [**網際網路選項**。  
  
        2.  按一下 [**安全性**索引標籤，然後按一下 [**信任的網站**。  
  
        3.  按一下 [**網站**。  
  
        4.  應該會顯示網站**將這個網站新增到區域**欄位。 按一下**新增**。  
  
        5.  按一下**關閉**，然後按**[確定]**。  
  
    3.  重新整理網頁。  
  

    4.  若要連接到執行 Windows Server Essentials 的伺服器的第二個伺服器，請依照[電腦連接到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  

  
    > [!NOTE]
    >  連接到執行 Windows Server Essentials 的伺服器的第二個伺服器不同連接到電腦 client，如下所示︰  
    >   
    >  -   還有不的使用者設定檔移轉。因此，將不會移轉目前的設定檔。  
    > -   您無法使用 client 電腦備份，備份第二個伺服器，並不會有選項來喚醒電腦上的備份。  
  
 您執行 Windows Server Essentials 的伺服器來加入第二個伺服器之後，連接的伺服器會提供下列功能︰  
  
-   更新與功能狀態，通知運作的其他電腦 client 一樣。  
  
-   Online 與離線功能運作的其他電腦 client 一樣。  
  
-   您可以使用遠端 Web 存取連接到第二個伺服器。  
  
-   第二個的伺服器會連同健康報告，因為 Windows Server Essentials 將會產生伺服器相關通知。  
  
 將會與伺服器執行 Windows Server Essentials 的第二個伺服器管理不同管理 client 的其他電腦，如下所示︰  
  
-   將會有任何 TrayApp、Launchpad，以及儀表板 Client 的進入點。  
  
-   中列出的第二個伺服器**伺服器**群組上**裝置**] 索引標籤。  
  
-   備份狀態因為 client 的備份的電腦不支援的第二個伺服器，會顯示為**不支援**。 此外，如果您選擇的第二個伺服器和以滑鼠右鍵按一下，有備份及還原相關顯示為第二個伺服器的工作。  
  
-   如果您選擇的第二個的伺服器，然後按一下**檢視伺服器屬性**工作，有任何**備份**伺服器的屬性頁面顯示] 索引標籤。  
  
-   因為 Windows Server 作業系統不安全中心」，以顯示的第二個伺服器的安全性狀態**不適用**。  
  
-   以顯示的第二個伺服器的群組原則狀態**不適用**。  
  
###  <a name="BKMK_11"></a>安裝連接器軟體  
 當使用電腦連接到 [伺服器] 精靈伺服器連接您的電腦安裝在 Windows Server Essentials 的連接器軟體。 您可以透過輸入推出此精靈**http://<ServerName\>/connect**位址列中的網頁瀏覽器 (位置*< ServerName\ >*伺服器的名稱)。  
  
> [!NOTE]
>  如果您的電腦位於遠端位置時，若要執行連接電腦伺服器精靈中，輸入**http://<domainname\>/connect**網址列中的網頁瀏覽器 (位置*< domain\ >*是您的組織所管理的網域名稱)。 您可以從您的網路系統管理員取得您的網域名稱資訊。  
  
 連接器軟體會執行下列動作︰  
  
-   Windows Server Essentials 連接您的電腦  
  
-   會自動備份您的電腦夜間（如果您設定的伺服器來建立備份，client）  
  
-   協助監視您的電腦健康系統管理員  
  
-   可讓您設定，以及 Windows Server Essentials 遠端管理您的家用電腦  
  

 逐步有關連接您的電腦與 Windows Server Essentials 伺服器，查看[電腦連接到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。   

  
###  <a name="BKMK_12"></a>將電腦的資料，並設定手動  
  Windows Server Essentials 與 Windows Server Essentials 支援使用者設定檔移轉只 client 電腦正在執行 Windows 7 的作業系統。 在 Windows 7 電腦連接到伺服器，[伺服器] 精靈連接的電腦可以自動移轉的使用者設定檔。  
  
 伺服器連接 Windows 8、Windows 8.1 或 Windows 10 電腦時，無法自動傳輸的使用者設定檔。 不過，Windows 8 電腦上，您可以使用 Windows 輕鬆傳輸原始本機使用者傳送的資料，並設定加入網域的電腦。 若要這樣做，您必須是 Windows 8 的來源電腦和 Windows 8 的目的地電腦上系統管理員的身分。 使用 Windows 輕鬆傳輸傳輸檔案與設定的相關資訊，請查看[文章 2735227](https://support.microsoft.com/kb/2735227) Microsoft 知識庫中。  
  
###  <a name="BKMK_Transfer"></a>在電腦部署傳輸多的使用者設定檔  
 在連接的電腦執行的 Windows 7 或 Windows 7 SP1 作業系統 Windows Server Essentials 伺服器之前，以傳輸多當地的使用者設定檔，您必須先建立對應的網路使用者帳號伺服器上。 如需帳號建立網路的相關詳細資訊，請查看[新增帳號](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。  
  
 執行 Windows 7（適用於 Windows Server Essentials) 或 Windows 7 SP1（適用於 Windows Server Essentials) 的電腦上只支援使用者設定檔移轉。 當您使用連接電腦的伺服器精靈與 Windows Server Essentials 伺服器連接電腦時，您便被提供的選項，來移動到新的網路使用者帳號的使用者資料與舊本機帳號的設定。 若要這樣做，在**將現有的使用者資料和設定**頁面精靈的對應的網路使用者帳號傳輸多的使用者設定檔 client 電腦上的電腦上存在之本機使用者帳號。  
  
###  <a name="BKMK_13"></a>解除安裝的連接器軟體  
 您可以使用 [控制台] 解除安裝的連接器軟體的電腦。 您將會通常這樣的連接器軟體問題時，或如果您需要安裝較新版本的連接器軟體。 您必須登入電腦以系統管理員，若要完成此程序。  
  
> [!IMPORTANT]
>  如果您升級 client 的電腦上的作業系統，連接器軟體會自動解除安裝。 升級完成後，您必須重新安裝軟體連接器。 最好是解除安裝的連接器軟體，先升級作業系統。 升級完成後，請解除安裝的連接器軟體是仍可接受。不過，可能會造成一致伺服器的 client 電腦之前的連接器軟體解除安裝或重新安裝。  
  
##### <a name="to-uninstall-connector-software-from-a-computer"></a>若要從電腦解除安裝連接器軟體  
  
1.  從電腦是執行 Windows 7、Windows 8、Windows 8.1 或 Windows 10，請打開**控制台**，然後在**程式**區段中，按一下 [**檢視安裝的更新**。  
  
2.  在已安裝的程式] 清單，選取 [ **Windows Server Essentials 連接器**，然後按一下 [**解除安裝**。  
  
3.  在警告頁面上，按一下 [**是**。  
  
4.  如果**使用者以來控制項**視窗中，按一下**允許**。  
  
5.  在 Windows Server Essentials，如果關閉 Launchpad 提出 Windows Server Essentials 連接器網頁，按一下**確定**。  
  
6.  若要解除安裝程式等。 移除該軟體之後，**Windows Server Essentials 連接器**不會再出現在已安裝的程式或更新的清單。 此外，Launchpad 和儀表板的快速鍵都不會再顯示電腦 s 桌面上。  
  
> [!NOTE]
>  -   解除安裝的連接器軟體不會移除在電腦顯示的電腦清單**裝置**儀表板中的索引標籤。 若要將電腦從儀表板中移除，請查看[電腦移除伺服器](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)。  
> -   當您在解除安裝的連接器軟體時，將不會刪除 client 電腦上已伺服器對應的共用的資料夾。 您必須手動 delete 伺服器對應的共用的資料夾。  

> -   解除安裝的連接器軟體的影響不做解除加入原始網域的電腦。 您必須手動解除加入網域的電腦。 指示，請查看[將電腦從 Windows 網域移除](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)。  

  
###  <a name="BKMK_14"></a>中斷連接您的電腦或重新連接您的電腦 server  
 若要中斷伺服器的電腦，您必須先完成下列步驟︰  
  

1.  解除安裝連接器軟體的電腦上使用 [控制台]。 如逐步[解除安裝的連接器軟體](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。   

  
2.  取消加入 Windows Server Essentials 網域的電腦，而且加入該群組。 適用於 Windows 加入工作群組中，逐步[加入或工作群組的建立](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup)。  
  
3.  移除伺服器的電腦使用儀表板。 逐步，查看[電腦移除伺服器](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)。  
  
 若要重新連接到先前的 Windows Server Essentials 伺服器網路中斷連接伺服器電腦時，您必須先完成下列步驟︰  
  

1.  解除安裝連接器軟體的電腦上使用 [控制台]。 如逐步[解除安裝的連接器軟體](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。  
  
2.  取消加入 Windows Server Essentials 網域的電腦，而且加入該群組。 適用於 Windows 加入工作群組中，逐步[加入或工作群組的建立](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup)。  
  
3.  使用連接電腦精靈伺服器連接的電腦。 逐步，查看[電腦連接到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
###  <a name="BKMK_Sleep"></a>如何備份搭配睡眠和休眠模式  
 如果您選擇**喚醒這台電腦上的備份**選項伺服器，電腦連接電腦時將會自動從睡眠狀態喚醒或休眠模式中的備份排程指定每天，讓它可以備份。 備份完成之後，將會返回電腦進入睡眠或休眠模式，根據其電源管理設定。 如果您選取此選項，伺服器將會備份的電腦是否電腦進入睡眠或休眠。 如需詳細資訊，[管理 Client 備份](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)。  
  
##  <a name="BKMK_C"></a>使用 Launchpad  
 您可以使用 Launchpad 存取共用的資源從 Windows Server Essentials 伺服器、備份的電腦，並回應系統健康警示。  
  
-   [Launchpad 概觀](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  
  
-   [使用 Launchpad Mac 的電腦](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
## <a name="see-also"></a>也了  
  
-   [疑難排解伺服器連接的電腦](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  
  
-   [管理使用者帳號](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  

-   [使用共用資料夾](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [從遠端使用](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [播放數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [從遠端使用](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

