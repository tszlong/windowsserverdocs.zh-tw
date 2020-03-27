---
title: 安裝並設定 NPS 伺服器
description: NPS 伺服器處理 VPN 伺服器所傳送的連線要求時，會驗證使用者是否有連線的許可權、使用者的身分識別，並記錄您在 NPS 中設定 RADIUS 帳戶處理時所選擇的連線要求層面。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 08/30/2018
ms.openlocfilehash: 18fa85189b082a4a88a8a0bc0d6df11e21e7c97d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307692"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>步驟 4. 安裝和設定網路原則伺服器（NPS）

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**下一步：** 步驟3。設定 Always On VPN 的遠端存取服務器](vpn-deploy-ras.md)
- [**下一步：** 步驟5。設定 DNS 和防火牆設定](vpn-deploy-dns-firewall.md)

在此步驟中，您將安裝網路原則伺服器（NPS），以處理 VPN 伺服器所傳送的連線要求：

- 執行授權以確認使用者具有連接的許可權。
- 執行驗證來驗證使用者的身分識別。
- 執行帳戶處理，以記錄您在 NPS 中設定 RADIUS 帳戶處理時所選擇的連線要求層面。

本節中的步驟可讓您完成下列專案：

1. 在規劃 NPS 伺服器並安裝在您的組織或公司網路上的電腦或 VM 上，您可以安裝 NPS。

   >[!TIP]
   >如果您的網路上已有一或多個 NPS 伺服器，則不需要執行 NPS 伺服器安裝-相反地，您可以使用本主題來更新現有 NPS 伺服器的設定。

> [!NOTE]
> 您不能在 Windows Server Core 上安裝網路原則伺服器服務。

2. 在組織/公司 NPS 伺服器上，您可以將 NPS 設定為以 RADIUS 伺服器的形式執行，以處理從 VPN 伺服器接收的連線要求。

## <a name="install-network-policy-server"></a>安裝網路原則伺服器

在此程式中，您會使用 Windows PowerShell 或伺服器管理員新增角色及功能嚮導 來安裝 NPS。 NPS 是網路原則與存取服務伺服器角色的角色服務。

>[!TIP]
>根據預設值，NPS 會為所有已安裝的網路介面卡接聽連接埠 1812、1813、1645 和 1646 上的 RADIUS 流量。 當您安裝 NPS，並啟用具有 Advanced Security 的 Windows 防火牆時，會針對 IPv4 和 IPv6 流量自動建立這些埠的防火牆例外。 如果您的網路存取伺服器設定為透過非預設的埠來傳送 RADIUS 流量，請移除在 NPS 安裝期間，在 [具有 Advanced Security 的 Windows 防火牆] 中建立的例外狀況，並為您要使用的埠建立例外。RADIUS 流量。

**Windows PowerShell 的程式：**

若要使用 Windows PowerShell 執行此程式，請以系統管理員身分執行 Windows PowerShell，輸入下列 Cmdlet：

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**伺服器管理員的程式：**

1.  在伺服器管理員中，選取 [**管理**]，然後選取 [**新增角色及功能**]。
        [新增角色及功能精靈] 隨即開啟。

2.  在 [開始之前] 中，選取 **[下一步]** 。

    >[!NOTE] 
    >如果您先前在執行 [新增角色及功能嚮導] 時選取 [**略過此頁面**]，則不會顯示 [新增角色及功能] 的 [**開始之前**] 頁面。

3.  在 [選取安裝類型] 中，確定已選取 [以**角色為基礎] 或 [功能型安裝**]，然後選取 **[下一步]** 。

4.  在 [選取目的地伺服器] 中，確定已選取 **[從伺服器集區選取伺服器**]。

5.  在 [伺服器集區] 中，確定已選取本機電腦，然後選取 **[下一步]** 。

6.  在 [選取伺服器角色] 的 [**角色**] 中，選取 [**網路原則與存取服務**]。 隨即會開啟對話方塊，詢問它是否應新增網路原則與存取服務所需的功能。

7.  選取 [**新增功能**]，然後選取 **[下一步]**

8.  在 選取功能 中選取**下一步**，然後在 網路原則與存取服務 中，檢查提供的資訊，然後選取**下一步**

9.  在 [選取角色服務] 中，選取 [**網路原則伺服器**]。

10. 若為網路原則伺服器所需的功能，請選取 [**新增功能**]，然後選取 **[下一步]** 。

11. 在 [確認安裝選項] 中，選取 **[必要時自動重新開機目的地伺服器**]。

12. 選取 **[是]** 確認選取的，然後選取 [**安裝**]。
    
    [安裝進度] 頁面會在安裝過程中顯示狀態。 當程式完成時，會顯示「在*ComputerName*上安裝成功」訊息，其中*ComputerName*是您安裝網路原則伺服器的電腦名稱稱。

13. 選取 [關閉]。

## <a name="configure-nps"></a>設定 NPS

安裝 NPS 之後，您可以設定 NPS 以處理它從 VPN 伺服器接收之連線要求的所有驗證、授權和帳戶管理責任。

### <a name="register-the-nps-server-in-active-directory"></a>在 Active Directory 中註冊 NPS 伺服器

在此程式中，您會在 Active Directory 中註冊伺服器，以便在處理連線要求時擁有存取使用者帳戶資訊的許可權。

**步**

1.  在伺服器管理員中，選取 [**工具**]，然後選取 [**網路原則伺服器**]。 NPS 主控台隨即開啟。

2.  在 NPS 主控台中，以滑鼠右鍵按一下 [ **nps （本機）** ]，然後選取 [**在 Active Directory 中註冊伺服器**]。
   
     此時會開啟 [網路原則伺服器] 對話方塊。

3.  在 [網路原則伺服器] 對話方塊中，選取 **[確定]** 兩次。

如需註冊 NPS 的替代方法，請參閱[在 Active Directory 網域中註冊 Nps 伺服器](../../../../../networking/technologies/nps/nps-manage-register.md)。

### <a name="configure-network-policy-server-accounting"></a>設定網路原則伺服器計量

在此程式中，請使用下列其中一種記錄類型來設定網路原則伺服器帳戶處理：

- **事件記錄**。 主要用於稽核和疑難排解連線嘗試問題。 您可以在 NPS 主控台中取得 NPS 伺服器屬性來設定 NPS 事件記錄。

- **將使用者驗證和帳戶處理要求記錄到本機**檔案。 主要用於連線分析及計費用途。 也可用來做為安全性調查工具，因為它提供了一種方法，讓您在攻擊之後追蹤惡意使用者的活動。 您可以使用 [帳戶處理] 設定向導來設定本機檔案記錄。

- **將使用者驗證和帳戶處理要求記錄到 MICROSOFT SQL SERVER XML 相容的資料庫**。 用於允許執行 NPS 的多部伺服器擁有一個資料來源。 還可提供使用關聯式資料庫的優點。 您可以使用 [帳戶處理] 設定向導來設定 SQL Server 記錄。

若要設定網路原則伺服器帳戶處理，請參閱[設定網路原則伺服器帳戶](../../../../../networking/technologies/nps/nps-accounting-configure.md)處理。

### <a name="add-the-vpn-server-as-a-radius-client"></a>新增 VPN 伺服器做為 RADIUS 用戶端

在 [[設定 ALWAYS ON VPN 的遠端存取服務器](vpn-deploy-ras.md)] 區段中，您已安裝並設定您的 VPN 伺服器。 在 VPN 伺服器設定期間，您已在 VPN 伺服器上新增 RADIUS 共用密碼。

在此程式中，您會使用相同的共用密碼文字字串，將 VPN 伺服器設定為 NPS 中的 RADIUS 用戶端。 使用您在 VPN 伺服器上使用的相同文字字串，或 NPS 伺服器與 VPN 伺服器之間的通訊失敗。

>[!IMPORTANT] 
>當您將新的網路存取伺服器（VPN 伺服器、無線存取點、驗證交換器或撥號伺服器）新增至您的網路時，您必須將伺服器新增為 NPS 中的 RADIUS 用戶端，NPS 才能感知並與網路存取伺服器通訊。

**步**

1. 在 NPS 伺服器上的 NPS 主控台中，按兩下 [ **RADIUS 用戶端和伺服器**]。

2. 以滑鼠右鍵按一下 [ **RADIUS 用戶端**]，然後選取 [**新增**]。 [新增 RADIUS 用戶端] 對話方塊隨即開啟。

3. 確認已選取 [**啟用此 RADIUS 用戶端**] 核取方塊。

4. 在 [**易記名稱**] 中，輸入 VPN 伺服器的顯示名稱。

5. 在 [**位址（IP 或 DNS）** ] 中，輸入 NAS IP 位址或 FQDN。
     
     如果您輸入 FQDN，請選取 [**驗證**] 以確認名稱是否正確，並對應至有效的 IP 位址。

6. 在 [**共用密碼**] 中，執行：

    1. 確定已選取 [**手動**]。

    2. 輸入您也在 VPN 伺服器上輸入的強文字字串。

    3. 在 [確認共用密碼] 中重新輸入共用密碼。

7. 選取 **[確定]** 。 VPN 伺服器會出現在 NPS 伺服器上設定的 RADIUS 用戶端清單中。

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>將 NPS 設定為 VPN 連線的 RADIUS

在此程式中，您會將 NPS 設定為組織網路上的 RADIUS 伺服器。 在 NPS 上，您必須定義原則，讓特定群組中的使用者只能透過 VPN 伺服器存取組織/公司網路，然後只在 PEAP 驗證要求中使用有效的使用者憑證時才允許。

**步**

1. 在 NPS 主控台的 [標準設定] 中，確定已選取 [**撥號或 VPN 連線的 RADIUS 伺服器**]。

2. 選取 [**設定 VPN 或撥號**]。
        
    [設定 VPN 或撥號嚮導] 隨即開啟。

3. 選取 [**虛擬私人網路（VPN）** 連線]，然後選取 **[下一步]** 。

4. 在 [指定撥號或 VPN 伺服器] 的 [RADIUS 用戶端] 中，選取您在上一個步驟中新增的 VPN 伺服器名稱。 例如，如果您的 VPN 伺服器 NetBIOS 名稱是 RAS1，請選取 [ **RAS1**]。

5. 選取 [下一步]。

6. 在 [設定驗證方法] 中，完成下列步驟：

    1. 清除 [ **Microsoft 加密驗證第2版（MS-CHAPv2）** ] 核取方塊。

    2. 選取 [可延伸的**驗證通訊協定**] 核取方塊來選取它。

    3. 在 [類型] 中（根據存取和網路設定的方法），選取 [ **Microsoft：受保護的 EAP （PEAP）** ]，然後選取 [**設定**]。
      
        [編輯受保護的 EAP 屬性] 對話方塊隨即開啟。

    4. 選取 [**移除**] 以移除受保護的密碼（EAP-MSCHAP V2） EAP 類型。

    5. 選取 [新增]。 [新增 EAP] 對話方塊隨即開啟。

    6. 選取 [**智慧卡或其他憑證**]，然後選取 **[確定]** 。

    7. 選取 **[確定]** 以關閉 [編輯受保護的 EAP 屬性]。

7. 選取 [下一步]。

8. 在 [指定使用者群組] 中，完成下列步驟：

    1. 選取 [新增]。 [選取使用者、電腦、服務帳戶或群組] 對話方塊隨即開啟。

    2. 輸入**VPN 使用者**，然後選取 **[確定]** 。

    3. 選取 [下一步]。

9. 在 [指定 IP 篩選器 **] 中選取 [下一步]** 。

10. 在 [指定加密設定] 中，選取 **[下一步]** 。 請勿進行任何變更。

    這些設定僅適用于此案例不支援的 Microsoft 點對點加密（MPPE）連線。

11. 在 [指定領域名稱] 中，選取 **[下一步]** 。

12. 選取 **[完成**] 以關閉嚮導。

## <a name="autoenroll-the-nps-server-certificate"></a>自動註冊 NPS 伺服器憑證

在此程式中，您會以手動方式重新整理本機 NPS 伺服器上的群組原則。 當群組原則重新整理時，如果憑證自動註冊已設定且正常運作，則會由憑證授權單位單位（CA）自動將憑證註冊到本機電腦。

>[!NOTE]  
>當您重新開機網域成員電腦，或使用者登入網域成員電腦時，群組原則自動重新整理。 此外，群組原則定期重新整理。 根據預設，此定期重新整理會每隔90分鐘執行一次，隨機位移最長為30分鐘。

若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。

**步**

1. 在 NPS 上，開啟 Windows PowerShell。

2. 在 Windows PowerShell 命令提示字元中，輸入**gpupdate**，然後按 enter。

## <a name="next-steps"></a>後續步驟

[步驟5。設定 Always On VPN 的 DNS 和防火牆設定](vpn-deploy-dns-firewall.md)：在此步驟中，您會使用 Windows PowerShell 或伺服器管理員新增角色及功能嚮導來安裝網路原則伺服器（NPS）。 您也可以設定 NPS，以處理從 VPN 伺服器接收的連線要求的所有驗證、授權和帳戶管理責任。