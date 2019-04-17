---
title: 疑難排解 Always On VPN
description: 本主題提供確認及疑難排解 Always On VPN 部署 Windows Server 2016 中的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067157"
---
# 疑難排解 Always On VPN 

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

如果您 Always On VPN 設定失敗來連線到您的內部網路的用戶端，原因可能是無效的 VPN 憑證、 不正確的 NPS 原則或使用用戶端部署指令碼，或在路由及遠端存取的問題。 疑難排解和測試您的 VPN 連線的第一個步驟了解 Always On VPN 基礎結構的核心元件。 

您可以針對數種方式連線問題進行疑難排解。 對於用戶端問題和一般疑難排解，用戶端電腦上的應用程式記錄檔是重要的。 驗證特定問題，NPS 伺服器上的 NPS 記錄檔，也可以協助您判斷問題的來源。


## 錯誤碼


### 錯誤碼： 800

-   **錯誤描述。** 遠端連線不進行，因為嘗試的 VPN 通道失敗。 VPN 伺服器可能會無法連線。 如果此連線嘗試使用 L2TP/IPsec 通道，IPsec 交涉可能未正確設定所需的安全性參數。


-   **可能的原因。** VPN 通道類型會**自動**執行，並針對所有 VPN 通道的連線嘗試失敗時，就會發生這個錯誤。

-   **可能的解決方案：**

    -   如果您知道哪些通道以用於您的部署，設定到該特定通道類型的 VPN 類型，VPN 用戶端上。

    -   藉由特定的通道類型的 VPN 連線，您的連線仍將會失敗，但它將會導致更通道特定錯誤 (例如，「 GRE 封鎖 PPTP 」)。

    -   無法連線到 VPN 伺服器或通道連線失敗時，也會發生這個錯誤。

-   **請確定：**

    -   IKE 連接埠 （UDP ports500 和 4500） 不封鎖。

    -   Ike 正確的憑證會出現在用戶端和伺服器上。

### 錯誤碼： 809

-   **錯誤描述。**  無法建立您的電腦，且 VPN 伺服器之間的網路連線，因為沒有回應的遠端伺服器。 這可能是因為網路裝置的其中一個 \ (例如，防火牆，NAT，routers\) 您的電腦和遠端伺服器之間未設定為允許 VPN 連線。 請連絡您的系統管理員或您的服務提供者用來判斷哪一種裝置可能會造成問題。

-   **可能的原因。** 此錯誤的原因是藉由封鎖的 UDP 500 或 4500 連接埠上的 VPN 伺服器或防火牆。

-   **可能的解決方案。** 請確定 UDP ports500 和 4500 允許透過用戶端和 RRAS 伺服器之間的所有防火牆。 
    
    
### 錯誤碼： 812

-   **錯誤描述。** 無法連線到 Always On VPN。 因為您 RAS/VPN 伺服器上設定的原則，所以無法連線。 具體而言，驗證方法伺服器用來確認您的使用者名稱和密碼可能不符合在您的連線設定檔中設定的驗證方法。 請連絡 RAS 伺服器的系統管理員，並通知他或她這個錯誤。

-   **可能的原因：**

    -  這個錯誤的典型的原因是 NPS 所指定無法符合用戶端的驗證狀況。 例如，NPS 可能指定使用的憑證，來保護 PEAP 連線，但用戶端嘗試使用 Eap-mschapv2。

    -  當 RRAS 型 VPN 伺服器驗證通訊協定設定不符合，VPN 用戶端電腦時，事件記錄檔 20276 已經登事件檢視器。

-   **可能的解決方案。** 請確定您的用戶端設定符合 NPS 伺服器同時指定的條件。


### 錯誤碼： 13806

-   **錯誤描述。** IKE 找不到正確的電腦憑證。 請連絡您的網路安全性系統管理員有關安裝適當憑證存放區中的有效的憑證。

-   **可能的原因。** 當沒有機器憑證時，通常會發生此錯誤，或根電腦憑證存在 VPN 伺服器上。

-   **可能的解決方案。** 確保 VPN 伺服器和用戶端電腦上安裝此部署中所述的憑證。

### 錯誤碼： 13801

-   **錯誤描述。** IKE 驗證認證都無法接受。

-   **可能的原因。** 這個錯誤通常會發生下列情況的其中一種：

    -   用於 IKEv2 驗證 RAS 伺服器上的電腦憑證不會有**伺服器驗證**下**增強金鑰使用方法**。

    -   RAS 伺服器上的電腦憑證已到期。

    -   若要驗證 RAS 伺服器憑證的根憑證不存在用戶端電腦上。

    -   使用用戶端電腦上的 VPN 伺服器名稱不相符的伺服器憑證**subjectName** 。

-   **可能的解決方案。** 確認伺服器憑證包含下**增強金鑰使用方法**的**伺服器驗證**。 確認伺服器憑證仍然有效。 驗證用的 CA 列在**受信任的根憑證授權單位**RRAS 伺服器上。 確認 VPN 用戶端連線的 VPN 伺服器憑證上呈現的方式使用 VPN 伺服器的 FQDN。


### 錯誤碼： 0x80070040

-   **錯誤描述。** 伺服器憑證做為其中一個其憑證使用方式的項目沒有**伺服器驗證**。

-   **可能的原因。** 如果沒有伺服器驗證憑證 RAS 伺服器上安裝，可能會發生這個錯誤。

-   **可能的解決方案。** 請確定針對**IKEv2**使用 RAS 伺服器的電腦憑證做為其中一個憑證用法項目有**伺服器驗證**。

### 錯誤碼： 0x800B0109

一般而言，VPN 用戶端電腦已加入 Active Directory 型的網域。 如果您使用的網域認證來登入 VPN 伺服器時，將憑證就會自動安裝受信任的根憑證授權單位中儲存。 不過，如果電腦未加入網域，或如果您使用替代的憑證鏈結，您可能會遇到這個問題。

-   **錯誤描述。** 憑證鏈結已處理，但在信任提供者不信任的根憑證中終止。

-   **可能的原因。** 如果適當受信任的根 CA 憑證中不會安裝受信任的根憑證授權單位，可能會發生這個錯誤儲存在用戶端電腦上。

-   **可能的解決方案。** 請確定在受信任的根憑證授權單位存放區中的用戶端電腦上已安裝的根憑證。

## 記錄檔

### 應用程式記錄檔

用戶端電腦上的應用程式記錄檔錄製大多數的 VPN 連線事件較高層級的詳細資料。

從來源 RasClient 尋找事件。 所有的錯誤訊息傳回訊息的結尾的錯誤碼。 一些比較常見的錯誤代碼中有詳細說明如下，但使用[路由及遠端存取錯誤碼](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx)的完整清單。

## NPS 記錄檔
NPS 會建立並儲存 NPS 計量記錄檔。 根據預設，這些會儲存在檔案中名為中 %SYSTEMROOT%\\System32\\Logfiles\\*XXXX。* txt，其中*XXXX*是日期建立檔案。

根據預設，這些記錄檔以逗點分隔值的格式，但它們不包含標題列中。 標題列是：

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

如果您的記錄檔的第一行貼上此標題列，然後將檔案匯入 Microsoft Excel，欄會正確標示為。

NPS 記錄檔可協助診斷原則相關的問題。 如需有關 NPS 記錄檔的詳細資訊，請參閱[解譯 NPS 資料庫格式記錄檔](https://technet.microsoft.com/library/cc771748.aspx)。

## VPN_Profile.ps1 指令碼問題
最常見的問題時手動執行 VPN_ Profile.ps1 指令碼包括：

- 您使用的遠端連線工具？  請確定不使用 RDP 或遠端連線的另一個方法，因為它破壞有使用者登入偵測。

- 是使用者的本機電腦的系統管理員？  請確定 VPN_Profile.ps1 指令碼執行時使用者具有系統管理員權限。

- 您有其他啟用 PowerShell 的安全性功能？ 請確定 PowerShell 執行原則不會封鎖指令碼。 您可以考慮將受限於語言模式中，關閉，如果啟用，才能執行指令碼。 指令碼順利完成之後，您可以啟用受限於語言模式。

## Always On VPN 用戶端連線問題
小型錯誤設定可能會造成用戶端連線失敗，並可以尋找原因挑戰性更高。  Always On VPN 用戶端會通過連線之前的幾個步驟。 當用戶端連線問題進行疑難排解，瀏覽的抵銷使用下列程序：


1. 是範本外部電腦連線？ **Whatismyip**掃描應該會顯示不屬於您的公用 IP 位址。

2. 您可以解析 IP 位址的遠端存取 VPN 伺服器名稱？ 在 [**控制台** \> **網路**和**網際網路** \> **網路連線**，開啟您的 VPN 設定檔的內容。 [**一般**] 索引標籤中的值應該是可公開透過 DNS 解析。

3. 您可以從外部網路存取 VPN 伺服器嗎？ 請考慮到外部介面開啟網際網路控制訊息通訊協定 (ICMP) 以及 ping 從遠端用戶端的名稱。 Ping 成功後，您可以移除 ICMP 允許規則。

4. 您已正確設定的 VPN 伺服器上有內部和外部 Nic？ 它們是在不同子網路中嗎？ 不連接至正確的介面上的防火牆外部 NIC？

5. UDP 500 和是 4500 連接埠開啟來自用戶端 VPN 伺服器的外部介面嗎？ 檢查用戶端防火牆、 伺服器防火牆和任何硬體防火牆。 IPSEC 使用 UDP 連接埠 500，因此請確定您沒有 IPEC 停用或封鎖任何地方。

7. 憑證驗證失敗？ 確認 NPS 伺服器有可以服務 IKE 要求的伺服器驗證憑證。 請確定您擁有正確的 VPN 伺服器 IP 指定為 NPS 用戶端。 請確定您的驗證使用 PEAP 時，並受保護的 EAP 內容應該只允許使用憑證驗證。 您可以檢查驗證失敗 NPS 事件記錄檔。 如需詳細資訊，請參閱[安裝及設定 NPS 伺服器](vpn-deploy-nps.md)

8. 準備進行連線，但無法存取網際網路/鎖定的本機網路？ 檢查設定問題您 DHCP/VPN 伺服器 IP 集區。

9.  您是否有連接與有有效內部 IP 但不是能存取本機資源？  確認用戶端知道如何取得這些資源。 您可以使用路線要求的 VPN 伺服器。


## Azure AD 的條件式存取連線問題

### 糟糕-您無法取得這尚未

-   **錯誤描述。** 當條件式存取原則不滿足之後，請封鎖 VPN 連線，但連線後使用者按下**X**關閉訊息。  按一下 **[確定]** 會導致另一個驗證嘗試，在另一個_Oops_訊息結束。 這些事件會記錄在用戶端的 AAD 操作事件記錄檔中。 

-   **可能的原因。** 

    - 使用者會有 Azure AD 所簽發不有效的用戶端驗證憑證在其個人憑證存放區。

    - VPN 設定檔 \ < TLSExtensions\ > 區段會遺失或不含**\ < EKUName\ > AAD 條件式 Access\ < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ > \ < EKUName > AAD 條件式存取< / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ >** 項目。 \ < EKUName > 和 \ < EKUOID > 項目告訴 VPN 用戶端来擷取使用者的憑證存放區中時傳送的 VPN 伺服器憑證的憑證。 沒有這麼做，VPN 用戶端會使用任何有效的用戶端驗證憑證是在使用者的憑證存放區，並驗證成功。 

    - RADIUS 伺服器 (NPS) 尚未設定為只接受包含**AAD 條件式存取**OID 的用戶端憑證。

-   **可能的解決方案。** 逸出這個迴圈中，執行下列動作：

    1. 在 Windows PowerShell 中，執行**Get-wmiobject** cmdlet，傾印 VPN 設定檔設定。 
    2. 確認**\ < TLSExtensions >**， **\ < EKUName >**，並**\ < EKUOID >** 章節存在，並顯示正確的名稱和 OID。 
        ```
        PS C:\> Get-WmiObject -Class MDM_VPNv2_01 -Namespace root\cimv2\mdm\dmmap

        __GENUS                 : 2
        __CLASS                 : MDM_VPNv2_01
        __SUPERCLASS            :
        __DYNASTY               : MDM_VPNv2_01
        __RELPATH               : MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VPNv2"
        __PROPERTY_COUNT        : 10
        __DERIVATION            : {}
        __SERVER                : DERS2
        __NAMESPACE             : root\cimv2\mdm\dmmap
        __PATH                  : \\DERS2\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VP
                                    Nv2"
        AlwaysOn                :
        ByPassForLocal          :
        DnsSuffix               :
        EdpModeId               :
        InstanceID              : AlwaysOnVPN
        LockDown                :
        ParentID                : ./Vendor/MSFT/VPNv2
        ProfileXML              : <VPNProfile><RememberCredentials>false</RememberCredentials><DeviceCompliance><Enabled>true</
                                    Enabled><Sso><Enabled>true</Enabled></Sso></DeviceCompliance><NativeProfile><Servers>derras2.
                                    corp.deverett.info;derras2.corp.deverett.info</Servers><RoutingPolicyType>ForceTunnel</Routin
                                    gPolicyType><NativeProtocolType>Ikev2</NativeProtocolType><Authentication><UserMethod>Eap</Us
                                    erMethod><MachineMethod>Eap</MachineMethod><Eap><Configuration><EapHostConfig
                                    xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config
                                    xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.
                                    com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.mic
                                    rosoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptFor
                                    ServerValidation>true</DisableUserPromptForServerValidation><ServerNames></ServerNames></Serv
                                    erValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Ea
                                    p xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type>
                                    <EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><Credenti
                                    alsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore
                                    ></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUse
                                    rPromptForServerValidation><ServerNames></ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b
                                    1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>fal
                                    se</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/E
                                    apTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://ww
                                    w.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName><TLSExtens
                                    ions
                                    xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xml
                                    ns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><
                                    EKUName>AAD Conditional
                                    Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList
                                    Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></Client
                                    AuthEKUList></FilteringInfo></TLSExtensions></EapType></Eap><EnableQuarantineChecks>false</En
                                    ableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><Perfo
                                    rmServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2"
                                    >false</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisionin
                                    g/MsPeapConnectionPropertiesV2">false</AcceptServerName></PeapExtensions></EapType></Eap></Co
                                    nfig></EapHostConfig></Configuration></Eap></Authentication></NativeProfile></VPNProfile>
        RememberCredentials     : False
        TrustedNetworkDetection :
        PSComputerName          : DERS2
        ```

    3. 若要判斷使用者的憑證存放區是否有有效的憑證，請執行**Certutil**命令：

       ```
       C:\>certutil -store -user My

        My "Personal"
        ================ Certificate 0 ================
        Serial Number: 32000000265259d0069fa6f205000000000026
        Issuer: CN=corp-DEDC0-CA, DC=corp, DC=deverett, DC=info
         NotBefore: 12/8/2017 8:07 PM
         NotAfter: 12/8/2018 8:07 PM
        Subject: E=winfed@deverett.info, CN=WinFed, OU=Users, OU=Corp, DC=corp, DC=deverett, DC=info
        Certificate Template Name (Certificate Type): User
        Non-root Certificate
        Template: User
        Cert Hash(sha1): a50337ab015d5612b7dc4c1e759d201e74cc2a93
          Key Container = a890fd7fbbfc072f8fe045e680c501cf_5834bfa9-1c4a-44a8-a128-c2267f712336
          Simple container name: te-User-c7bcc4bd-0498-4411-af44-da2257f54387
          Provider = Microsoft Enhanced Cryptographic Provider v1.0
        Encryption test passed
        
        ================ Certificate 1 ================
        Serial Number: 367fbdd7e6e4103dec9b91f93959ac56
        Issuer: CN=Microsoft VPN root CA gen 1
         NotBefore: 12/8/2017 6:24 PM
         NotAfter: 12/8/2017 7:29 PM
        Subject: CN=WinFed@deverett.info
        Non-root Certificate
        Cert Hash(sha1): 37378a1b06dcef1b4d4753f7d21e4f20b18fbfec
          Key Container = 31685cae-af6f-48fb-ac37-845c69b4c097
          Unique container name: bf4097e20d4480b8d6ebc139c9360f02_5834bfa9-1c4a-44a8-a128-c2267f712336
          Provider = Microsoft Software Key Storage Provider
        Private key is NOT exportable
        Encryption test passed
       ```
       >[!NOTE]
       >如果是從簽發者憑證**CN = Microsoft VPN 根 CA 產生 1**是出現在使用者的個人存放區，但使用者按一下**X**關閉 Oops 訊息、 收集 CAPI2 事件記錄檔以取得存取驗證用來驗證的憑證有效用戶端驗證憑證不從 Microsoft VPN 根 CA 發行。

   4. 有效的用戶端驗證憑證存在於使用者的個人存放區中，如果連接失敗 （因為它應該） 在使用者按一下**X**之後，如果**\ < TLSExtensions >**， **\ < EKUName >**，並**\ < EKUOID >** 區段存在並包含正確的資訊。<p>_憑證找不到可用的可延伸驗證通訊協定。_ 此時會出現錯誤訊息。

### 無法從 VPN 連線刀鋒視窗中刪除憑證

-   **錯誤描述。** 無法刪除 VPN 連線刀鋒視窗上的憑證。

-   **可能的原因。** 憑證會設定為**主要**。

-   **可能的解決方案。** 

    1. 在 VPN 連線刀鋒視窗中，選取的憑證。
    2. 在**主要**，選取 [**否]** ，按一下 [**儲存**]。
    3. 在 VPN 連線刀鋒視窗中，請再次選取的憑證。
    4. 按一下 [**刪除**。


---
