---
title: 使用 OTP 驗證部署遠端存取
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecb4503c6f8cbc08f2175a33a41929491e4a2b7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811989"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>使用 OTP 驗證部署遠端存取

>適用於：Windows Server （半年通道），Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與路由及遠端存取服務\(RRAS\) VPN 成一個遠端存取角色。   

## <a name="BKMK_OVER"></a>案例描述  
在此案例中遠端存取具備 DirectAccess 的伺服器設定為 DirectAccess 用戶端使用進行使用者驗證兩個\-因素單次密碼\(OTP\)驗證，除了標準的作用目錄的認證。  
  
## <a name="prerequisites"></a>先決條件  
開始部署這個案例之前，請先檢閱這份重要需求清單：  
  
-   [部署單一 DirectAccess 伺服器使用進階設定](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)必須部署 OTP 之前部署。  
  
-   Windows 7 用戶端必須使用 DCA 2.0 來支援 OTP。  
  
-   OTP 不支援 PIN 變更。  
  
-   必須部署公開金鑰基礎結構。  
  
    如需詳細資訊，請參閱：[測試實驗室指南小單元：適用於 Windows Server 2012 的基本 PKI。](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   不支援變更原則之外的 DirectAccess 管理主控台或 Windows PowerShell cmdlet。  
  
## <a name="in-this-scenario"></a>在這個案例中  
OTP 驗證案例包含一些步驟：  
  
1.  [部署單一 DirectAccess 伺服器使用進階設定](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 設定 OTP 之前，必須部署單一遠端存取伺服器。 規劃以及部署單一伺服器的過程包含設計和設定網路拓樸、規劃以及部署憑證、設定 DNS 和 Active Directory、設定遠端存取伺服器、部署 DirectAccess 用戶端以及準備內部網路伺服器。  
  
2.  [規劃遠端存取使用 OTP 驗證](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication)。 除了所需的規劃為單一伺服器，OTP 需要計劃 Microsoft 憑證授權單位\(CA\)和 憑證範本以及 RADIUS\-啟用 OTP 伺服器。 計劃可能也會包含安全性群組，以強式豁免特定使用者的需求\(OTP 或智慧卡\)驗證。 如需有關在多重設定 OTP\-樹系環境，請參閱 <<c2> [ 設定多樹系部署](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md)。  
  
3.  [設定 DirectAccess 使用 OTP 驗證](/configure/Configure-RA-with-OTP-Authentication.md)。 OTP 部署包含數個設定步驟，包括準備 OTP 驗證，設定 OTP 伺服器、 設定 OTP 設定上的遠端存取伺服器，以及更新 DirectAccess 用戶端設定的基礎結構。  
  
4.  [Troubleshoot an OTP Deployment]((/troubleshoot/Troubleshoot-an-OTP-Deployment.md)。 此疑難排解章節說明在使用 OTP 驗證部署遠端存取 」 時最常見錯誤數目。  
  
## <a name="BKMK_APP"></a>實際的應用程式  
增加安全性-使用 OTP 增加 DirectAccess 部署的安全性。 使用者一定要有 OTP 認證，才能連通內部網路。 使用者提供了 OTP 認證能夠透過網路連線的 Windows 10 或 Windows 8 用戶端電腦上，或使用 DirectAccess 連線助理 中，您可以使用 「 工作地點連線\(DCA\)執行的用戶端電腦上Windows 7。 OTP 驗證程序運作方式如下：  
  
1.  DirectAccess 用戶端輸入來存取 DirectAccess 基礎結構伺服器的網域認證\(透過基礎結構通道\)。  如果因為特定的 IKE 失敗而無法連接內部網路，用戶端電腦上的「工作地方連線」會通知使用者務必準備好認證。 用戶端電腦上執行 Windows 7，pop\-要求會顯示智慧卡認證。  
  
2.  輸入 OTP 認證之後，它們會透過 SSL 傳送到遠端存取伺服器，以及一個簡短的要求\-詞彙智慧卡登入憑證。  
  
3.  遠端存取伺服器會起始驗證使用 RADIUS 的 OTP 認證\-OTP 伺服器。  
  
4.  如果成功，遠端存取伺服器會使用其登錄授權單位憑證簽署憑證要求，然後傳回至 DirectAccess 用戶端電腦  
  
5.  DirectAccess 用戶端電腦將簽署的憑證要求轉送至 CA，並將註冊的憑證，以供 Kerberos SSP\/AP。  
  
6.  使用這個憑證時，用戶端無形中就是在執行標準智慧卡 Kerberos 驗證。  
  
## <a name="BKMK_NEW"></a>在此案例包含角色與功能  
下表列出本案例必備的角色和功能：  
  
|角色\/功能|如何支援本案例|  
|---------|-----------------|  
|*遠端存取管理角色*|這個角色是利用伺服器管理員主控台安裝和解除安裝。 這個角色包含 DirectAccess，這是先前在 Windows Server 2008 R2，以及 「 路由及遠端存取服務角色服務在網路原則與存取服務的功能\(NPAS\)伺服器角色。 遠端存取角色包含兩個元件：<br /><br />1.DirectAccess 與路由及遠端存取服務\(RRAS\) VPN-管理 DirectAccess 和 VPN 一起在遠端存取管理主控台中。<br />2.在舊版的路由及遠端存取主控台中管理 RRAS 路由-RRAS 路由功能。<br /><br />遠端存取角色需要以下伺服器功能：<br /><br />為 Internet Information Services \(IIS\) Web Serve-這項功能，才能設定網路位置伺服器、 使用 OTP 驗證和設定預設 web 探查。<br />-Windows 內部 Database-Used 的遠端存取伺服器上的本機計量。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-它預設會安裝在遠端存取伺服器時的遠端存取角色安裝，並且可支援遠端管理主控台使用者介面。<br />-它可以選擇性地安裝在不執行 遠端存取伺服器角色的伺服器。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取 GUI 和命令列工具<br />-適用於 Windows PowerShell 遠端存取模組<br /><br />依存項目包括：<br /><br />群組原則管理主控台<br />RAS 連線管理員系統管理組件\(CMAK\)<br />-   Windows PowerShell 3.0<br />圖形化管理工具和基礎結構|  
  
## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
-   符合 Windows Server 2016 或 Windows Server 2012 的硬體需求的電腦。  
  
-   為了測試案例，至少一部執行 Windows 10，Windows 8 或 Windows 7，設定為 DirectAccess 用戶端的電腦則是必要項目。  
  
-   一部 OTP 伺服器，支援透過 RADIUS 進行 PAP。  
  
-   OTP 硬體或軟體權杖。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
本案例的一些需求：  
  
1.  部署單一伺服器時的軟體需求。 如需詳細資訊，請參閱 <<c0> [ 部署單一 DirectAccess 伺服器使用進階設定](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
2.  除了在單一伺服器的軟體需求有幾個的 OTP\-特定需求：  
  
    1.  適用於 IPsec 驗證中的 CA 必須使用 IPsec 部署 DirectAccess OTP 部署機器 CA 所簽發的憑證。 在部署的 OTP 中，不支援將遠端存取伺服器當作 Kerberos Proxy，負責驗證工作。 需要內部 CA。  
  
    2.  OTP 驗證的 microsoft 企業 CA 的 CA\(在 Windows 2003 Server 或更新版本執行\)才能簽發 OTP 用戶端憑證。 為了驗證 IPsec 而負責簽發憑證的同一個 CA，也是可以使用的。 CA 伺服器必須可透過第一個基礎結構通道來使用。  
  
    3.  安全性群組來讓使用者得以豁免增強式驗證，Active Directory 安全性群組，其中包含這些使用者需要。  
  
    4.  用戶端\-戶端需求-適用於 Windows 10 和 Windows 8 用戶端電腦，網路連線助理\(NCA\)服務可用來偵測是否需要 OTP 認證。 如果它們是 DirectAccess 媒體管理員提示輸入認證。  作業系統中，有 nca，不不需要任何安裝或部署。 Windows 7 用戶端電腦，DirectAccess 連線助理\(DCA\) 2.0 是必要。 您可以從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=29039)加以取得。  
  
    5.  請注意下列事項：  
  
        1.  可以使用 OTP 驗證，以平行方式與智慧卡和信賴平台模組\(TPM\)\-型驗證。 在遠端存取管理主控台中啟用 OTP 驗證，也可以啟用智慧卡驗證。  
  
        2.  時，在指定的安全性的遠端存取設定使用者群組可以豁免於兩個\-因素驗證，並藉此驗證的使用者名稱與\/僅限於密碼。  
  
        3.  支援 OTP 新的 PIN 以及下一個 tokencode 模式  
  
        4.  在遠端存取多站台部署中，OTP 設定是全系統通用的而且會識別所有進入點。 如果為 OTP 設定多個 RADIUS 或 CA 伺服器，每一個遠端存取伺服器就會根據可用性和遠近關係排序它們。  
  
        5.  在遠端存取多重設定 OTP 時\-樹系環境，OTP Ca 只能來自資源樹系，以及應該跨樹系設定憑證註冊。 如需詳細資訊，請參閱[AD CS:跨樹系憑證註冊到 Windows Server 2008 R2](https://technet.microsoft.com/library/ff955842.aspx)。  
  
        6.  使用 KEY FOB OTP 權杖使用者應該插入 PIN 後面的 tokencode\(不含任何分隔符號\)DirectAccess OTP 對話方塊中。 PIN PAD OTP 權杖使用者只能在對話方塊中插入 tokencode。  
  
        7.  如果已啟用 WEBDAV，則不應啟用 OTP。  
  
## <a name="KnownIssues"></a>已知的問題  
以下是設定 OTP 案例的已知問題：  
  
-   遠端存取會使用探查機制來確認能夠連線到 RADIUS\-為基礎的 OTP 伺服器。 在某些情況下，這可能會導致 OTP 伺服器發出錯誤。 若要避免這個問題，請在 OTP 伺服器上執行下列作業：  
  
    -   建立與遠端存取伺服器上針對探查機制而設定的使用者名稱和密碼相符的使用者帳戶。 此使用者名稱不應定義 Active Directory 使用者。  
  
        根據預設，遠端存取伺服器上的使用者名稱會是 DAProbeUser，密碼是 DAProbePass。 這些預設設定可以使用遠端存取伺服器的下列登錄值來修改：  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   如果您在已設定並執行中的 DirectAccess 部署中變更 IPsec 根憑證，OTP 將會停止運作。 若要解決此問題，在每個 DirectAccess 伺服器上，在 Windows PowerShell 提示字元中執行命令： `iisreset`  
  
