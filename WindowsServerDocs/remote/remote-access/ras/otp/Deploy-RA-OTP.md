---
title: 使用 OTP 驗證部署遠端存取
description: 瞭解如何設定啟用 DirectAccess 的遠端存取服務器，以使用雙因素單次密碼驗證來驗證 DirectAccess 用戶端使用者。
manager: brianlic
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 14b546cf7a0fecbff94c6778e3731d02f57bddf6
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038878"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>使用 OTP 驗證部署遠端存取

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與路由及遠端存取服務 \( RRAS \) VPN 合併為單一遠端存取角色。

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>案例描述
在此案例中，已啟用 DirectAccess 的遠端存取服務器會設定為使用兩個因素單次密碼 OTP 驗證來驗證 DirectAccess 用戶端使用者 \- \( ，以及 \) 標準 Active Directory 認證。

## <a name="prerequisites"></a>先決條件
開始部署這個案例之前，請先檢閱這份重要需求清單：

-   部署 OTP 之前，必須先部署[具有 Advanced Settings 的單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。

-   Windows 7 用戶端必須使用 DCA 2.0 來支援 OTP。

-   OTP 不支援 PIN 變更。

-   必須部署公開金鑰基礎結構。

    如需詳細資訊，請參閱： [測試實驗室指南小單元：Windows Server 2012 的基本 PKI。](/answers/topics/windows-server-2012.html)

-   不支援在 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 之外變更原則。

## <a name="in-this-scenario"></a>在這個案例中
OTP 驗證案例包含一些步驟：

1.  [使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 設定 OTP 之前，必須先部署單一遠端存取服務器。 規劃以及部署單一伺服器的過程包含設計和設定網路拓樸、規劃以及部署憑證、設定 DNS 和 Active Directory、設定遠端存取伺服器、部署 DirectAccess 用戶端以及準備內部網路伺服器。

2.  [規劃使用 OTP 驗證進行遠端存取](./plan/plan-remote-access-with-otp-authentication.md)。 除了針對單一伺服器所需的規劃之外，OTP 還需要規劃 Microsoft 憑證授權單位單位 \( CA \) 和 otp 的憑證範本，以及啟用 RADIUS 的 \- otp 伺服器。 規劃也可能包含安全性群組的需求，以免除特定使用者免于強式 \( OTP 或智慧卡 \) 驗證。 如需有關在多樹系環境中設定 OTP 的詳細資訊 \- ，請參閱 [設定多樹系部署](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md)。

3.  [使用 OTP 驗證設定 DirectAccess](/configure/Configure-RA-with-OTP-Authentication.md)。 OTP 部署是由許多設定步驟所組成，包括準備 OTP 驗證的基礎結構、設定 OTP 伺服器、設定遠端存取服務器的 OTP 設定，以及更新 DirectAccess 用戶端設定。

4.  [針對 OTP 部署進行疑難排解] ( # B1/troubleshoot/Troubleshoot-an-OTP-Deployment.md) 。 此疑難排解章節說明使用 OTP 驗證部署「遠端存取」時可能發生的一些最常見錯誤。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
提高安全性-使用 OTP 可提高 DirectAccess 部署的安全性。 使用者一定要有 OTP 認證，才能連通內部網路。 使用者透過 Windows 10 或 Windows 8 用戶端電腦上網路連線中的可用工作地點連線，或 \( \) 在執行 Windows 7 的用戶端電腦上使用 DirectAccess 連線助理 DCA，提供 OTP 認證。 OTP 驗證程序運作方式如下：

1.  DirectAccess 用戶端會輸入網域認證，以透過基礎結構通道存取 DirectAccess 基礎結構伺服器 \( \) 。  如果因為特定的 IKE 失敗而無法連接內部網路，用戶端電腦上的「工作地方連線」會通知使用者務必準備好認證。 在執行 Windows 7 的用戶端電腦上， \- 會出現要求智慧卡認證的快顯視窗。

2.  輸入 OTP 認證之後，系統會透過 SSL 將其傳送至遠端存取服務器，並提供短期智慧卡登入憑證的要求 \- 。

3.  遠端存取服務器會使用以 RADIUS 為基礎的 OTP 伺服器來起始 OTP 認證的驗證 \- 。

4.  如果成功，遠端存取伺服器會使用其登錄授權單位憑證簽署憑證要求，然後傳回至 DirectAccess 用戶端電腦

5.  DirectAccess 用戶端電腦會將已簽署的憑證要求轉送至 CA，並儲存註冊的憑證供 Kerberos SSP \/ AP 使用。

6.  使用這個憑證時，用戶端無形中就是在執行標準智慧卡 Kerberos 驗證。

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>這個案例包含的角色與功能
下表列出本案例必備的角色和功能：

|角色 \/ 功能|如何支援本案例|
|---------|-----------------|
|*遠端存取管理角色*|這個角色是利用伺服器管理員主控台安裝和解除安裝。 此角色包含 DirectAccess （先前為 Windows Server 2008 R2 中的一項功能），以及路由及遠端存取服務（先前是網路原則與存取服務 \( NPAS 伺服器角色底下的角色服務） \) 。 遠端存取角色包含兩個元件：<p>1. DirectAccess 與路由及遠端存取服務 \( RRAS \) VPN-DirectAccess 和 VPN 會在 [遠端存取管理] 主控台中一起管理。<br />2. 在舊版路由和遠端存取主控台中管理 RRAS 路由 RRAS 的路由功能。<p>遠端存取角色需要以下伺服器功能：<p>-Internet Information Services \( IIS \) Web 服務器-設定網路位置伺服器、使用 OTP 驗證，以及設定預設 Web 探查時，需要這項功能。<br />-Windows 內部 Database-Used 用於遠端存取服務器上的本機帳戶處理。|
|遠端存取管理工具功能|這個功能的安裝方式如下：<p>-安裝遠端存取角色時，預設會將它安裝在遠端存取服務器上，並支援遠端管理主控台使用者介面。<br />-您可以選擇性地將它安裝在未執行遠端存取服務器角色的伺服器上。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<p>遠端存取管理工具功能包含以下各項：<p>-遠端存取 GUI 和命令列工具<br />-Windows PowerShell 的遠端存取模組<p>依存項目包括：<p>-群組原則管理主控台<br />-RAS 連線管理員系統管理組件 \( CMAK\)<br />-Windows PowerShell 3。0<br />-圖形化管理工具和基礎結構|

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>硬體需求
本案例需要的硬體如下所示：

-   符合 Windows Server 2016 或 Windows Server 2012 硬體需求的電腦。

-   為了測試案例，至少需要一部執行 Windows 10、Windows 8 或 Windows 7 的電腦設定為 DirectAccess 用戶端。

-   一部 OTP 伺服器，支援透過 RADIUS 進行 PAP。

-   OTP 硬體或軟體權杖。

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求
本案例的一些需求：

1.  部署單一伺服器時的軟體需求。 如需詳細資訊，請參閱 [使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。

2.  除了單一伺服器的軟體需求以外，還有一些 OTP \- 特有的需求：

    1.  IPsec 驗證的 CA-在 OTP 部署中，必須使用 CA 所發行的 IPsec 電腦憑證來部署 DirectAccess。 在部署的 OTP 中，不支援將遠端存取伺服器當作 Kerberos Proxy，負責驗證工作。 需要內部 CA。

    2.  CA 進行 OTP 驗證- \( 需要在 Windows 2003 伺服器或更新版本上執行的 Microsoft 企業 CA \) ，才能發行 OTP 用戶端憑證。 為了驗證 IPsec 而負責簽發憑證的同一個 CA，也是可以使用的。 CA 伺服器必須可透過第一個基礎結構通道來使用。

    3.  安全性群組-若要豁免使用者免于強式驗證，則需要包含這些使用者的 Active Directory 安全性群組。

    4.  客戶 \- 端需求-針對 Windows 10 和 Windows 8 用戶端電腦，會使用網路連接助理的 \( NCA \) 服務來偵測是否需要 OTP 認證。 如果是，DirectAccess 媒體管理員會提示輸入認證。  NCA 包含在作業系統中，不需要安裝或部署。 若為 Windows 7 用戶端電腦，則需要 DirectAccess 連線助理 \( DCA \) 2.0。 您可以從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=29039)加以取得。

    5.  請注意：

        1.  OTP 驗證可與智慧卡和可信賴平臺模組 TPM 型驗證並行使用 \( \) \- 。 在遠端存取管理主控台中啟用 OTP 驗證，也可以啟用智慧卡驗證。

        2.  在遠端存取設定期間，指定安全性群組中的使用者可以免于雙 \- 因素驗證，因此只會以使用者名稱密碼進行驗證 \/ 。

        3.  支援 OTP 新的 PIN 以及下一個 tokencode 模式

        4.  在遠端存取多站台部署中，OTP 設定是全系統通用的而且會識別所有進入點。 如果為 OTP 設定多個 RADIUS 或 CA 伺服器，每一個遠端存取伺服器就會根據可用性和遠近關係排序它們。

        5.  在遠端存取多樹系環境中 \- 設定 otp 時，OTP ca 應該只來自資源樹系，而憑證註冊應該跨樹系信任進行設定。 如需詳細資訊，請參閱 [AD CS：將跨樹系憑證註冊到 Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff955842(v=ws.10))。

        6.  使用金鑰 FOB OTP 權杖的使用者應該 \( \) 在 [DirectAccess OTP] 對話方塊中插入 PIN，並在後面加上權杖，而不含任何分隔符號。 PIN PAD OTP 權杖使用者只能在對話方塊中插入 tokencode。

        7.  如果已啟用 WEBDAV，則不應啟用 OTP。

## <a name="known-issues"></a><a name="KnownIssues"></a>已知問題
以下是設定 OTP 案例的已知問題：

-   遠端存取會使用探查機制來確認與 RADIUS 型 \- OTP 伺服器的連線能力。 在某些情況下，這可能會導致 OTP 伺服器發出錯誤。 若要避免這個問題，請在 OTP 伺服器上執行下列作業：

    -   建立與遠端存取伺服器上針對探查機制而設定的使用者名稱和密碼相符的使用者帳戶。 此使用者名稱不應定義 Active Directory 使用者。

        根據預設，遠端存取伺服器上的使用者名稱會是 DAProbeUser，密碼是 DAProbePass。 這些預設設定可以使用遠端存取伺服器的下列登錄值來修改：

        -   HKEY \_ 本機 \_ 電腦 \\ 軟體 \\ Microsoft \\ DirectAccess \\ OTP \\ RadiusProbeUser

        -   HKEY \_ 本機 \_ 電腦 \\ 軟體 \\ Microsoft \\ DirectAccess \\ OTP \\ RadiusProbePass

-   如果您在已設定並執行中的 DirectAccess 部署中變更 IPsec 根憑證，OTP 將會停止運作。 若要解決此問題，請在每個 DirectAccess 伺服器上的 Windows PowerShell 提示字元中執行下列命令： `iisreset`