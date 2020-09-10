---
title: 在 Windows Server Essentials 中建立連線
description: 說明如何使用 Windows Server Essentials
ms.date: 05/07/2016
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ef0cdcffb90ba14eca6ccff50e4a297662bd61c6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624977"
---
# <a name="get-connected-in-windows-server-essentials"></a>在 Windows Server Essentials 中建立連線

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 您可以使用連線程式軟體將電腦連線到 Windows Server Essentials 伺服器。 當您使用 [將電腦連線到伺服器精靈] 將電腦連線到伺服器時，就會安裝連線程式軟體。 您可以藉由輸入**HTTP://<servername \> /connect**來啟動精靈，其中 **<\> servername**是您伺服器的名稱。

 本主題內容：


-   [準備將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)

-   [使用連線程式軟體將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)

-   [使用啟動列](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)

-   [準備將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)

-   [使用連線程式軟體將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)

-   [使用啟動列](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)


##  <a name="prepare-to-connect-computers-to-the-server"></a><a name="BKMK_A"></a> 準備將電腦連線到伺服器
 本節討論連線程式軟體、Windows Server Essentials 支援的作業系統、將電腦連線到伺服器之前必須先完成的先決條件工作，以及執行連線程式軟體時伺服器對電腦所做的變更。


-   [連線程式軟體概觀](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)

-   [將電腦連線到伺服器的必要條件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)

-   [將 Mac 電腦連線到伺服器的先決條件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)

-   [支援用戶端電腦的作業系統](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)

-   [伺服器對用戶端電腦所做的變更](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)

-   [網路使用者名稱和密碼資訊](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)

-   [伺服器管理員的帳戶](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)

-   [將電腦從 Windows 網域中移除](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)

###  <a name="connector-software-overview"></a><a name="BKMK_1"></a> 連接器軟體總覽
 Windows Server Essentials 作業系統的連線程式軟體可將您網路中的電腦連線到 Windows Server Essentials 伺服器。 將電腦連線到伺服器時，連線程式軟體可讓您自動備份電腦並監視其健康情況。 連線程式軟體也可讓您設定及從遠端管理 Windows Server Essentials 伺服器。 當您將用戶端電腦連線到伺服器時，就會安裝連線程式軟體。 如需有關將用戶端電腦連線到 Windows Server Essentials 伺服器的詳細指示，請參閱本主題中稍後的[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。

-   [連線程式軟體概觀](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)

-   [將電腦連線到伺服器的必要條件](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)

-   [將 Mac 電腦連線到伺服器的先決條件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)

-   [支援用戶端電腦的作業系統](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)

-   [伺服器對用戶端電腦所做的變更](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)

-   [網路使用者名稱和密碼資訊](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)

-   [伺服器管理員的帳戶](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)

-   [將電腦從 Windows 網域中移除](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)

###  <a name="connector-software-overview"></a><a name="BKMK_1"></a> 連接器軟體總覽
 Windows Server Essentials 作業系統的連線程式軟體可將您網路中的電腦連線到 Windows Server Essentials 伺服器。 將電腦連線到伺服器時，連線程式軟體可讓您自動備份電腦並監視其健康情況。 連線程式軟體也可讓您設定及從遠端管理 Windows Server Essentials 伺服器。 當您將用戶端電腦連線到伺服器時，就會安裝連線程式軟體。 如需有關將用戶端電腦連線到 Windows Server Essentials 伺服器的詳細指示，請參閱本主題中稍後的[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。


###  <a name="prerequisites-for-connecting-a-computer-to-the-server"></a><a name="BKMK_2"></a> 將電腦連接到伺服器的必要條件
 將電腦連線到網路之前，必須先符合下列需求：

-   已完成 Windows Server Essentials 安裝且伺服器正在執行。 如果連線程式軟體無法與伺服器通訊，就會結束連線程式軟體安裝。


-   用戶端電腦所執行的是支援的作業系統。 如需詳細資訊，請參閱[支援的用戶端電腦作業系統](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)。


-   用戶端電腦必須具有有效的網際網路連線。

-   當用戶端電腦所在的網路與執行 Windows Server Essentials 的伺服器相同時，用戶端電腦所在的 IP 子網路也與該伺服器相同。

-   用戶端電腦上已安裝 .NET Framework 4.5。 連線程式軟體會自動將它安裝在電腦上。

-   用戶端電腦符合下列最低系統需求：

    -   1.4 GHz 或更快的處理器

    -   1 GB RAM 或更多

    -   1 GB 的可用硬碟空間 (安裝之後將會釋出此空間的一部分)

-   開機磁碟分割 (也就是 Windows 作業系統安裝所在的磁碟分割) 是以 NTFS 檔案系統格式化。

-   電腦名稱長度不超過 15 個字元。

-   電腦名稱不包含底線 (_)。

-   電腦的日期和時間設定會與伺服器上的設定一致。

-   在任何指定時間，用戶端電腦都只能連線到一部 Windows Server Essentials 伺服器。

-   已經加入另一個 Active Directory 網域的用戶端電腦無法再加入 Windows Server Essentials 網域。

> [!NOTE]
>
>  在適用于 Windows Server Essentials 或 Windows Server Essentials 的內部部署用戶端部署中，您可以將電腦連線到伺服器，而不需要將它們新增到 Windows Server Essentials 網域。 並非所有支援的用戶端作業系統都可以使用這個方法，而且群組原則和虛擬私人網路 (VPN) 這類需要將電腦連線到網域的功能也無法供使用。 如需相關需求和指示，請參閱[在不加入網域的情況下將電腦連線到 Windows Server Essentials 伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。

 如需將電腦連線到執行 Windows Server Essentials 之伺服器的逐步指示，請參閱[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。

>  在適用于 Windows Server Essentials 或 Windows Server Essentials 的內部部署用戶端部署中，您可以將電腦連線到伺服器，而不需要將它們新增到 Windows Server Essentials 網域。 並非所有支援的用戶端作業系統都可以使用這個方法，而且群組原則和虛擬私人網路 (VPN) 這類需要將電腦連線到網域的功能也無法供使用。 如需相關需求和指示，請參閱[在不加入網域的情況下將電腦連線到 Windows Server Essentials 伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。

 如需將電腦連線到執行 Windows Server Essentials 之伺服器的逐步指示，請參閱[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。


###  <a name="prerequisites-for-connecting-a-mac-computer-to-the-network"></a><a name="BKMK_3"></a> 將 Mac 電腦連接到網路的必要條件
 將 Mac 電腦連線到網路之前，必須先符合下列需求：

-   已完成伺服器作業系統安裝且伺服器正在執行。 如果連線程式軟體無法與伺服器通訊，就不會安裝連線程式軟體。

-   電腦執行 Mac OS X 10.5 (Leopard) 或更新版本。

-   電腦與伺服器位於相同的 IP 子網路上。

-   電腦必須具有有效的網際網路連線。

-   確定電腦符合下列最低系統需求：

    -   1.4 GHz 或更快的處理器

    -   1 GB RAM 或更多

    -   1 GB 的可用硬碟空間 (安裝之後將會釋出此空間的一部分)

-   在任何指定時間，用戶端電腦都只能連線到一部伺服器。

###  <a name="supported-operating-systems-for-client-computers"></a><a name="BKMK_4"></a> 用戶端電腦支援的作業系統
 Windows Server Essentials 為所有支援的用戶端電腦都提供一組相同的功能。 這些功能包括 [網域加入]、[啟動列] 及用戶端健康情況通知。

> [!IMPORTANT]
>  Windows Server Essentials 不支援將執行 Home、Starter 或 Media Center 版本的 Windows 電腦加入網域。 此外，您也無法使用「遠端 Web 存取」來連線到這些電腦。

#### <a name="windows-server-essentials"></a>Windows Server Essentials
  本節適用于執行 Windows Server Essentials 的伺服器，或是執行 Windows server 2012 R2 Standard 或 Windows server 2012 R2 Datacenter （已安裝 Windows Server Essentials Experience 角色）的伺服器。 支援下列電腦作業系統：

 **Windows 7 作業系統**

- Windows 7 Home Basic SP1 (x86 和 x64) 

- Windows 7 Home Premium SP1 (x86 和 x64) 

- Windows 7 Professional SP1 (x86 和 x64) 

- Windows 7 旗艦版 SP1 (x86 和 x64) 

- Windows 7 企業版 SP1 (x86 和 x64) 

- Windows 7 Starter SP1 (x86) 

  **Windows 8 作業系統**

- Windows 8

- Windows 8 Pro

- Windows 8 企業版

  **Windows 8.1 作業系統**

- Windows 8。1

- Windows 8.1 專業版

- Windows 8.1 Enterprise

  **Windows 10 作業系統**

- Windows 10

- Windows 10 Pro

- Windows 10 Enterprise

- Windows 10 Education

  **Mac 用戶端電腦**

- Mac OS X v10.5 Leopard

- Mac OS X v10.6 Snow Leopard

- Mac OS X v10.7 Lion

- Mac OS X v10.8 Mountain Lion

> [!NOTE]
>  您可以從 Windows Server Essentials 儀表板查看 Mac 電腦的健康情況和備份狀態。 不過，您無法從 [儀表板] 設定電腦備份或啟動備份。 此外，您也無法使用「遠端 Web 存取」來連線到 Mac 電腦。

#### <a name="windows-server-essentials"></a>Windows Server Essentials
  本節適用于執行 Windows Server Essentials 的伺服器。 支援下列電腦作業系統：

 **Windows 7 作業系統**

- Windows 7 Home Basic (x86 和 x64) 

- Windows 7 Home Premium (x86 和 x64) 

- Windows 7 Professional (x86 和 x64) 

- Windows 7 旗艦版 (x86 和 x64) 

- Windows 7 企業版 (x86 和 x64) 

- Windows 7 Starter (x86) 

  **Windows 8 作業系統**

- Windows 8

- Windows 8 Pro

- Windows 8 企業版

  **Windows 10 作業系統**

- Windows 10

- Windows 10 Pro

- Windows 10 Enterprise

- Windows 10 Education

  **Mac 用戶端電腦**

- Mac OS X v10.5 Leopard

- Mac OS X v10.6 Snow Leopard

- Mac OS X v10.7 Lion

> [!NOTE]
>  您可以從 [Windows Server Essentials 儀表板] 檢視 Mac 電腦的健康情況和備份狀態。 不過，您無法從 [儀表板] 設定電腦備份或啟動備份。 此外，您也無法使用「遠端 Web 存取」來連線到 Mac 電腦。

###  <a name="changes-the-server-makes-to-a-client-computer"></a><a name="BKMK_5"></a> 對用戶端電腦進行的伺服器變更
 將電腦連線到伺服器時，Windows Server Essentials 軟體會對電腦做一些變更，以便讓電腦和伺服器可以一起運作。

 這個軟體會執行下列動作：

-   在電腦上安裝連線程式軟體

-   在電腦上安裝 Microsoft .NET Framework 4.5 (如果尚未安裝)

-   在電腦的桌面上建立快捷方式至儀表板和啟動控制板

-   在電腦上設定 Windows 防火牆連接埠以允許下列功能運作：

    -   核心網路功能

    -   遠端桌面服務

-   對電腦進行下列變更以協助進行備份：

    -   建立排定的工作來執行自動備份

    -   安裝管理伺服器之備份操作的服務

    -   安裝在檔案和資料夾還原過程中使用的虛擬裝置驅動程式

-   安裝「健康情況代理程式」來偵測問題和建立對應的警示通知

-   在電腦上建立排定的工作來進行週期性健康情況評估，以及同步健康情況警示定義

-   將服務新增到電腦，供電腦用來與伺服器及其他 Windows Server Essentials 功能進行通訊

-   在執行 Windows 用戶端的用戶端電腦上開啟 TCP 連接埠 3389，以允許執行遠端桌面服務

-   在用戶端電腦上部署 VPN，並在 Windows Server Essentials 上啟用 VPN 功能時提供單鍵體驗，或在 Windows Server Essentials 上啟用 VPN 功能時提供自動連線體驗


 如需有關將您的電腦連線到伺服器的資訊，請參閱[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。

###  <a name="network-user-name-and-password-information"></a><a name="BKMK_6"></a> 網路使用者名稱和密碼資訊
 您可以向管理您伺服器的人員取得您的網路使用者名稱和密碼資訊。 您可以使用這些認證將您的電腦連線到伺服器，以及從伺服器存取資訊。

###  <a name="network-user-name-and-password-information"></a><a name="BKMK_6"></a> 網路使用者名稱和密碼資訊
 您可以向管理您伺服器的人員取得您的網路使用者名稱和密碼資訊。 您可以使用這些認證將您的電腦連線到伺服器，以及從伺服器存取資訊。


 如果您是伺服器系統管理員，您可以從 [儀表板] 的 [使用者]**** 索引標籤新增使用者帳戶來建立網路認證 。 如需有關使用者帳戶的詳細資訊，請參閱[使用儀表板來管理使用者帳戶](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)。

###  <a name="server-administrators-account"></a><a name="BKMK_7"></a> 伺服器管理員的帳戶
 您必須要能夠提供網路系統管理員帳戶名稱和密碼，才能安裝連線程式軟體。 網路系統管理員帳戶可讓使用者管理您組織的區域網路，以及協助管理和維護網路裝置 (例如交換器和路由器)。

 可以使用網路系統管理員帳戶來執行的工作可包括：

- 安裝網路應用程式及其他軟體

- 管理伺服器上的存放裝置

- 散佈軟體更新

- 執行例行備份

- 監視網路上的日常活動

  在已安裝「Windows Server Essentials 體驗」角色的 Windows Server Essentials、Windows Server Essentials 和 Windows Server 2012 R2 中，您可以將「網路系統管理員」存取層級指派給任何使用者帳戶。 這會授與執行網路系統管理員工作所需的權限。 在使用者獲指派網路系統管理員存取層級之後，只要有任何需要系統管理員權限的工作，就會開啟 [使用者存取控制]**** 提示。

###  <a name="remove-a-computer-from-a-windows-domain"></a><a name="BKMK_8"></a> 從 Windows 網域移除電腦
 為了將電腦從其網域中移除，系統會提示您輸入網域帳戶的使用者名稱和密碼。

##### <a name="to-remove-a-computer-from-a-windows-domain"></a>將電腦從 Windows 網域中移除

1.  按一下 [開始]****，在 [電腦]**** 上按一下滑鼠右鍵，然後按一下 [內容]****。

2.  在 [電腦名稱、網域及工作群組設定]**** 底下，按一下 [變更設定]****。

    > [!NOTE]
    >  如果系統提示您輸入系統管理員密碼或確認，請輸入網域密碼或提供確認。

3.  在 [系統內容]**** 對話方塊中，按一下 [電腦名稱]**** 索引標籤，然後按一下 [變更]****。

4.  在 [電腦名稱/網域變更]**** 對話方塊的 [成員隸屬]**** 底下，按一下 [工作群組]****，然後執行下列其中一個動作：

    1.  若要加入現有的工作群組，請輸入您想要加入的工作群組名稱，然後按一下 [確定]****。

    2.  若要建立工作群組，請輸入您想要建立的工作群組名稱，然後按一下 [確定]****。

        > [!NOTE]
        >  您的電腦將會從網域中移除，而您在該網域上的電腦帳戶將會停用。

##  <a name="connect-computers-to-the-server-by-using-the-connector-software"></a><a name="BKMK_B"></a> 使用連接器軟體將電腦連線到伺服器
 本節可讓您存取一些程序和資訊，用來協助您安裝連線程式軟體、將您的電腦連線到伺服器，以及進行電腦與伺服器連線問題疑難排解。


-   [將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)

-   [在不加入網域的情況下將電腦連線到 Windows Server Essentials 伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)

-   [安裝連線程式軟體](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)

-   [手動移動電腦資料和設定](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)

-   [在進行電腦部署時傳輸多個使用者設定檔](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)

-   [將連線程式軟體解除安裝](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)

-   [將您的電腦與伺服器中斷連線或重新連線](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)

-   [備份如何與睡眠和休眠模式搭配運作](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)


###  <a name="connect-computers-to-the-server"></a><a name="BKMK_9"></a> 將電腦連線到伺服器
 當您將電腦連線到執行 Windows Server Essentials 或 Windows Server 2012 R2 且已安裝 Windows Server Essentials 體驗角色的伺服器時，請確定您的用戶端電腦具有有效的網際網路連線。

 請在所有用戶端電腦上完成以下程序，以將這些電腦連線到您的伺服器。

 若要完成這個程序，您需要下列資訊：

-   將使用新網路上電腦之人員的使用者名稱和密碼。

-   電腦本機系統管理員帳戶的使用者名稱和密碼

> [!NOTE]
>
>  在適用于 Windows Server Essentials 或 Windows Server Essentials 的內部部署用戶端部署中，您可以將電腦連線到伺服器，而不需要將它們新增到 Windows Server Essentials 網域。 並非所有支援的用戶端作業系統都可以使用這個方法，而且群組原則和虛擬私人網路 (VPN) 這類需要將電腦連線到網域的功能也無法供使用。 如需相關需求和指示，請參閱[在不加入網域的情況下將電腦連線到 Windows Server Essentials 伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。
>
>  在適用于 Windows Server Essentials 或 Windows Server Essentials 的內部部署用戶端部署中，您可以將電腦連線到伺服器，而不需要將它們新增到 Windows Server Essentials 網域。 並非所有支援的用戶端作業系統都可以使用這個方法，而且群組原則和虛擬私人網路 (VPN) 這類需要將電腦連線到網域的功能也無法供使用。 如需相關需求和指示，請參閱[在不加入網域的情況下將電腦連線到 Windows Server Essentials 伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)。


##### <a name="to-connect-a-client-computer-to-the-server"></a>將用戶端電腦連線至伺服器

1.  登入至您想要連線至伺服器的電腦。

    > [!NOTE]
    >  如果此電腦有多個使用者帳戶，請在將此電腦連線到伺服器之後，使用您想要保留其文件、圖片及個人喜好設定的使用者帳戶進行登入。

2.  開啟網際網路瀏覽器，例如 Internet Explorer。

3.  在網址列中，輸入 **HTTP://<servername \> /Connect**，然後按 enter。

    > [!NOTE]
    >  如果您的電腦位於 Windows Server Essentials 網路之外的遠端位置，若要執行 [將電腦連線到伺服器] 嚮導，請在網頁流覽 (器的網址列中輸入 **HTTP://<domainname \> /connect** ，其中 <網域 \> 是您組織) 的功能變數名稱。 您可以向您的網路系統管理員取得您的網域名稱資訊。

4.  **[將電腦連線到伺服器]** 頁面就會出現。 執行下列其中一個動作：

    -   在執行 Windows 作業系統的電腦上，按一下 [下載 Windows 適用的軟體]****。

    -   在執行 Mac OS X 或以上版本的電腦上，按一下 [下載 Mac 適用的軟體]****。

5.  在檔案下載安全性警告訊息中，按一下 **[執行]**。

6.  如果出現 [使用者帳戶控制] 訊息，按一下 [是]****，或在出現提示時輸入本機使用者名稱及密碼。

7.  [將電腦連線到伺服器精靈] 就會出現。 執行下列作業來完成精靈：

    1.  接受使用者授權合約。

    2.  在 [尋找我的伺服器]**** 頁面上，自動偵測區域網路中的伺服器，然後選取您想要連線的伺服器。 或者，如果您有此資訊，您可以手動輸入您伺服器的名稱或網域位址。

    3.  在 **[輸入您的新網路使用者名稱和密碼]** 頁面上，執行下列動作︰

        -   如果這是您要連線到伺服器的第一部電腦，而且您將使用此電腦來管理伺服器，請使用您在安裝期間建立的系統管理員帳戶。

        -   針對所有其他電腦，先使用儀表板在伺服器上建立網路使用者帳戶。 根據使用電腦之使用者執行的工作而定，建立具備系統管理員或標準使用者權限的使用者帳戶。

    4.  如果您的電腦正在執行 Windows 8、Windows 8.1 或 Windows 10，請略過此步驟。 如果您的電腦執行 Windows 7，而且您希望將電腦加入新網路之後保留文件、圖片或個人喜好設定 (例如桌面背景、螢幕保護程式或 Internet Explorer 我的最愛)，請在精靈的 [選擇您是否要移動現有資料及設定] **** 頁面上，選取 [將資料及設定移動至我的新的網路使用者帳戶] ****。

    5.  在 [選擇您是否要喚醒這部電腦來建立備份]**** 頁面上選擇是否想要自動喚醒此電腦建立備份。

    6.  遵循精靈中的其餘指示，將您的電腦加入網路。

8.  將電腦加入網路後，使用您新的使用者名稱及密碼登入電腦。

    > [!NOTE]
    >  在執行 Windows 8 的電腦連線到伺服器後，第一次使用您的網路帳戶登入電腦時，會出現從舊的使用者帳戶移轉檔案和應用程式的指示。 請遵循 **[我要如何從我的舊使用者帳戶移轉檔案和應用程式?]** 頁面的指示，將所有檔案和應用程式移轉到網路使用者帳戶。

9. 電腦順利連線到伺服器之後，連接器 TrayApp 和伺服器儀表板的快捷方式會出現在 [開始] 功能表上，您可以如下所示 (如果您的電腦正在執行 Windows 8、Windows 8.1 或 Windows 10，則會從電腦的開始畫面) 取得儀表板和連接器 TrayApp：

    -   如果您的電腦正在執行 Windows 8、Windows 8.1 或 Windows 10，儀表板和連接器 TrayApp 將可作為應用程式進行搜尋。

    -   從 [連線程式] 系統匣應用程式，您可以啟用或停用 [維持遠端連線]**** 功能。 您也可以按兩下系統匣應用程式來啟動 [啟動列]。 從 [啟動列]，您可以存取 [共用資料夾] 捷徑、設定電腦備份、處理警示，以及開啟「遠端 Web 存取」網站。

    -   從 [儀表板]**** 連結，您可以管理您的伺服器。

###  <a name="connect-computers-to-a-windows-server-essentials-server-without-joining-the-domain"></a><a name="BKMK_10"></a> 在未加入網域的情況下將電腦連線到 Windows Server Essentials 伺服器
 本主題說明如何將 Windows 7、Windows 8、Windows 8.1 或 Windows 10 電腦新增到 Windows Server Essentials 網路，而不需要在內部部署用戶端部署中將電腦加入 Windows Server Essentials 網域。 Windows Server Essentials 和 Windows Server Essentials 支援此連接方法。

 這是需要將電腦加入 Windows Server Essentials 網域之一般方法的替代方法。 使用該方法時，如果電腦位於另一個網域中，就必須先將它從該網域中移除，才能將它加入 Windows Server Essentials 網域中。

#### <a name="feature-limits"></a>功能限制
 當用戶端電腦沒有加入 Windows Server Essentials 網域時，有些功能會受到限制：

-   需要電腦加入網域的所有功能（包括網域認證、群組原則和虛擬私人網路 (Vpn) ）都無法使用。

-   任何需要電腦加入網域的協力廠商附加元件都將無法正常運作。

-   這個方法不能用來將外部部署電腦連線到伺服器。

#### <a name="prerequisites"></a>必要條件

-   電腦必須實際連線到區域網路。

-   電腦上必須安裝下列其中一種作業系統：

    -   Windows 10 專業版、Windows 10 企業版

    -    Windows 8.1 Pro、Windows 8.1 企業版、Windows 8 Pro、Windows 8 企業版

    -    Windows 7 Professional (x86 和 x64) 、Windows 7 企業版 (x86 和 x64) 、Windows 7 旗艦版 (x86 和 x64) 


-   電腦必須符合 Windows Server Essentials 中用戶端電腦的所有其他需求。 如需詳細資訊，請參閱[將電腦連線到伺服器的先決條件](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)。


-   若要能夠在不加入網域的情況下進行連線，您必須使用本機 Administrators 群組成員的帳戶來登入電腦。

-   若要將電腦連線到 Windows Server Essentials 伺服器，您需要下列帳戶資訊：

    -   Windows Server Essentials 上具有系統管理員權限的帳戶 (您的帳戶)。

    -   將使用電腦之人員的網域帳戶使用者名稱和密碼。 網域帳戶也必須具有 Windows Server Essentials 伺服器上的系統管理員權限。

#### <a name="connect-to-a-windows-server-essentials-network"></a>連線到 Windows Server Essentials 網路
 在您確認已符合所有先決條件之後，請將電腦連線到 Windows Server Essentials 網路。

###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>將不同網域中的電腦連線到 Windows Server Essentials 網路

1.  使用本機 Administrators 群組成員的帳戶來登入用戶端電腦。

2.  以系統管理員權限開啟命令提示字元。

    -   在 Windows 10 中，按一下 [**開始**] 按鈕，選取 [**所有應用程式**]  ->  **Windows 系統工具**  ->  **命令提示**字元，以滑鼠右鍵按一下命令提示字元，然後按一下 [以**系統管理員身分執行**]。

    -   在 Windows 8 的 [ **開始** ] 頁面上輸入 **command** ，然後按 enter 鍵。 在結果中，於 [命令提示字元]**** 上按一下滑鼠右鍵，然後按一下 [以系統管理員身分執行]****。

    -   在 Windows 7 的 [ **開始** ] 功能表上，于 [搜尋] 方塊中輸入 **command** ，以滑鼠右鍵按一下 [ **命令提示**字元]，然後按一下 [以 **系統管理員身分執行**]。

3.  在命令提示字元中輸入下列命令，然後按 ENTER：

    ```
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1
    ```


4.  完成[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)中的步驟。


####  <a name="join-a-second-server-to-the-network"></a><a name="BKMK_SecondServer"></a> 將第二部伺服器加入網路

###### <a name="to-join-a-second-server-to-the-network"></a>將第二部伺服器加入網路

1.  登入您想要連線到 Windows Server Essentials 網路的伺服器。

2.  開啟網際網路瀏覽器，然後在網址列中輸入**HTTP://<servername \> /Connect**，其中 *<Servername \> *是執行 Windows server Essentials 的伺服器名稱，然後按 enter。

3.  如果您嘗試連線到 Windows Server Essentials 網路的伺服器上已啟用 Internet Explorer 增強式安全性設定，請完成下列工作；否則請跳過這個步驟。

    1.  若要接受封鎖訊息，請按一下 [關閉]****。

    2.  將 **HTTP://<servername \> /Connect** 網站新增至信任的網站，如下所示：

        1.  在瀏覽器瀏覽窗格中，按一下 [工具]****，然後按一下 [網際網路選項]****。

        2.  按一下 [安全性]**** 索引標籤，然後按一下 [信任的網站]****。

        3.  按一下 **[網站]** 。

        4.  網站應該會顯示在 [將這個網站新增到區域]**** 欄位中。 按一下 [新增] 。

        5.  按一下 [關閉]****，然後按一下 [確定]****。

    3.  重新整理網頁。


    4.  若要將第二部伺服器連線到執行 Windows Server Essentials 的伺服器，請依照[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)中的指示執行。


~~~
> [!NOTE]
>  Connecting a second server to a server running Windows Server Essentials differs from connecting to a client computer as follows:
>
>  -   There is no user profile migration; therefore, the current profile will not be migrated.
> -   You cannot back up the second server by using client computer backup, and there is no option to wake up the computer for backup.
~~~

 將第二部伺服器加入執行 Windows Server Essentials 的伺服器之後，會提供下列功能給連線的伺服器：

- 「更新」和「警示」狀態功能的運作方式與其他用戶端電腦相同。

- 「線上」和「離線」功能的運作方式與其他用戶端電腦相同。

- 您可以使用「遠端 Web 存取」來連線到您的第二部伺服器。

- 第二部伺服器會包含在「健康情況報告」中，因為 Windows Server Essentials 將會產生與此伺服器相關的警示。

  從執行 Windows Server Essentials 的伺服器管理第二部伺服器與管理其他用戶端電腦會有所不同，如下所示：

- 將不會有系統匣應用程式、[啟動列] 及 [儀表板用戶端] 的進入點。

- 第二部伺服器會列在 [裝置]**** 索引標籤上的 [伺服器]**** 群組內 。

- 由於第二部伺服器並不支援用戶端電腦備份，因此備份狀態會顯示為 [不支援]****。 此外，如果您選取第二部伺服器並按一下滑鼠右鍵，也不會顯示第二部伺服器的任何備份和還原相關工作。

- 如果您選取第二部伺服器，然後按一下 [ **查看伺服器屬性** ] 工作，伺服器的 [屬性] 頁面上就不會顯示 [ **備份** ] 索引標籤。

- 因為 Windows Server 作業系統上沒有資訊安全中心，所以第二部伺服器的安全性狀態會顯示為 [ **不適用**]。

- 第二部伺服器的群組原則狀態會顯示為 [ **不適用**]。

###  <a name="install-the-connector-software"></a><a name="BKMK_11"></a> 安裝連接器軟體
 當您使用 [將電腦連線到伺服器精靈] 將您的電腦連線到伺服器時，就會安裝 Windows Server Essentials 中的連線程式軟體。 若要啟動此嚮導，請在網頁瀏覽器的網址列中輸入**HTTP://<ServerName \> /connect** (，其中 *<ServerName \> *是您的伺服器) 的名稱。

> [!NOTE]
>  如果您的電腦位於遠端位置，若要執行 [將電腦連線到伺服器]，請在網頁流覽 (器的網址列中輸入**HTTP://<domainname \> /connect** ，其中 *<\> 網域*是您組織) 的功能變數名稱。 您可以向您的網路系統管理員取得您的網域名稱資訊。

 連線程式軟體會執行下列動作：

-   將您的電腦連線到 Windows Server Essentials

-   每晚自動備份您的電腦 (如果您設定讓伺服器建立用戶端備份)

-   協助系統管理員監視您電腦的健康情況

-   可讓您設定並從家用電腦遠端管理 Windows Server Essentials


 如需有關將您的電腦連線到 Windows Server Essentials 伺服器的逐步指示，請參閱[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。


###  <a name="move-computer-data-and-settings-manually"></a><a name="BKMK_12"></a> 手動移動電腦資料和設定
  Windows Server Essentials 和 Windows Server Essentials 僅支援執行 Windows 7 作業系統的用戶端電腦的使用者設定檔遷移。 當您將 Windows 7 型電腦連線到伺服器時，[將電腦連線到伺服器精靈] 可以自動移轉使用者設定檔。

 將 Windows 8、Windows 8.1 或 Windows 10 的電腦連接到伺服器時，無法自動傳送使用者設定檔。 不過，在 Windows 8 電腦上，您可以使用「Windows 輕鬆傳輸」將原始本機使用者的資料和設定傳輸到已加入網域的電腦。 若要這樣做，您必須同時是 Windows 8 來源電腦和 Windows 8 目的電腦上的系統管理員。 如需使用「Windows 輕鬆傳輸」來傳輸檔案和設定的相關資訊，請參閱 Microsoft 知識庫中的[文章 2735227](https://support.microsoft.com/kb/2735227)。

###  <a name="transfer-multiple-user-profiles-during-computer-deployment"></a><a name="BKMK_Transfer"></a> 在電腦部署期間傳送多個使用者設定檔
 若要傳輸多個本機使用者設定檔，在您將執行 Windows 7 或 Windows 7 SP1 作業系統的電腦連線到 Windows Server Essentials 伺服器之前，必須先在伺服器上建立對應的網路使用者帳戶。 如需有關建立網路使用者帳戶的詳細資訊，請參閱[新增使用者帳戶](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)。

 只有在執行 windows Server Essentials) 的 windows 7 (，或 windows Server Essentials) 的 Windows 7 SP1 (電腦上，才支援使用者設定檔遷移。 當您使用 [將您的電腦連線到伺服器精靈] 將電腦連線到 Windows Server Essentials 伺服器時，會提供您將舊使用者本機帳戶的使用者資料和設定移到新網路使用者帳戶的選項。 若要這樣做，請在精靈的 [移動現有的使用者資料和設定]**** 頁面上，將網路使用者帳戶對應到存在於電腦上的本機使用者帳戶，以傳輸位於用戶端電腦上的多個使用者設定檔。

###  <a name="uninstall-the-connector-software"></a><a name="BKMK_13"></a> 卸載連接器軟體
 您可以使用 [控制台] 將連線程式軟體從電腦中解除安裝。 通常是在連線程式軟體發生問題，或需要安裝新版的連線程式軟體時，您才會這樣做。 您必須以系統管理員身分登入電腦，才能完成這個程序。

> [!IMPORTANT]
>  如果您升級用戶端電腦上的作業系統，系統會自動將連線程式軟體解除安裝。 您必須在升級完成之後，重新安裝連線程式軟體。 慣用的方法是在升級作業系統之前，先將連線程式軟體解除安裝。 在升級完成之後才將連線程式軟體解除安裝仍算可以接受；不過，可能會導致用戶端電腦與伺服器的狀態不一致，直到將連線程式軟體解除安裝後再重新安裝為止。

##### <a name="to-uninstall-connector-software-from-a-computer"></a>將連線程式軟體從電腦中解除安裝

1.  從執行 Windows 7、Windows 8、Windows 8.1 或 Windows 10 的電腦開啟 **[控制台]**，然後在 **[程式集]** 區段中，按一下 **[檢視已安裝的更新]**。

2.  從已安裝的程式清單中選取 [Windows Server Essentials 連線程式]****，然後按一下 [解除安裝]****。

3.  在警告頁面上，按一下 [是]****。

4.  如果出現 [使用者帳戶控制]**** 視窗，請按一下 [允許]****。

5.  在 Windows Server Essentials 中，如果出現 [Windows Server Essentials 連接器] 頁面，建議您關閉啟動控制板，請按一下 **[確定]**。

6.  等候程式解除安裝。 移除軟體之後，[Windows Server Essentials 連線程式]**** 就不會再出現在已安裝的程式或更新清單中。 此外，啟動控制板和儀表板的快捷方式不會再顯示于電腦的桌面上。

> [!NOTE]
> - 將連線程式軟體解除安裝並不會將電腦從 [儀表板] 之 [裝置]**** 索引標籤上顯示的電腦清單中移除。 若要將電腦從 [儀表板] 中移除，請參閱[將電腦從伺服器中移除](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)。
>   -   將連線程式軟體解除安裝時，並不會刪除用戶端電腦上對應到伺服器的共用資料夾。 您必須手動刪除對應到伺服器的共用資料夾。
>
> -   將連線程式軟體解除安裝並不會讓電腦退出原始網域。 您必須手動將電腦退出網域。 如需相關指示，請參閱[將電腦從 Windows 網域中移除](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)。


###  <a name="disconnect-your-computer-from-or-reconnect-your-computer-to-the-server"></a><a name="BKMK_14"></a> 中斷電腦的連線，或將電腦重新連線到伺服器
 若要將電腦與伺服器中斷連線，您必須完成下列步驟：


1. 使用 [控制台] 將連線程式軟體從電腦中解除安裝。 如需逐步指示，請參閱[將連線程式軟體解除安裝](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。


2. 將電腦退出 Windows Server Essentials 網域，然後加入工作群組。 如需將 Windows 加入工作群組的逐步指示，請參閱 [加入或建立工作群組](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup)。

3. 使用 [儀表板] 將電腦從伺服器中移除。 如需逐步指示，請參閱[將電腦從伺服器中移除](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)。

   若要將電腦重新連線到先前與您 Windows Server Essentials 伺服器網路中斷連線的伺服器，您必須完成下列步驟：


4. 使用 [控制台] 將連線程式軟體從電腦中解除安裝。 如需逐步指示，請參閱[將連線程式軟體解除安裝](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。

5. 將電腦退出 Windows Server Essentials 網域，然後加入工作群組。 如需將 Windows 加入工作群組的逐步指示，請參閱 [加入或建立工作群組](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup)。

6. 使用 [Connect Computer 精靈] 將電腦連線到伺服器。 如需逐步指示，請參閱[將電腦連線到伺服器](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。

###  <a name="how-backup-works-with-sleep-and-hibernate-modes"></a><a name="BKMK_Sleep"></a> 備份如何搭配睡眠和睡眠模式運作
 如果您在將電腦連線到伺服器時選取 [喚醒這台電腦以進行備份]**** 選項，電腦將會依據備份排程中所指定，每天自動從睡眠或休眠模式喚醒以供進行備份。 備份完成之後，電腦會依據其電源管理設定，回到睡眠或休眠模式。 如果您沒有選取這個選項，當電腦處於睡眠或休眠狀態時，伺服器將不會備份電腦。 如需詳細資訊，請參閱 [管理用戶端備份](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)。

##  <a name="use-the-launchpad"></a><a name="BKMK_C"></a> 使用啟動控制板
 您可以使用 [啟動列] 從 Windows Server Essentials 伺服器存取共用資源、執行電腦備份，以及回應系統健康情況警示。

-   [啟動列概觀](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)

-   [在 Mac 電腦使用啟動列](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)

## <a name="additional-references"></a>其他參考資料

-   [電腦與伺服器連線問題疑難排解](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)

-   [管理使用者帳戶](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)


-   [使用共用資料夾](Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [從遠端工作](Work-Remotely-in-Windows-Server-Essentials.md)

-   [播放數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [從遠端工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)

-   [播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

