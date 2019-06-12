---
title: 疑難排解 Always On VPN
description: 本主題提供驗證和 Windows Server 2016 中的一律開啟 」 VPN 部署進行疑難排解的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d9e0efede39f5a8189dbb3d62033210c393c424d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749651"
---
# <a name="troubleshoot-always-on-vpn"></a>疑難排解 Always On VPN 

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

如果您的一律開啟 」 VPN 設定為將用戶端連接到您的內部網路，原因可能是無效的 VPN 憑證、 不正確的 NPS 原則或使用用戶端部署指令碼或路由及遠端存取的問題。 疑難排解和測試您的 VPN 連線的第一個步驟了解一律開啟 」 VPN 基礎結構的核心元件。 

您可以針對數種方式的連線問題進行疑難排解。 用戶端的問題和一般的疑難排解，用戶端電腦上的應用程式記錄是非常寶貴。 如需驗證特定問題，NPS 伺服器上的 NPS 記錄檔可協助您判斷問題的來源。

## <a name="error-codes"></a>錯誤碼

### <a name="error-code-800"></a>錯誤碼：800

- **錯誤描述。** 不進行遠端的連線，因為嘗試的 VPN 通道失敗。 您可能無法連線到 VPN 伺服器。 如果此連線嘗試使用 L2TP/IPsec 通道，交涉可能未正確設定的 IPsec 所需的安全性參數。

- **可能的原因。** VPN 通道型別時，就會發生此錯誤**自動**和所有 VPN 通道連線嘗試都失敗。

- **可能的解決方案：**

    - 如果您知道哪些通道，以用於部署，設定為該特定通道類型的 VPN 類型，VPN 用戶端上。

    - 藉由特定通道型別透過 VPN 連線，連線還是會失敗，，但它會導致更多的通道特有的錯誤 (例如，"GRE 封鎖 PPTP")。

    - 無法連線到 VPN 伺服器或通道連線失敗時，也會發生此錯誤。

- **請確定：**

    - IKE 連接埠 （UDP 連接埠 500 和 4500），不會封鎖。

    - 正確的憑證，ike 會在用戶端和伺服器上。

### <a name="error-code-809"></a>錯誤碼：809

- **錯誤描述。**  無法建立您的電腦和 VPN 伺服器之間的網路連線，因為遠端伺服器沒有回應。 這可能是因為您的電腦和遠端伺服器之間的網路裝置 （例如防火牆、 NAT 路由器） 的其中一個未設定為允許 VPN 連線。 請連絡您的系統管理員或服務提供者，以判斷哪一種裝置可能會造成問題。

- **可能的原因。** 此錯誤被因為封鎖的 UDP 500 或 VPN 伺服器或防火牆上的 4500 連接埠。

- **可能的解決方案。** 確保透過用戶端與 RRAS 伺服器之間的所有防火牆允許 UDP 連接埠 500 和 4500。

### <a name="error-code-812"></a>錯誤碼：812

- **錯誤描述。** 無法連線到一律開啟 」 VPN。 因為您 RAS/VPN 伺服器上設定原則，因此已防止連線。 具體而言，驗證方法的伺服器用來驗證您的使用者名稱和密碼可能不符合您的連線設定檔中設定的驗證方法。 請連絡 RAS 伺服器的系統管理員，並通知他或她的這項錯誤。

- **可能的原因：**

    - 此錯誤的一般原因是，NPS 具有指定的用戶端無法符合驗證條件。 比方說，NPS 可指定 PEAP 保護連線安全的憑證的使用，但用戶端正在嘗試使用 EAP-MSCHAPv2。

    - RRAS 為基礎的 VPN 伺服器驗證通訊協定設定不符合 VPN 用戶端電腦的事件記錄檔 20276 會記錄到事件檢視器。

- **可能的解決方案。** 請確認您的用戶端設定符合 NPS 伺服器指定的條件。

### <a name="error-code-13806"></a>錯誤碼：13806

- **錯誤描述。** IKE 無法找不到有效的電腦憑證。 請連絡您的網路安全性系統管理員在適當的憑證存放區中安裝有效憑證的相關。

- **可能的原因。** 當沒有電腦憑證時，通常就會發生此錯誤，或根機器憑證存在於 VPN 伺服器上。

- **可能的解決方案。** 請確定此部署中所述的憑證會安裝在用戶端電腦和 VPN 伺服器。

### <a name="error-code-13801"></a>錯誤碼：13801

- **錯誤描述。** IKE 驗證認證就是無法接受的。

- **可能的原因。** 此錯誤通常是發生在下列案例之一：

    - 電腦憑證用於 IKEv2 驗證 RAS 伺服器上沒有**伺服器驗證**下方**增強金鑰使用方法**。

    - RAS 伺服器上的電腦憑證已過期。

    - 若要驗證的 RAS 伺服器憑證的根憑證不存在用戶端電腦上。

    - 使用用戶端電腦上的 VPN 伺服器名稱不符合**subjectName**伺服器憑證。

- **可能的解決方案。** 請確認伺服器憑證包含**伺服器驗證**下方**增強金鑰使用方法**。 請確認伺服器憑證仍然有效。 確認使用的 CA 列底下**受信任的根憑證授權單位**RRAS 伺服器上。 確認 VPN 用戶端連線 VPN 伺服器的憑證上所述，使用 VPN 伺服器的 FQDN。

### <a name="error-code-0x80070040"></a>錯誤碼：0x80070040

- **錯誤描述。** 伺服器憑證沒有**伺服器驗證**作為其憑證使用方式的項目之一。

- **可能的原因。** 如果沒有伺服器驗證憑證已安裝 RAS 伺服器上，可能會發生此錯誤。

- **可能的解決方案。** 請確認電腦憑證 RAS 伺服器會使用針對**IKEv2**已**伺服器驗證**做為其中一個憑證使用方式的項目。

### <a name="error-code-0x800b0109"></a>錯誤碼：0x800B0109

一般而言，VPN 用戶端電腦已加入 Active Directory 為基礎的網域。 如果您使用網域認證來登入 VPN 伺服器時，憑證會自動安裝在受信任的根憑證授權單位存放區。 不過，如果電腦未加入網域，或使用替代的憑證鏈結，您可能會遇到此問題。

- **錯誤描述。** 憑證鏈結處理，但它終止於信任提供者不信任的根憑證。

- **可能的原因。** 如果適當信任的根 CA 憑證未安裝在受信任的根憑證授權單位，可能會發生此錯誤儲存在用戶端電腦上。

- **可能的解決方案。** 請確定在受信任的根憑證授權單位存放區中的用戶端電腦上已安裝根憑證。

## <a name="logs"></a>記錄檔

### <a name="application-logs"></a>應用程式記錄檔

用戶端電腦上的應用程式記錄檔記錄大部分的較高層級的詳細資料的 VPN 連線的事件。

事件來源為 RasClient 外觀。 所有的錯誤訊息傳回訊息結尾處的錯誤程式碼。 一些較常見的錯誤代碼詳述如下，但完整清單位於[路由及遠端存取錯誤碼](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx)。

## <a name="nps-logs"></a>NPS 記錄檔

NPS 會建立並儲存 NPS 帳戶處理記錄檔。 根據預設，這些儲存在 %SYSTEMROOT%\\System32\\Logfiles\\在檔案中名為*XXXX*.txt，其中*XXXX*是檔案的建立的日期。

根據預設，這些記錄是以逗號分隔值格式，但它們不包含標題列。 標題資料列是：

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

如果您將此標題資料列貼為記錄檔的第一行，然後將檔案匯入 Microsoft Excel，正確地標示為資料行。

NPS 記錄檔能幫助您診斷原則相關的問題。 如需 NPS 記錄檔的詳細資訊，請參閱[解譯 NPS 資料庫格式記錄檔](https://technet.microsoft.com/library/cc771748.aspx)。

## <a name="vpnprofileps1-script-issues"></a>VPN_Profile.ps1 指令碼的問題

最常見的問題時以手動方式執行 VPN_ Profile.ps1 指令碼包括：

- 您使用遠端連線工具？  請確定不使用 RDP 或另一種遠端連線方法，因為它會使用使用者登入偵測的異動。

- 使用者是該本機電腦的系統管理員？  請確定執行 VPN_Profile.ps1 指令碼時的使用者具有系統管理員權限。

- 您是否有啟用的其他 PowerShell 安全性功能？ 請確定 PowerShell 執行原則不會封鎖指令碼。 您可以考慮關閉限制語言模式中，如果啟用，才能執行指令碼。 指令碼順利完成之後，您可以啟用限制語言模式。

## <a name="always-on-vpn-client-connection-issues"></a>一律開啟 」 VPN 用戶端連線問題

小型的設定不正確可能會導致用戶端連接失敗，並不容易找出原因。  一律開啟 」 VPN 用戶端會經歷幾個步驟之前建立的連接。 當用戶端連線問題進行疑難排解，經歷消去法使用下列程序：

1. 外部範本電腦已連線？ A **whatismyip**掃描應該會顯示不屬於您的公用 IP 位址。

2. 您可以解決遠端存取 VPN 伺服器名稱為 IP 位址？ 在**控制台中** > **網路**並**網際網路** > **網路連線**，開啟 [內容]VPN 設定檔。 中的值**一般** 索引標籤應該是可公開解析的 dns。

3. 您可以從外部網路存取 VPN 伺服器嗎？ 請考慮開啟給外部介面的網際網路控制訊息通訊協定 (ICMP)，並 ping 遠端用戶端的名稱。 Ping 成功之後，您可以移除 ICMP 允許規則。

4. 您已正確設定 VPN 伺服器上是否有內部和外部 Nic？ 它們位於不同子網路嗎？ 不是連接至正確的介面，在您的防火牆外部的 NIC 嗎？

5. 是 UDP 500 和 4500 連接埠的 VPN 伺服器的外部介面從用戶端開啟？ 請檢查用戶端防火牆、 伺服器防火牆，以及任何硬體防火牆。 IPSEC 使用 UDP 連接埠 500，因此請確定您沒有 IPEC 停用或封鎖的任何位置。

6. 憑證驗證失敗？ 確認 NPS 伺服器可以 IKE 要求提供服務的伺服器驗證憑證。 請確定您有正確的 VPN 伺服器 IP 指定做為 NPS 用戶端。 請確定您的驗證與 PEAP 搭配，而且受保護的 EAP 內容應該只允許使用憑證進行驗證。 您可以檢查 NPS 事件日誌中的驗證失敗。 如需詳細資訊，請參閱[安裝和設定 NPS 伺服器](vpn-deploy-nps.md)

7. 您連接，但沒有網際網路] / [本機網路存取權嗎？ 請檢查組態問題您 DHCP/VPN 伺服器 IP 集區。

8. 您會連接並具有正確內部 IP 但沒有本機資源的存取權嗎？  請確認用戶端知道如何存取這些資源。 您可以使用 VPN 伺服器，將要求路由傳送。

## <a name="azure-ad-conditional-access-connection-issues"></a>Azure AD 條件式存取連線問題

### <a name="oops---you-cant-get-to-this-yet"></a>抱歉-您無法檢視這尚未

- **錯誤描述。** 條件式存取原則時不滿意，封鎖 VPN 連線，但連線使用者選取後**X**關閉訊息。  選取**確定**會導致另一個結尾為另一個 「 糟糕 」 訊息的驗證嘗試。 這些事件會記錄在用戶端的 AAD 操作事件記錄檔。

- **可能的原因**

  - 使用者會有未由 Azure AD 所發出的有效的用戶端驗證憑證在其個人憑證存放區。

  - VPN 設定檔\<TLSExtensions\>一節已遺漏或不包含 **\<EKUName\>AAD 條件式存取\</EKUName\> \<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>\<EKUName > AAD 條件式存取 < / EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>** 項目。 \<EKUName > 和\<EKUOID > 項目告知的 VPN 用戶端將憑證傳遞到 VPN 伺服器時，從使用者的憑證存放區擷取憑證。 如果沒有這麼做，VPN 用戶端會使用任何有效的用戶端驗證憑證位於使用者的憑證存放區，並驗證成功。 

  - RADIUS 伺服器 (NPS) 尚未設定為只接受包含的用戶端憑證**AAD 條件式存取**OID。

- **可能的解決方案。** 若要逸出此迴圈中，執行下列作業：

  1. 在 Windows PowerShell 中執行**Get-wmiobject** cmdlet 來傾印 VPN 設定檔設定。 
  2. 確認 **\<TLSExtensions >** ，  **\<EKUName >** ，以及 **\<EKUOID >** 區段存在，並顯示正確名稱和 OID。
      
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
     >如果從簽發者憑證**CN = Microsoft VPN 根 CA 層代 1**出現在使用者的個人存放區，但使用者獲得存取選取**X**關閉糟糕訊息，收集 CAPI2 事件記錄檔，以確認用來驗證憑證已從 Microsoft VPN 根 CA 未發出的有效用戶端驗證憑證。

  4. 如果使用者的個人存放區中，有有效的用戶端驗證憑證，連線會失敗 （正常） 使用者選取後**X**如果 **\<TLSExtensions >** ， **\<EKUName >** ，並 **\<EKUOID >** 區段存在，而且包含正確的資訊。
   
     出現錯誤訊息指出 「 憑證不找，可以搭配可延伸驗證通訊協定 」。

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>無法從 [VPN 連線] 刀鋒視窗中刪除憑證

- **錯誤描述。** 無法刪除 VPN 連線 刀鋒視窗上的憑證。

- **可能的原因。** 憑證設定為**主要**。

- **可能的解決方案。**

    1. 在 [VPN 連線] 刀鋒視窗中選取的憑證。
    2. 底下**主要**，選取**No**，然後選取**儲存**。
    3. 在 [VPN 連線] 刀鋒視窗中再次選取憑證。
    4. 選取 **刪除**。
