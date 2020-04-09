---
title: 新增第三層網域名稱
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 659844ce4da6a7ec6311b36b4516875ed468733e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817551"
---
# <a name="add-third-level-domain-names"></a>新增第三層網域名稱

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以在設定網域名稱精靈中新增讓使用者要求第三層網域名稱的功能。 透過建立及安裝作業系統中設定管理員所使用的程式碼組件即可達成。  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>建立第三層網域名稱的提供者  
 您可以建立並安裝會提供網域名稱給精靈的程式碼組件，以使第三層網域名稱變得可用。 若要執行此動作，請完成下列工作：  
  
-   [將 IDomainSignupProvider 介面的執行加入至元件](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [將 IDomainMaintenanceProvider 介面的執行加入至元件](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [使用 Authenticode 簽章簽署元件](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [將元件安裝在參照電腦上](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [重新開機 Windows Server 功能變數名稱管理服務](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="add-an-implementation-of-the-idomainsignupprovider-interface-to-the-assembly"></a><a name="BKMK_DomainSignup"></a>將 IDomainSignupProvider 介面的執行加入至元件  
 IDomainSignupProvider 介面可用來新增網域選項至精靈。  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>若要將 IDomainSignupProvider 程式碼新增至組件  
  
1.  用滑鼠右鍵按一下 [開始] 功能表中的程式，然後選取 [以系統管理員身分執行]，以系統管理員身分開啟 Visual Studio 2008。  
  
2.  依序按一下 **[檔案]** 、 **[新增]** ，然後按一下 **[專案]** 。  
  
3.  在 **[新增專案]** 對話方塊中，依序按一下 **[Visual C#]** 和 **[類別庫]** ，輸入方案的名稱，然後按一下 **[確定]** 。  
  
4.  重新命名 Class1.cs 檔案。 例如，MyDomainNameProvider.cs。  
  
5.  新增 Wssg.Web.DomainManagerObjectModel.dll、CertManaged.dll、WssgCertMgmt.dll 和 WssgCommon.dll 檔案的參照。  
  
6.  新增下列 using 陳述式。  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  變更命名空間和類別標頭，以符合下列範例：  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  將 Initialize 方法和必要變數新增至類別，這個類別會定義精靈中出現的選項。  
  
    > [!NOTE]
    >  Initialize 方法會定義網域提供者的識別項 (這個識別項必須是唯一的)。 通常這是透過定義 GUID 作為識別項來達成。 如需有關建立 GUID 的詳細資訊，請參閱 [建立 GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。  
  
     下列程式碼範例顯示 Initialize 方法：  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. 新增 GetOfferings 方法，這個方法會傳回在上一個步驟中初始化的選項清單。 下列程式碼範例顯示 GetOfferings 方法：  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. 新增 FindOfferingForDomain 方法，這個方法會傳回清單中的項目。 下列程式碼範例顯示 FindOfferingForDomain 方法。  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. 新增 SetCredentials 方法，這個方法會定義存取選項所需的認證。 下列程式碼範例顯示 SetCredentials 方法。  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. 新增 ValidateCredentials 方法，這個方法會驗證 SetCredentials 所定義的認證。 下列程式碼範例顯示 ValidateCredentials 方法。  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. 新增 GetAvailableDomainRoots 方法，這個方法會傳回要求中指定的選項所支援之根網域名稱的清單。 此根網域名稱清單不得為空白。 下列程式碼範例顯示 GetAvailableDomainRoots 方法。  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. 新增 GetUserDomainNames 方法，這個方法會傳回目前使用者已經擁有、與目前選項相關之網域名稱的清單。 此清單可以是空白。 下列程式碼範例顯示 GetUserDomainNames 方法。  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. 新增 GetUserDomainQuota 方法，這個方法會傳回指定的選項所允許的網域數目上限。 如果上限不適用，此方法應傳回 0。 下列程式碼範例顯示 GetUserDomainQuota 方法。  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. 新增 CheckDomainAvailability 方法，這個方法會檢查所要求的網域名稱是否可用，並可傳回建議清單。 下列程式碼範例顯示 CheckDomainAvailability 方法。  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. 新增 CommitDomain 方法，這個方法會認可所要求的網域名稱。 這個方法成功完成，即表示網域名稱與使用者帳戶有關聯，如果狀態為 FullyOperational 時即會立即呼叫維護提供者來擷取憑證，而提供者和選項都變成作用中。 下列程式碼範例顯示 CommitDomain 方法。  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. 新增 ReleaseDomain 方法，這個方法會通知提供者，使用者想要釋出網域名稱。 下列程式碼範例顯示 ReleaseDomain 方法。  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. 新增 GetProviderLandingUrl 方法，這個方法會傳回網域註冊工作流程中登陸網頁的 URL。 下列程式碼範例顯示 GetProviderLandingUrl 方法。  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. 新增 GetDomainMaintenanceProvider 方法，這個方法會傳回用於網域維護工作的 IDomainMaintenanceProvider 執行個體。 在 CommitDomain 方法成功後，以及網域管理員啟動時，都會呼叫這個方法。 下列程式碼範例顯示 GetDomainMaintenanceProvider 方法。  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. 儲存但不要關閉專案，因為您會在下一個程序中新增專案內容。 除非您完成下一個程序，否則無法建置專案。  
  
###  <a name="add-an-implementation-of-the-idomainmaintenanceprovider-interface-to-the-assembly"></a><a name="BKMK_DomainMaintenance"></a>將 IDomainMaintenanceProvider 介面的執行加入至元件  
 IDomainMaintenanceProvider 用來在網域建立之後維護網域。  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>若要將 IDomainMaintenanceProvider 程式碼新增至組件  
  
1.  新增網域維護提供者的類別標頭。 確保您為提供者定義的名稱，符合您先前在 GetDomainMaintenanceProvider 方法中定義的名稱。  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  新增 Activate 方法，這個方法會設定作用中提供者。 下列程式碼範例顯示 Activate 方法。  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  新增用來停用所有動作的 Deactivate 方法。 下列程式碼範例顯示 Deactivate 方法。  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  新增 SetCredentials 方法，這個方法會更新使用者認證。 例如，可以呼叫這個方法來更新不再有效的認證。 下列程式碼範例顯示 SetCredentials 方法。  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  新增 ValidateCredentials 方法，這個方法會驗證指定的認證。 下列程式碼範例顯示 ValidateCredentials 方法。  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  新增 GetPublicAddress 方法，這個方法會傳回伺服器的外部 IP 位址。 下列程式碼範例顯示 GetPublicAddress 方法。  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  新增 SubmitCertificateRequest 方法，這個方法會針對目前設定的網域名稱提交憑證要求。  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  新增 GetCertificateResponse 方法，這個方法會在網域狀態為 FullyOperational 時傳回憑證的回應。 當有新憑證要求和憑證更新要求時，都會呼叫這個方法。 下列程式碼範例顯示 GetCertificateResponse 方法。  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. 新增 SubmitRenewCertificateRequest 方法，這個方法會處理憑證的更新。 下列程式碼範例顯示 SubmitRenewCertificateRequest 方法。  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. 新增 UpdateDNSRecords 方法，這個方法會更新提供者所儲存的 DNS 記錄。 下列程式碼範例顯示 UpdateDNS 方法。  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. 新增 TestConnection 方法，這個方法會嘗試建立後端服務連線。 如果這個方法需要驗證，應會擲回適當的例外狀況。 下列程式碼範例顯示 TestConnection 方法。  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. 新增 GetDomainState 方法，這個方法會傳回網域目前的狀態。 下列程式碼範例顯示 GetDomainState 方法。  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. 新增 GetCertificateState 方法，這個方法會傳回憑證目前的狀態。 下列程式碼範例顯示 GetCertificateState 方法。  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. 儲存並建置方案。  
  
###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>使用 Authenticode 簽章簽署元件  
 您必須使用 Authenticode 來簽署組件，才能在作業系統中使用這些組件。 如需簽署組件的相關詳細資訊，請參閱 [使用 Authenticode 簽署和檢查程式碼](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a>將元件安裝在參照電腦上  
 將組件放置在參照電腦上的資料夾中。 記下資料夾位置，因為在下一個步驟您將會在登錄中輸入此位置。  
  
### <a name="add-a-key-to-the-registry"></a>新增登錄機碼  
 您會新增登錄項目以定義組件的特性和位置。  
  
##### <a name="to-add-a-key-to-the-registry"></a>若要新增登錄機碼  
  
1.  在參照電腦上，按一下 **[開始]** ，輸入 **regedit**，然後按 **ENTER**。  
  
2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**、**Windows Server**、**Domain Managers** 和 **Providers**。  
  
3.  以滑鼠右鍵按一下 **Providers**，指向 **[新增]** ，然後按一下 **[機碼]** 。  
  
4.  輸入提供者的識別項作為機碼的名稱。 識別項就是您在步驟 8 [將 IDomainSignupProvider 介面的實作新增至組件](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)中為提供者定義的 GUID。  
  
5.  以滑鼠右鍵按一下您剛才建立的機碼，然後按一下 **[字串值]** 。  
  
6.  輸入 **Assembly** 作為字串的名稱，然後按 **ENTER**。  
  
7.  在右窗格中，以滑鼠右鍵按一下新的 **Assembly**，然後按一下 **[修改]** 。  
  
8.  輸入您先前建立之組件檔案的完整路徑，然後按一下 **[確定]** 。  
  
9. 再以滑鼠右鍵按一下機碼，然後按一下 **[字串值]** 。  
  
10. 輸入 **Enabled** 作為字串的名稱，然後按 **ENTER**。  
  
11. 在右窗格中，以滑鼠右鍵按一下新的 **Enabled**，然後按一下 **[修改]** 。  
  
12. 輸入 **True**，然後按一下 [確定]。  
  
13. 再以滑鼠右鍵按一下機碼，然後按一下 **[字串值]** 。  
  
14. 輸入 **Type** 作為字串名稱，然後按 **ENTER**。  
  
15. 在右窗格中，以滑鼠右鍵按一下新的 **Type**，然後按一下 **[修改]** 。  
  
16. 輸入組件中所定義之提供者的完整類別名稱，然後按一下 **[確定]** 。  
  
###  <a name="restart-the-windows-server-domain-name-management-service"></a><a name="BKMK_RestartService"></a>重新開機 Windows Server 功能變數名稱管理服務  
 您必須重新啟動 Windows Server Domain Management 服務，讓提供者在作業系統中生效。  
  
##### <a name="restart-the-service"></a>重新啟動服務  
  
1.  按一下 [開始]，輸入 **mmc**，然後按 **ENTER**。  
  
2.  如果主控台中沒有列出 [服務] 嵌入式管理單元，請完成下列步驟加以新增：  
  
    1.  按一下 **[檔案]** ，然後按一下 **[新增/移除嵌入式管理單元]** 。  
  
    2.  在 **[可用的嵌入式管理單元]** 清單中，按一下 **[服務]** ，然後按一下 **[新增]** 。  
  
    3.  在 **[服務]** 對話方塊中，確保選取 **[本機電腦]** ，然後按一下 **[完成]** 。  
  
    4.  按一下 **[確定]** 以關閉 **[新增/移除嵌入式管理單元]** 對話方塊。  
  
3.  按兩下 **[服務]** ，向下捲動並選取 **[Windows Server Domain Management]** ，然後按一下 **[重新啟動服務]** 。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)