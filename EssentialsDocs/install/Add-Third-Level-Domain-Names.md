---
title: "新增第三個層級網域名稱"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64bf24e45155fdd981e2061b3de7ebce1c53b36c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-third-level-domain-names"></a>新增第三個層級網域名稱

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以加入的功能讓使用者要求設定網域名稱精靈中的第三個層級網域名稱。 建立及安裝的程式碼組件所使用的網域管理員作業系統來執行此動作。  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>建立第三個層級網域名稱的提供者  
 您可以使用第三個層級網域名稱來建立和安裝的程式碼組件所提供的網域名稱精靈。 若要這樣做，請完成下列工作：  
  
-   [新增 IDomainSignupProvider 介面實作組件](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [新增 IDomainMaintenanceProvider 介面實作組件](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [登入驗證碼簽章組的件](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [參考電腦上安裝組件](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Windows Server 網域名稱管理服務重新開機](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a>新增 IDomainSignupProvider 介面實作組件  
 IDomainSignupProvider 介面用來在精靈加入網域方案。  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>若要新增 IDomainSignupProvider 程式碼組件  
  
1.  Visual Studio 2008 開放系統管理員的身分，以滑鼠右鍵按一下計畫的**[開始]**功能表，然後選取**以系統管理員身分執行**。  
  
2.  按一下**檔案**，按一下 [**新**，然後按一下 [**專案**。  
  
3.  在**新專案**對話方塊中，按**Visual C#**，按一下 [**程式庫**，輸入方案的名稱，然後按一下**[確定]**。  
  
4.  重新命名 Class1.cs 檔案。 例如，MyDomainNameProvider.cs  
  
5.  加入參考 Wssg.Web.DomainManagerObjectModel.dll、CertManaged.dll、WssgCertMgmt.dll，以及 WssgCommon.dll 檔案。  
  
6.  新增下列使用聲明。  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  變更命名空間與課程標頭，以符合下列範例。  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  加入課程，會顯示方案定義精靈中的初始化方法與所需的變數。  
  
    > [!NOTE]
    >  起始方法定義識別字必須唯一網域提供者。 若要這樣做一般的方式是 id 定義 GUID。 如需有關建立 GUID 的詳細資訊，請查看[建立 Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。  
  
     下列範例顯示初始化方法。  
  
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
  
9. 新增 GetOfferings 方法，在上一個步驟傳回初始化方案的清單。 下列範例顯示 GetOfferings 方法。  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. 新增從清單中傳回所提供的 FindOfferingForDomain 方法。 下列範例顯示 FindOfferingForDomain 方法。  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. 新增定義的憑證存取方案所需的 SetCredentials 方法。 下列範例顯示 SetCredentials 方法。  
  
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
  
12. 新增 ValidateCredentials 方法，讓進行驗證 SetCredentials 所定義的認證。 下列範例顯示 ValidateCredentials 方法。  
  
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
  
13. 新增的 GetAvailableDomainRoots 方法，傳回根網域名稱在要求中指定的提供支援的清單。 不可空白這份根網域名稱。 下列範例顯示 GetAvailableDomainRoots 方法。  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. 新增的 GetUserDomainNames 方法，傳回目前使用者已經擁有、和目的地的相對目前提供的網域名稱的清單。 此清單可能是空的。 下列範例顯示 GetUserDomainNames 方法。  
  
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
  
15. 新增傳回最大的網域所指定的提供可以讓的 GetUserDomainQuota 方法。 如果不適用的最大值，此方法應該返回 0。 下列範例顯示 GetUserDomainQuota 方法。  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. 新增的 CheckDomainAvailability 方法，檢查有可用的要求的網域名稱，並可以退還一份建議。 下列範例顯示 CheckDomainAvailability 方法。  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. 新增的 CommitDomain 方法，認可要求的網域名稱。 完成此方法表示的網域名稱相關聯的使用者帳號，維護提供者會立即稱為行動電話後取回憑證狀態 FullyOperational，並提供者，並提供變成作用中。 下列範例顯示 CommitDomain 方法。  
  
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
  
18. 新增的 ReleaseDomain 方法，使用者想要的網域名稱日發行版本的提供者的通知。 下列範例顯示 ReleaseDomain 方法。  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. 新增 GetProviderLandingUrl 方法，它會傳回登陸頁面註冊工作流程網域中的 URL。 下列範例顯示 GetProviderLandingUrl 方法。  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. 新增傳回 IDomainMaintenanceProvider 網域維護工作用來執行個體的 GetDomainMaintenanceProvider 方法。 這個方法稱為 CommitDomain 方法成功之後，並開始網域管理員。 下列範例顯示 GetDomainMaintenanceProvider 方法。  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. 請儲存專案，不要關閉因為將它新增的下一個程序。 您將無法再建置專案，直到您完成程序。  
  
###  <a name="BKMK_DomainMaintenance"></a>新增 IDomainMaintenanceProvider 介面實作組件  
 IDomainMaintenanceProvider 用來建立後維護網域。  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>若要新增 IDomainMaintenanceProvider 程式碼組件  
  
1.  新增課程標頭網域維護提供者。 請確定您定義提供者的名稱符合 GetDomainMaintenanceProvider 方法預先定義的名稱。  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  新增 Activate 方法，這會將設定為使用中的提供者。 下列範例顯示 Activate 方法。  
  
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
  
3.  新增停用方法，用來停用所有動作。 下列範例顯示停用的方法。  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  新增的 SetCredentials 方法，更新的使用者的認證。 例如，此方法可能稱為「更新是有效的憑證。 下列範例顯示 SetCredentials 方法。  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  新增的 ValidateCredentials 方法，驗證指定的憑證。 下列範例顯示 ValidateCredentials 方法。  
  
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
  
6.  新增的 GetPublicAddress 方法，傳回外部的伺服器的 IP 位址。 下列範例顯示 GetPublicAddress 方法。  
  
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
  
7.  新增的 SubmitCertificateRequest 方法，提交憑證要求，目前已設定的網域名稱。  
  
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
  
8.  新增的 GetCertificateResponse 方法，如果網域狀態 FullyOperational 傳回憑證回應。 這個方法稱為這兩個新的憑證要求和憑證續約要求。 下列範例顯示 GetCertificateResponse 方法。  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. 新增的 SubmitRenewCertificateRequest 方法，處理憑證的更新。 下列範例顯示 SubmitRenewCertificateRequest 方法。  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. 新增的 UpdateDNSRecords 方法，更新 DNS 記錄儲存提供者。 下列範例顯示 UpdateDNS 方法。  
  
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
  
11. 新增的 TestConnection 方法，嘗試在連接後端服務。 如果這種方法需要驗證，適當應該會例外。 下列範例顯示 TestConnection 方法。  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. 新增 GetDomainState 方法，它會傳回達到目前狀態的網域。 下列範例顯示 GetDomainState 方法。  
  
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
  
13. 新增的 GetCertificateState 方法，傳回達到目前狀態的憑證。 下列範例顯示 GetCertificateState 方法。  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. 儲存或建置方案。  
  
###  <a name="BKMK_SignAssembly"></a>登入驗證碼簽章組的件  
 您必須在作業系統中使用的驗證碼登入一組件。 如需有關登入一組件，請查看[簽章和檢查的驗證碼的程式碼](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
###  <a name="BKMK_InstallAssembly"></a>參考電腦上安裝組件  
 將組件參照電腦上的資料夾中。 記下的路徑資料夾會輸入中的下一個步驟的登錄因為。  
  
### <a name="add-a-key-to-the-registry"></a>新增到登錄按鍵  
 您加入登錄項目，來定義的特性和組的位置。  
  
##### <a name="to-add-a-key-to-the-registry"></a>若要新增按鍵登錄  
  
1.  參考在電腦上，按一下 [ **[開始]**，輸入**regedit**，然後按**輸入**。  
  
2.  在左窗格中，展開**跳**，展開**軟體**，展開 [ **Microsoft**，展開 [ **Windows Server**，展開 [**網域經理**，然後展開**提供者**。  
  
3.  以滑鼠右鍵按一下**提供者**，指向 [**新**，然後按一下 [**鍵**。  
  
4.  輸入金鑰的名稱為識別碼提供者。 識別碼是您在執行「步驟 8 的提供者所定義的 GUID[新增 IDomainSignupProvider 介面實作組件以](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)。  
  
5.  以滑鼠右鍵按一下您剛鍵建立，，然後按一下**字串值**。  
  
6.  輸入**組件**字串，然後按下的名稱為**輸入**。  
  
7.  以滑鼠右鍵按一下新的**組件**字串右窗格中，然後按一下 [**修改]**。  
  
8.  輸入的完整路徑組件檔案，您先前建立，然後按一下**[確定]**。  
  
9. 再試一次，以滑鼠右鍵按一下鍵，然後按一下**字串值**。  
  
10. 輸入**啟用**字串，然後按下的名稱為**輸入**。  
  
11. 以滑鼠右鍵按一下新的**啟用**字串右窗格中，然後按一下 [**修改]**。  
  
12. 輸入**True**，然後按一下 [ **[確定]**。  
  
13. 再試一次，以滑鼠右鍵按一下鍵，然後按一下**字串值**。  
  
14. 輸入**輸入**字串，然後按下的名稱為**輸入**。  
  
15. 以滑鼠右鍵按一下新的**輸入**字串右窗格中，然後按一下 [**修改]**。  
  
16. 輸入定義組件，您提供者課程完整名稱，然後按一下**[確定]**。  
  
###  <a name="BKMK_RestartService"></a>Windows Server 網域名稱管理服務重新開機  
 您必須將 Windows 伺服器網域管理服務提供者作業系統可用來重新開機。  
  
##### <a name="restart-the-service"></a>重新開機服務  
  
1.  按一下**[開始]**，輸入**mmc**，然後按**輸入**。  
  
2.  如果 [服務] 嵌入式管理單元未列出的主機，將它新增完成下列步驟：  
  
    1.  按一下**檔案**，然後按**新增/移除嵌入式管理單元**。  
  
    2.  在**可用嵌入式管理單元**清單中，按一下 [**服務**，然後按一下 [**新增**。  
  
    3.  在**服務**對話方塊中，請確定**本機電腦**已選取，然後按一下 [**完成**。  
  
    4.  按一下**[確定]**以關閉 [**新增/移除嵌入式管理單元**對話方塊。  
  
3.  按兩下**服務**、向下捲動並選取 [ **Windows Server 網域管理**，然後按一下 [**重新開機服務**。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)