---
title: Always On VPN 疑難排解
description: 本主題提供驗證和疑難排解 Windows Server 2016 中 Always On VPN 部署的指示。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: bbb614886099bf2adc1239a699ef8d904e71be7b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961780"
---
# <a name="troubleshoot-always-on-vpn"></a>Always On VPN 疑難排解 

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

如果您的 Always On VPN 安裝程式無法將用戶端連線到您的內部網路，則原因可能是不正確 VPN 憑證、不正確的 NPS 原則，或用戶端部署腳本或路由及遠端存取的問題。 針對 VPN 連線進行疑難排解和測試的第一個步驟，是瞭解 Always On VPN 基礎結構的核心元件。 

您可以透過數種方式對連接問題進行疑難排解。 針對用戶端問題和一般疑難排解，用戶端電腦上的應用程式記錄非常寶貴。 對於驗證特定的問題，NPS 伺服器上的 NPS 記錄檔可協助您判斷問題來源。

## <a name="error-codes"></a>錯誤碼

### <a name="error-code-800"></a>錯誤碼：800

- **錯誤描述。** 因為嘗試的 VPN 通道失敗，未建立遠端連線。 可能無法連上 VPN 伺服器。 如果此連線嘗試使用 L2TP/IPsec 通道，則 IPsec 協商所需的安全性參數可能未正確設定。

- **可能的原因。** 當 VPN 通道類型為 [**自動**]，且所有 VPN 通道的連線嘗試失敗時，就會發生此錯誤。

- **可能的解決方案：**

    - 如果您知道要用於部署的通道，請將 VPN 的類型設定為 VPN 用戶端上的特定通道類型。

    - 藉由建立具有特定通道類型的 VPN 連線，您的連線仍會失敗，但會導致更多的通道特定錯誤（例如，「對 PPTP 封鎖了 GRE」）。

    - 當無法連線到 VPN 伺服器，或通道連線失敗時，也會發生此錯誤。

- **請確定：**

    - IKE 埠（UDP 埠500和4500）不會遭到封鎖。

    - 用戶端和伺服器上都有正確的 IKE 憑證。

### <a name="error-code-809"></a>錯誤碼：809

- **錯誤描述。**  因為遠端伺服器沒有回應，所以無法建立電腦與 VPN 伺服器之間的網路連線。 這可能是因為您的電腦與遠端伺服器之間的其中一個網路裝置（例如防火牆、NAT、路由器）未設定為允許 VPN 連線。 請洽詢您的系統管理員或服務提供者，以判斷可能造成問題的裝置。

- **可能的原因。** 此錯誤是由 VPN 伺服器或防火牆上已封鎖的 UDP 500 或4500埠所造成。

- **可能的解決方案。** 請確定已允許 UDP 埠500和4500通過用戶端和 RRAS 伺服器之間的所有防火牆。

### <a name="error-code-812"></a>錯誤碼：812

- **錯誤描述。** 無法連接到 Always On VPN。 因為您 RAS/VPN 伺服器設定的原則，連線被禁止。 具體而言，伺服器用來驗證您的使用者名稱和密碼的驗證方法可能與連線設定檔中設定的驗證方法不相符。 請洽詢 RAS 伺服器的系統管理員，並通知他或她這項錯誤。

- **可能的原因：**

    - 此錯誤的常見原因是 NPS 已指定用戶端無法符合的驗證條件。 例如，NPS 可能會指定使用憑證來保護 PEAP 連線，但用戶端正在嘗試使用 EAP-Eap-mschapv2。

    - 當以 RRAS 為基礎的 VPN 伺服器驗證通訊協定設定與 VPN 用戶端電腦不相符時，事件記錄檔20276會記錄到事件檢視器中。

- **可能的解決方案。** 請確定您的用戶端設定符合 NPS 伺服器上指定的條件。

### <a name="error-code-13806"></a>錯誤碼：13806

- **錯誤描述。** IKE 找不到有效的電腦憑證。 請洽詢您的網路安全性系統管理員，以瞭解如何在適當的憑證存放區中安裝有效的憑證。

- **可能的原因。** 當 VPN 伺服器上沒有電腦憑證或根電腦憑證時，通常就會發生此錯誤。

- **可能的解決方案。** 確定在用戶端電腦和 VPN 伺服器上都已安裝此部署中所述的憑證。

### <a name="error-code-13801"></a>錯誤碼：13801

- **錯誤描述。** 不接受 IKE 驗證認證。

- **可能的原因。** 此錯誤通常會在下列其中一種情況下發生：

    - 在 RAS 伺服器上用於 IKEv2 驗證的電腦憑證沒有 [**增強金鑰使用**方法] 底下的 [**伺服器驗證**]。

    - RAS 伺服器上的電腦憑證已過期。

    - 用來驗證 RAS 伺服器憑證的根憑證不存在於用戶端電腦上。

    - 用戶端電腦上使用的 VPN 伺服器名稱與伺服器憑證的**subjectName**不相符。

- **可能的解決方案。** 確認伺服器憑證在 [**增強金鑰使用**方法] 下包含 [**伺服器驗證**]。 確認伺服器憑證仍然有效。 確認所使用的 CA 列于 RRAS 伺服器上的 [**受信任的根憑證授權**單位] 底下。 確認 VPN 用戶端會使用 vpn 伺服器憑證上所顯示的 VPN 伺服器的 FQDN 來進行連線。

### <a name="error-code-0x80070040"></a>錯誤碼：0x80070040

- **錯誤描述。** 伺服器憑證沒有**伺服器驗證**做為其中一個憑證使用方式專案。

- **可能的原因。** 如果 RAS 伺服器上沒有安裝伺服器驗證憑證，就會發生此錯誤。

- **可能的解決方案。** 請確定 RAS 伺服器用於**IKEv2**的電腦憑證具有**伺服器驗證**，做為其中一個憑證使用方式專案。

### <a name="error-code-0x800b0109"></a>錯誤碼：0x800B0109

一般而言，VPN 用戶端機器會聯結至以 Active Directory 為基礎的網域。 如果您使用網域認證來登入 VPN 伺服器，憑證會自動安裝在「信任的根憑證授權」存放區中。 不過，如果電腦未加入網域，或如果您使用替代的憑證鏈，您可能會遇到此問題。

- **錯誤描述。** 憑證鏈已處理，但在信任提供者不信任的根憑證中終止。

- **可能的原因。** 如果用戶端電腦上的 [信任的根憑證授權單位] 存放區中未安裝適當的受根信任 CA 憑證，則可能會發生此錯誤。

- **可能的解決方案。** 確定 [信任的根憑證授權單位] 存放區中的用戶端電腦上已安裝根憑證。

## <a name="logs"></a>記錄

### <a name="application-logs"></a>應用程式記錄

用戶端電腦上的應用程式記錄檔會記錄 VPN 線上活動的大部分較高層級詳細資料。

尋找來自來源 RasClient 的事件。 所有錯誤訊息都會在訊息結尾傳回錯誤碼。 以下詳述一些較常見的錯誤碼，但完整清單可在[路由及遠端存取錯誤碼](/previous-versions//mt728163(v=technet.10))中取得。

## <a name="nps-logs"></a>NPS 記錄檔

NPS 會建立並儲存 NPS 帳戶處理記錄。 根據預設，這些檔案會以 Xxxx 中名為的檔案儲存在% SYSTEMROOT% System32 記錄檔中 \\ \\ \\ ，其中*xxxx*是檔案的建立日期。* *

根據預設，這些記錄是以逗點分隔值格式，但不包含標題資料列。 標題資料列為：

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

如果您將此標題列貼入記錄檔的第一行，然後將該檔案匯入 Microsoft Excel 中，資料行會適當地加上標籤。

NPS 記錄檔有助於診斷與原則相關的問題。 如需 NPS 記錄的詳細資訊，請參閱[解讀 Nps 資料庫格式記錄](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771748(v=ws.10))檔。

## <a name="vpn_profileps1-script-issues"></a>VPN_Profile.ps1 指令碼問題

手動執行 VPN_ Profile.ps1 腳本時最常見的問題包括：

- 您是否使用遠端連線工具？  請確定不使用 RDP 或其他遠端連線方法，因為它混亂使用者登入偵測。

- 使用者是該本機電腦的系統管理員嗎？  請確定在執行 VPN_Profile.ps1 腳本時，使用者具有系統管理員許可權。

- 您是否已啟用額外的 PowerShell 安全性功能？ 請確定 PowerShell 執行原則不會封鎖腳本。 執行腳本之前，您可以考慮關閉限制語言模式（如果已啟用）。 您可以在腳本順利完成後啟動限制語言模式。

## <a name="always-on-vpn-client-connection-issues"></a>Always On VPN 用戶端連線問題

較小的錯誤設定可能會導致用戶端連線失敗，而且可能會很難找出原因。  在建立連線之前，Always On VPN 用戶端會經歷數個步驟。 針對用戶端連線問題進行疑難排解時，請使用下列程式來完成消除程式：

1. 範本機器是否已外部連線？ **Whatismyip**掃描應該會顯示不屬於您的公用 IP 位址。

2. 您可以將遠端存取/VPN 伺服器名稱解析成 IP 位址嗎？ 在 [**控制台**] 的  >  [**網路**和網際網路網路**連線**] 中  >  ** **，開啟您的 VPN 設定檔的屬性。 [**一般**] 索引標籤中的值應該可透過 DNS 公開解析。

3. 您可以從外部網路存取 VPN 伺服器嗎？ 請考慮開啟外部介面的網際網路控制訊息通訊協定（ICMP），並從遠端用戶端 ping 名稱。 Ping 成功之後，您可以移除 ICMP 允許規則。

4. 您是否已正確設定 VPN 伺服器上的內部和外部 Nic？ 它們位於不同的子網嗎？ 外部 NIC 會連線到您防火牆上的正確介面嗎？

5. 從用戶端開啟 UDP 500 和4500埠到 VPN 伺服器的外部介面嗎？ 檢查用戶端防火牆、伺服器防火牆和任何硬體防火牆。 IPSEC 使用 UDP 埠500，因此請確定您未在任何地方停用或封鎖 IPEC。

6. 憑證驗證是否失敗？ 確認 NPS 伺服器具有可服務 IKE 要求的伺服器驗證憑證。 請確定您已將正確的 VPN 伺服器 IP 指定為 NPS 用戶端。 請確定您是使用 PEAP 進行驗證，且受保護的 EAP 屬性只允許使用憑證進行驗證。 您可以檢查 NPS 事件記錄檔中是否有驗證失敗。 如需詳細資訊，請參閱[安裝和設定 NPS 伺服器](vpn-deploy-nps.md)。

7. 您是否正在連線，但沒有網際網路/區域網路存取權？ 檢查您的 DHCP/VPN 伺服器 IP 集區是否有設定問題。

8. 您是否連線並擁有有效的內部 IP，但沒有本機資源的存取權？  確認用戶端知道如何取得這些資源。 您可以使用 VPN 伺服器來路由傳送要求。

## <a name="azure-ad-conditional-access-connection-issues"></a>Azure AD 條件式存取連線問題

### <a name="oops---you-cant-get-to-this-yet"></a>糟糕-您還無法進入此

- **錯誤描述。** 當條件式存取原則不符合時，會封鎖 VPN 連線，但會在使用者選取**X**以關閉訊息後進行連接。  選取 **[確定]** 會導致另一個驗證嘗試，這會在另一個「糟糕」訊息中結束。 這些事件會記錄在用戶端的 AAD 操作事件記錄檔中。

- **可能的原因**

  - 使用者的個人憑證存儲中具有有效的用戶端驗證憑證，但 Azure AD 不發出。

  - [VPN 設定檔] \<TLSExtensions\> 區段遺失，或未包含** \<EKUName\> Aad 條件式存取 \</EKUName\> \<EKUOID\> 1.3.6.1.4.1.311.87，</Ekuoid \> \<EKUName> aad 條件存取</ekuname \> \<EKUOID\> 1.3.6.1.4.1.311.87< \> /ekuoid**專案。 \<EKUName>和 \<EKUOID> 專案會告訴 vpn 用戶端，在將憑證傳遞給 vpn 伺服器時，要從使用者的憑證存放區中取出哪一個憑證。 如果沒有這麼做，VPN 用戶端會使用使用者憑證存放區中任何有效的用戶端驗證憑證，而且驗證會成功。 

  - RADIUS 伺服器（NPS）尚未設定為只接受包含**AAD 條件式存取**OID 的用戶端憑證。

- **可能的解決方案。** 若要取消此迴圈，請執行下列動作：

  1. 在 Windows PowerShell 中，執行**WmiObject** Cmdlet 來傾印 VPN 設定檔設定。 
  2. 確認 **\<TLSExtensions>** 、 **\<EKUName>** 和 **\<EKUOID>** 區段存在，並顯示正確的名稱和 OID。
      
      ```powershell
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

  3. 若要判斷使用者的憑證存放區中是否有有效的憑證，請執行**Certutil**命令：

     ```powershell
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
     >如果使用者的個人存放區中有來自簽發者**CN = MICROSOFT VPN 根 CA gen 1**的憑證，但是使用者藉由選取**X**以關閉糟糕訊息來取得存取權，請收集 CAPI2 事件記錄檔，以確認用來驗證的憑證是否為不是從 Microsoft VPN 根 CA 發行的有效用戶端驗證憑證。

  4. 如果使用者的個人存放區中有有效的用戶端驗證憑證，則在使用者選取**X**之後，如果 **\<TLSExtensions>** 、 **\<EKUName>** 和 **\<EKUOID>** 區段存在且包含正確的資訊，連接就會失敗（如其所示）。
   
     此時會出現一個錯誤訊息，指出「找不到可以搭配可延伸驗證通訊協定使用的憑證」。

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>無法從 VPN 連線分頁刪除憑證

- **錯誤描述。** 無法刪除 VPN 連線分頁上的憑證。

- **可能的原因。** 憑證設定為 [**主要**]。

- **可能的解決方案。**

    1. 在 [VPN 連線] 分頁中，選取憑證。
    2. 在 [**主要**] 底下，選取 [**否**]，然後選取 [**儲存**]。
    3. 在 [VPN 連線] 分頁中，再次選取憑證。
    4. 選取 [刪除]。
