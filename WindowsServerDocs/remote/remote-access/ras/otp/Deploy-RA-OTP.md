---
title: 使用 OTP 驗證部署遠端存取
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d0de5f459e31e1dfac40e49cd6cc83de8722df4d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404430"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>使用 OTP 驗證部署遠端存取

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與路由及遠端存取服務 \(RRAS @ no__t-1 VPN 合併成一個遠端存取角色。   

## <a name="BKMK_OVER"></a>案例描述  
在此案例中，已啟用 DirectAccess 的遠端存取服務器已設定為使用兩個 @ no__t-0factor 一次性密碼來驗證 DirectAccess 用戶端使用者 \(OTP @ no__t-2 驗證，以及標準 Active Directory 認證。  
  
## <a name="prerequisites"></a>必要條件  
開始部署這個案例之前，請先檢閱這份重要需求清單：  
  
-   部署 OTP 之前，必須先部署[具有 Advanced 設定的單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
-   Windows 7 用戶端必須使用 DCA 2.0 來支援 OTP。  
  
-   OTP 不支援 PIN 變更。  
  
-   必須部署公開金鑰基礎結構。  
  
    如需詳細資訊，請參閱：@no__t 0Test Lab Guide 迷你模組：Windows Server 2012 的基本 PKI。 ](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   不支援在 DirectAccess 管理主控台或 Windows PowerShell Cmdlet 以外變更原則。  
  
## <a name="in-this-scenario"></a>在這個案例中  
OTP 驗證案例包含一些步驟：  
  
1.  [使用 [Advanced] 設定部署單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 設定 OTP 之前，必須先部署單一遠端存取服務器。 規劃以及部署單一伺服器的過程包含設計和設定網路拓樸、規劃以及部署憑證、設定 DNS 和 Active Directory、設定遠端存取伺服器、部署 DirectAccess 用戶端以及準備內部網路伺服器。  
  
2.  [使用 OTP 驗證規劃遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication)。 除了針對單一伺服器所需的規劃以外，OTP 還需要規劃 Microsoft 憑證授權單位單位 \(CA @ no__t-1 和 OTP 的憑證範本。和 RADIUS @ no__t-2enabled OTP 伺服器。 規劃也可能包含安全性群組的需求，讓特定使用者免于受到強式 @no__t 0OTP 或智慧卡 @ no__t-1 驗證的要求。 如需多個 @ no__t-0forest 環境中 OTP 設定的相關資訊，請參閱[設定多樹系部署](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md)。  
  
3.  [使用 OTP 驗證來設定 DirectAccess](/configure/Configure-RA-with-OTP-Authentication.md)。 OTP 部署包含數個設定步驟，包括準備 OTP 驗證的基礎結構、設定 OTP 伺服器、設定遠端存取服務器上的 OTP 設定，以及更新 DirectAccess 用戶端設定。  
  
4.  [針對 OTP 部署進行疑難排解]((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). 此疑難排解章節說明使用 OTP 驗證部署「遠端存取」時，可能發生的一些最常見錯誤。  
  
## <a name="BKMK_APP"></a>實際應用  
增加安全性-使用 OTP 可增加 DirectAccess 部署的安全性。 使用者一定要有 OTP 認證，才能連通內部網路。 使用者可透過 Windows 10 或 Windows 8 用戶端電腦上網路連線中的「工作場所連線」，或在執行 Windows 7 的用戶端電腦上使用 DirectAccess 連線助理 \(DCA @ no__t 來提供 OTP 認證。 OTP 驗證程序運作方式如下：  
  
1.  DirectAccess 用戶端會輸入網域認證來存取 DirectAccess 基礎結構伺服器，\(over 基礎結構通道 @ no__t-1。  如果因為特定的 IKE 失敗而無法連接內部網路，用戶端電腦上的「工作地方連線」會通知使用者務必準備好認證。 在執行 Windows 7 的用戶端電腦上，會出現 pop @ no__t-0up 要求智慧卡認證。  
  
2.  輸入 OTP 認證之後，它們會透過 SSL 傳送至遠端存取服務器，以及簡短 @ no__t-0term 智慧卡登入憑證的要求。  
  
3.  遠端存取服務器會使用 RADIUS @ no__t-0based OTP 伺服器起始 OTP 認證的驗證。  
  
4.  如果成功，遠端存取伺服器會使用其登錄授權單位憑證簽署憑證要求，然後傳回至 DirectAccess 用戶端電腦  
  
5.  DirectAccess 用戶端電腦會將已簽署的憑證要求轉送至 CA，並儲存已註冊的憑證供 Kerberos SSP @ no__t-0AP 使用。  
  
6.  使用這個憑證時，用戶端無形中就是在執行標準智慧卡 Kerberos 驗證。  
  
## <a name="BKMK_NEW"></a>此案例中包含的角色和功能  
下表列出本案例必備的角色和功能：  
  
|角色 @ no__t-0feature|如何支援本案例|  
|---------|-----------------|  
|*遠端存取管理角色*|這個角色是利用伺服器管理員主控台安裝和解除安裝。 此角色同時包含 DirectAccess （先前為 Windows Server 2008 R2 中的功能）和路由及遠端存取服務，先前是網路原則與存取服務的角色服務 \(NPAS @ no__t-1 伺服器角色。 遠端存取角色包含兩個元件：<br /><br />1.DirectAccess 與路由及遠端存取服務 \(RRAS @ no__t-1 VPN-DirectAccess 和 VPN 會在 [遠端存取管理] 主控台中一起管理。<br />2.RRAS 路由-在舊版路由和遠端存取主控台中管理 RRAS 路由功能。<br /><br />遠端存取角色需要以下伺服器功能：<br /><br />-Internet Information Services \(IIS @ no__t-1 Web 服務器-設定網路位置伺服器、使用 OTP 驗證和設定預設 Web 探查時，需要這項功能。<br />-Windows 內部資料庫-用於遠端存取服務器上的本機帳戶處理。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-安裝遠端存取角色時，預設會將它安裝在遠端存取服務器上，並支援遠端管理主控台使用者介面。<br />-您可以選擇性地將它安裝在未執行遠端存取服務器角色的伺服器上。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取 GUI 和命令列工具<br />-適用于 Windows PowerShell 的遠端存取模組<br /><br />依存項目包括：<br /><br />-群組原則管理主控台<br />-RAS 連線管理員系統管理元件 \(CMAK @ no__t-1<br />-Windows PowerShell 3。0<br />-圖形化管理工具與基礎結構|  
  
## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
-   一部符合 Windows Server 2016 或 Windows Server 2012 硬體需求的電腦。  
  
-   為了測試此案例，至少需要一部執行 Windows 10、Windows 8 或 Windows 7 的電腦設定為 DirectAccess 用戶端。  
  
-   一部 OTP 伺服器，支援透過 RADIUS 進行 PAP。  
  
-   OTP 硬體或軟體權杖。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
本案例的一些需求：  
  
1.  部署單一伺服器時的軟體需求。 如需詳細資訊，請參閱[使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
2.  除了單一伺服器的軟體需求以外，還有許多 OTP @ no__t-0specific 需求：  
  
    1.  IPsec 驗證的 CA-在 OTP 部署中，必須使用 CA 所簽發的 IPsec 機器憑證來部署 DirectAccess。 在部署的 OTP 中，不支援將遠端存取伺服器當作 Kerberos Proxy，負責驗證工作。 需要內部 CA。  
  
    2.  CA 進行 OTP 驗證-Windows 2003 Server 或更新版本上的 Microsoft 企業 CA @no__t 0running，必須要有 @ no__t-1 才能發行 OTP 用戶端憑證。 為了驗證 IPsec 而負責簽發憑證的同一個 CA，也是可以使用的。 CA 伺服器必須可透過第一個基礎結構通道來使用。  
  
    3.  安全性群組-若要讓使用者免于進行增強式驗證，則需要包含這些使用者的 Active Directory 安全性群組。  
  
    4.  用戶端 @ no__t-0side 需求-對於 Windows 10 和 Windows 8 用戶端電腦，網路連線助理 \(NCA @ no__t-2 服務用來偵測是否需要 OTP 認證。 如果是，DirectAccess 媒體管理員就會提示您輸入認證。  NCA 已包含在作業系統中，不需要安裝或部署。 若是 Windows 7 用戶端電腦，DirectAccess 連線助理 \(DCA @ no__t-1 2.0 是必要的。 您可以從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=29039)加以取得。  
  
    5.  請注意下列事項：  
  
        1.  OTP 驗證可以與智慧卡和信賴平臺模組並行使用 \(TPM @ no__t-1 @ no__t-2based authentication。 在遠端存取管理主控台中啟用 OTP 驗證，也可以啟用智慧卡驗證。  
  
        2.  在遠端存取設定期間，指定安全性群組中的使用者可以豁免兩個 @ no__t-0factor 驗證，因此只能使用 username @ no__t-1password 進行驗證。  
  
        3.  支援 OTP 新的 PIN 以及下一個 tokencode 模式  
  
        4.  在遠端存取多站台部署中，OTP 設定是全系統通用的而且會識別所有進入點。 如果為 OTP 設定多個 RADIUS 或 CA 伺服器，每一個遠端存取伺服器就會根據可用性和遠近關係排序它們。  
  
        5.  在遠端存取多個 @ no__t-0forest 環境中設定 OTP 時，OTP Ca 只能來自資源樹系，而且應該在樹系信任之間設定憑證註冊。 如需詳細資訊，請參閱 @no__t 0AD CS：Windows Server 2008 R2 @ no__t-0 的跨樹系憑證註冊。  
  
        6.  使用金鑰 FOB OTP 權杖的使用者應在 [DirectAccess OTP] 對話方塊中插入 PIN，後面加上權杖 \(without 任何分隔符號 @ no__t-1。 PIN PAD OTP 權杖使用者只能在對話方塊中插入 tokencode。  
  
        7.  如果已啟用 WEBDAV，則不應啟用 OTP。  
  
## <a name="KnownIssues"></a>已知問題  
以下是設定 OTP 案例的已知問題：  
  
-   遠端存取會使用探查機制來驗證 RADIUS @ no__t-0based OTP 伺服器的連線能力。 在某些情況下，這可能會導致 OTP 伺服器發出錯誤。 若要避免這個問題，請在 OTP 伺服器上執行下列作業：  
  
    -   建立與遠端存取伺服器上針對探查機制而設定的使用者名稱和密碼相符的使用者帳戶。 此使用者名稱不應定義 Active Directory 使用者。  
  
        根據預設，遠端存取伺服器上的使用者名稱會是 DAProbeUser，密碼是 DAProbePass。 這些預設設定可以使用遠端存取伺服器的下列登錄值來修改：  
  
        -   HKEY @ no__t-0LOCAL @ no__t-1MACHINE @ no__t-2SOFTWARE @ no__t-3Microsoft @ no__t-4DirectAccess @ no__t-5OTP @ no__t-6RadiusProbeUser  
  
        -   HKEY @ no__t-0LOCAL @ no__t-1MACHINE @ no__t-2SOFTWARE @ no__t-3Microsoft @ no__t-4DirectAccess @ no__t-5OTP @ no__t-6 RadiusProbePass  
  
-   如果您在已設定並執行中的 DirectAccess 部署中變更 IPsec 根憑證，OTP 將會停止運作。 若要解決此問題，請在每個 DirectAccess 伺服器上的 Windows PowerShell 提示字元中，執行下列命令： `iisreset`  
  
