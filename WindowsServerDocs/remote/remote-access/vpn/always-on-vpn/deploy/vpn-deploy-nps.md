---
title: 安裝並設定 NPS 伺服器
description: 使用者有權連線，使用者的身分識別，並會記錄連線要求您選擇在 NPS 中設定 RADIUS 帳戶處理時的層面，就會驗證之 VPN 伺服器所傳送的連線要求的 NPS 伺服器處理。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca53ef28497a78f264c60ac1132f721fb6e01c15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890309"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>步驟 4. 安裝及設定網路原則伺服器 (NPS)

>   適用於：Windows Server 2019，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10


&#171;  [**下一步:** 步驟 3。設定遠端存取伺服器的 Alwayson VPN](vpn-deploy-ras.md)<br>
&#187;  [**下一步:** 步驟 5.設定 DNS 和防火牆設定](vpn-deploy-dns-firewall.md)


在此步驟中，您可以安裝網路原則伺服器 (NPS) 之 VPN 伺服器所傳送的連線要求的處理：

- 執行以確認使用者有權連線的授權。
- 執行驗證來確認使用者的身分識別。
- 執行帳戶處理記錄的層面，選擇當您在 NPS 中設定 RADIUS 帳戶處理連接要求。

在本節中的步驟可讓您完成下列項目：

1.  在電腦或 NPS 伺服器規劃與安裝在您的組織或公司網路上的 VM 上，您可以安裝 NPS。

   >[!TIP] 
   >如果您已經在網路上有一或多個 NPS 伺服器，您就不必在執行 NPS 伺服器安裝-相反地，您可以使用本主題來更新現有的 NPS 伺服器的設定。

>[!NOTE]  
您可以在 Windows Server Core 上安裝網路原則伺服器服務。

2.  在公司組織/NPS 伺服器上，您可以設定 NPS 做為處理從 VPN 伺服器收到連線要求的 RADIUS 伺服器。

## <a name="install-network-policy-server"></a>安裝網路原則伺服器

在此程序中，您會藉由使用 Windows PowerShell 或伺服器管理員新增角色和功能精靈 安裝 NPS。 NPS 是網路原則與存取服務伺服器角色的角色服務。

>[!TIP] 
>根據預設值，NPS 會為所有已安裝的網路介面卡接聽連接埠 1812、1813、1645 和 1646 上的 RADIUS 流量。 當您安裝 NPS，而且您啟用具有進階安全性的 Windows 防火牆時，這些連接埠的防火牆例外狀況取得 IPv4 和 IPv6 流量自動建立。 如果您的網路存取伺服器會設定為透過非預設的連接埠傳送 RADIUS 流量，移除 NPS 在安裝期間，建立具有進階安全性的 Windows 防火牆中的例外狀況，並建立您要針對使用的連接埠的例外狀況RADIUS 流量。

**適用於 Windows PowerShell 的程序：**

若要使用 Windows PowerShell 中，系統管理員，以執行 Windows PowerShell 執行此程序輸入下列命令，並再按 ENTER 鍵。

`Install-WindowsFeature NPAS -IncludeManagementTools`

**程序的 伺服器管理員：**

1.  在 [伺服器管理員] 中，按一下**管理**，然後按一下**新增角色及功能**。 <p>[新增角色及功能精靈] 隨即開啟。

2.  在您開始前，按一下**下一步**。

    >[!NOTE] 
    >**在您開始前**如果您先前選取的 [新增角色及功能精靈] 的頁面未顯示**略過此頁面預設**[新增角色及功能精靈] 執行的時間。

1.  在 [選取安裝類型，請確認**角色型或功能型安裝**已選取，然後按一下**下一步]**。

2.  在 選取目的地伺服器，請確認**從伺服器集區選取伺服器**已選取。

3.  在 伺服器集區中，確認已選取 本機電腦，然後按一下 **下一步**。

4.  在選取伺服器角色**角色**，選取**網路原則與存取服務**。 會開啟一個對話方塊，要求如果它應該加入所需的網路原則與存取服務的功能。

5.  按一下 [**將功能加入**，然後按一下**下一步]**

6.  在 [選取功能] 中，按一下**下一步**，並在 [網路原則與存取服務，檢閱提供的資訊，然後按一下**下一步]**。

7.  在 [選取角色服務] 中，按一下**網路原則伺服器**。

8.  如需網路原則伺服器所需的功能，請按一下**將功能加入**，然後按一下**下一步**。

9.  在 確認安裝選項，按一下**需要時自動重新啟動目的地伺服器**。

10. 按一下  **是**以確認已選取，然後按一下**安裝**。 <p>[安裝進度] 頁面會顯示安裝程序期間的狀態。 此程序完成時，訊息 」 上安裝成功*ComputerName*"的訊息，其中*ComputerName*是您安裝網路原則伺服器的電腦名稱。

11. 按一下 **關閉**。

## <a name="configure-nps"></a>設定 NPS

安裝 NPS，您將 NPS 設定來處理所有驗證、 授權和計量職責，連線要求之後收到來自 VPN 伺服器。

### <a name="register-the-nps-server-in-active-directory"></a>在 Active Directory 中登錄 NPS 伺服器

在此程序中，您會註冊 Active Directory 中的伺服器，使其具有處理連線要求時的使用者帳戶資訊的存取權限。

**程序：**

1.  在 [伺服器管理員] 中，按一下**工具**，然後按一下**網路原則伺服器**。 NPS 主控台隨即開啟。

2.  在 NPS 主控台中，以滑鼠右鍵按一下**NPS （本機）**，然後按一下**在 Active Directory 中的註冊伺服器**來選取它。<p>[網路原則伺服器] 對話方塊隨即開啟。

3.  在 [網路原則伺服器] 對話方塊中，按一下**確定**兩次。

註冊 NPS 的替代方法，請參閱[在 Active Directory 網域中登錄 NPS 伺服器](../../../../../networking/technologies/nps/nps-manage-register.md)。

### <a name="configure-network-policy-server-accounting"></a>設定網路原則伺服器計量

在此程序，設定網路原則伺服器帳戶處理使用其中一個下列的記錄類型：

-   **事件記錄**。 主要用於稽核和疑難排解連線嘗試。 您可以設定 NPS 事件記錄取得在 NPS 主控台中的 NPS 伺服器屬性。

-   **使用者驗證與帳戶處理要求記錄到本機檔案**。 主要用於連線分析及計費用途。 也做為安全性調查工具，因為它會提供您追蹤活動的惡意使用者攻擊後方法。 您可以設定使用帳戶處理設定精靈 的本機檔案記錄。

-   **使用者驗證與帳戶處理要求記錄到 Microsoft SQL Server 符合 XML 資料庫**。 用來允許執行 NPS，有一個資料來源的多部伺服器。 也提供使用關聯式資料庫的優點。 您可以使用帳戶處理設定精靈來設定 SQL Server 記錄。

若要設定網路原則伺服器說明，請參閱[設定網路原則伺服器 Accounting](../../../../../networking/technologies/nps/nps-accounting-configure.md)。

### <a name="add-the-vpn-server-as-a-radius-client"></a>將 VPN 伺服器新增為 RADIUS 用戶端

在 [[遠端存取伺服器設定一律開啟 」 VPN](vpn-deploy-ras.md) ] 區段中，您安裝並設定您的 VPN 伺服器。 VPN 伺服器在設定期間，您可以新增 RADIUS 共用的密碼在 VPN 伺服器上。 

此程序，在中，您可以使用相同的共用祕密文字字串做為 RADIUS 用戶端，在 NPS 中設定 VPN 伺服器。 使用相同的文字字串，您使用的 VPN 伺服器上，或在 NPS 伺服器和 VPN 伺服器之間的通訊會失敗。

>[!IMPORTANT] 
>當您將新的網路存取伺服器 （VPN 伺服器、 無線存取點、 驗證交換器或撥號伺服器） 加入您的網路時，您必須將伺服器新增在 NPS 中的 RADIUS 用戶端，讓 NPS 感知，而且可以與網路存取伺服器通訊。

**程序：**

1.  在 NPS 伺服器上，在 NPS 主控台中，按兩下**RADIUS 用戶端和伺服器**。

2.  以滑鼠右鍵按一下**RADIUS 用戶端**然後按一下**新增**。 [新增 RADIUS 用戶端] 對話方塊隨即開啟。

3.  確認**啟用此 RADIUS 用戶端**選取核取方塊。

4.  在 **易記名稱**，輸入 VPN 伺服器的顯示名稱。

5.  在 **位址 （IP 或 DNS）**，請輸入 NAS IP 位址或 FQDN。<p>如果您輸入的 FQDN，請按一下**確認**如果您想要確認名稱正確，且對應至有效的 IP 位址。

6.  在 **共用祕密**，執行：

    1.  請確認**手動**已選取。

    2.  輸入您也將輸入 VPN 伺服器的強式的文字字串。

    3.  重新輸入共用的密碼，在 確認共用密碼。

7.  按一下 [確定] 。 VPN 伺服器會出現在清單中的 NPS 伺服器上設定的 RADIUS 用戶端。

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>VPN 連線時，將 NPS 設定為 RADIUS

在此程序中，您將 NPS 設定為 RADIUS 伺服器在您的組織網路上。 在 NPS 中，您必須定義的原則，只允許使用者透過 VPN Server-存取組織/公司網路的特定群組中，然後使用有效的使用者憑證中的 PEAP 驗證要求時，才。

**程序：**

1.  在 NPS 主控台中，在標準的組態中，請確認**撥號或 VPN 連線的 RADIUS 伺服器**已選取。

2.  按一下 **設定 VPN 或撥號**。<p>設定 VPN 或撥號精靈隨即開啟。

3.  按一下 [**虛擬私人網路 (VPN) 連線**，然後按一下**下一步]**。

4.  在 指定撥號或 VPN 伺服器的 RADIUS 用戶端中，選取您在上一個步驟中加入 VPN 伺服器的名稱。<p>例如，如果您的 VPN 伺服器 NetBIOS 名稱 RAS1，請按一下**RAS1**。

5.  按一下 [下一步] 。

6.  在設定驗證方法，完成下列步驟：

    1.  清除**Microsoft 加密驗證版本 2 (MS-CHAPv2)** 核取方塊。

    2.  按一下 **可延伸驗證通訊協定**核取方塊來選取它。

    3.  在類型 （根據存取和網路設定的方法） 中，按一下  **Microsoft:受保護的 EAP (PEAP)**，然後按一下**設定**。<p>編輯受保護的 EAP 內容 對話方塊隨即開啟。

    4.  按一下 **移除**若要移除的保護密碼 (EAP-MSCHAP v2) 的 EAP 類型。

    5.  按一下 **\[新增\]**。 [新增 EAP] 對話方塊隨即開啟。

    6.  按一下 **智慧卡或其他憑證**，然後按一下**確定**。

    7.  按一下 **確定**以關閉 編輯受保護的 EAP 內容。

7.  按一下 [下一步] 。

8.  在指定的使用者群組 中，完成下列步驟：

    1.  按一下 **\[新增\]**。 [選取使用者、 電腦、 服務帳戶或群組] 對話方塊隨即開啟。

    2.  型別**VPN 使用者**然後按一下**確定**。

    3.  按一下 [下一步] 。

9.  在指定的 IP 篩選器，按一下**下一步**。

10. 在指定的加密設定 中，按一下**下一步**。 不進行任何變更。<p>這些設定僅適用於此案例中不支援的 Microsoft 點對點加密 (MPPE) 連線。

11. 在 [指定領域名稱，按一下**下一步]**。

12. 按一下 **完成**即可關閉精靈。

## <a name="autoenroll-the-nps-server-certificate"></a>自動註冊 NPS 伺服器憑證

在此程序中，您手動重新整理群組原則在本機 NPS 伺服器上。 當群組原則重新整理，如果設定憑證自動註冊，而且運作正常，本機電腦是經由自動註冊憑證的憑證授權單位 (CA)。

>[!NOTE]  
>當您重新啟動網域成員電腦上，或使用者登入的網域成員電腦時，將會自動更新群組原則。 此外，定期重新整理群組原則。 根據預設，這個定期重新整理會使用隨機的位移，最多 30 分鐘每隔 90 分鐘。

中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。

**程序：**

1.  在 NPS 中，開啟 Windows PowerShell。

2.  在 Windows PowerShell 提示字元中，輸入**gpupdate**，然後按 ENTER 鍵。

## <a name="next-step"></a>後續步驟
[步驟 5。設定 DNS 和防火牆設定一律開啟 」 VPN](vpn-deploy-dns-firewall.md):在此步驟中，您可以安裝網路原則伺服器 (NPS) 藉由使用 Windows PowerShell 或伺服器管理員新增角色和功能精靈。 您也可以設定來處理所有的驗證、 授權和帳戶處理連接的責任的 NPS 收到來自 VPN 伺服器的要求。



---

