---
title: 具有 AD FS 的身分識別委派案例
description: 此案例描述需要存取後端資源的應用程式，需要身分識別委派鏈來執行存取控制檢查。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2c461e1051e59fcdb533c00b45157545ffb15df1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867452"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>具有 AD FS 的身分識別委派案例


[從 .NET Framework 4.5 開始，Windows Identity Foundation （WIF）已完全整合到 .NET Framework 中。 本主題 WIF 3.5 所解決的 WIF 版本已被取代，只有在針對 .NET Framework 3.5 SP1 或 .NET Framework 4 進行開發時才應該使用。 如需 .NET Framework 4.5 （也稱為 WIF 4.5）中 WIF 的詳細資訊，請參閱 .NET Framework 4.5 開發指南中的 Windows Identity Foundation 檔。 

此案例描述需要存取後端資源的應用程式，需要身分識別委派鏈來執行存取控制檢查。 簡單的身分識別委派鏈通常包含初始呼叫端的資訊，以及立即呼叫端的身分識別。

現在在 Windows 平臺上使用 Kerberos 委派模型，後端資源只能存取立即呼叫者的身分識別，而不是初始呼叫者的身分。 此模型通常稱為受信任的子系統模型。 WIF 會使用 [動作專案] 屬性來維護初始呼叫端的身分識別，以及委派鏈中的立即呼叫端。

下圖說明典型的身分識別委派案例，其中 Fabrikam 員工會存取 Contoso.com 應用程式中公開的資源。

![身分識別](media/ad-fs-identity-delegation/id1.png)

參與此案例的虛構使用者包括：

- Frank想要存取 Contoso 資源的 Fabrikam 員工。
- Daniel在應用程式中執行必要變更的 Contoso 應用程式開發人員。
- 原址Contoso IT 系統管理員。

此案例中包含的元件包括：

- web1Web 應用程式，其中包含需要初始呼叫者委派身分識別的後端資源連結。 此應用程式是以 ASP.NET 建立。
- 存取 SQL Server 的 Web 服務，這需要初始呼叫者的委派身分識別，以及立即呼叫端的識別碼。 此服務是以 WCF 建立。
- sts1:屬於宣告提供者角色的 STS，會發出應用程式所預期的宣告（web1）。 它已建立與 Fabrikam.com 的信任，以及與應用程式搭配使用。
- sts2:以 Fabrikam.com 身分識別提供者角色的 STS，並提供 Fabrikam 員工用來進行驗證的結束點。 它已建立與 Contoso.com 的信任，讓 Fabrikam 員工可以存取 Contoso.com 上的資源。

>[!NOTE] 
>在此案例中經常使用的「ActAs token」一詞是指 STS 所發出的權杖，並包含使用者的身分識別。 執行者屬性包含 STS 的身分識別。

如上圖所示，此案例中的流程為：


1. Contoso 應用程式已設定為取得 ActAs token，其中同時包含 Fabrikam 員工的身分識別和動作專案屬性中的直接呼叫者身分識別。 Daniel 已對應用程式執行這些變更。
2. Contoso 應用程式已設定為將 ActAs token 傳遞至後端服務。 Daniel 已對應用程式執行這些變更。
3. Contoso Web 服務已設定為藉由呼叫 sts1 來驗證 ActAs token。 Adam 已啟用 sts1 來處理委派要求。
4. Fabrikam 使用者 Frank 會存取 Contoso 應用程式，並獲得對後端資源的存取權。

## <a name="set-up-the-identity-provider-ip"></a>設定身分識別提供者（IP）

有三個選項可供 Fabrikam.com 系統管理員使用，Frank：


1. 購買並安裝 STS 產品，例如 Active Directory® Federation Services （AD FS）。
2. 訂閱雲端 STS 產品，例如 LiveID STS。
3. 使用 WIF 建立自訂的 STS。

在此範例案例中，我們假設 Frank 選取 option1，並將 AD FS 安裝為 IP STS。 他也會設定名為 \windowsauth 的端點來驗證使用者。 藉由參考 AD FS 產品檔並與 Adam 交談，Contoso IT 系統管理員 Frank 會建立與 Contoso.com 網域的信任。

## <a name="set-up-the-claims-provider"></a>設定宣告提供者

Contoso.com 系統管理員（Adam）可用的選項與先前針對身分識別提供者所述的選項相同。 在此範例案例中，我們假設 Adam 選取選項1，並將 AD FS 2.0 安裝為 RP-STS。

## <a name="set-up-trust-with-the-ip-and-application"></a>設定與 IP 和應用程式的信任

藉由參考 AD FS 檔，Adam 會在 Fabrikam.com 與應用程式之間建立信任。

## <a name="set-up-delegation"></a>設定委派

AD FS 提供委派處理。 藉由參考 AD FS 檔，Adam 可讓您處理 ActAs 權杖。

## <a name="application-specific-changes"></a>應用程式特定的變更

必須進行下列變更，才可將對身分識別委派的支援新增至現有的應用程式。 Daniel 會使用 WIF 來進行這些變更。


- 快取 web1 從 sts1 接收的啟動程式權杖。
- 使用 CreateChannelActingAs 搭配已發行的權杖，以建立對後端 Web 服務的通道。
- 呼叫後端服務的方法。

## <a name="cache-the-bootstrap-token"></a>快取啟動程式權杖

啟動程式權杖是 STS 所發行的初始權杖，而應用程式會從它解壓縮宣告。 在此範例案例中，sts1 會為使用者 Frank 發行此權杖，而應用程式會將其快取。 下列程式碼範例示範如何在 ASP.NET 應用程式中取出啟動程式權杖：

```
// Get the Bootstrap Token
SecurityToken bootstrapToken = null;

IClaimsPrincipal claimsPrincipal = Thread.CurrentPrincipal as IClaimsPrincipal;
if ( claimsPrincipal != null )
{
    IClaimsIdentity claimsIdentity = (IClaimsIdentity)claimsPrincipal.Identity;
    bootstrapToken = claimsIdentity.BootstrapToken;
}
```
WIF 提供的方法是[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)，它會建立指定類型的通道，以使用指定的安全性權杖，將權杖發行要求擴大為 ActAs 元素。 您可以將啟動載入器 token 傳遞至此方法，然後在傳回的通道上呼叫所需的服務方法。 在此範例案例中，Frank 的身分識別[將動作專案屬性設定](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx)為 web1's identity。

下列程式碼片段顯示如何使用[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)呼叫 Web 服務，然後在傳回的通道上呼叫其中一個服務的方法 ComputeResponse：

```
// Get the channel factory to the backend service from the application state
ChannelFactory<IService2Channel> factory = (ChannelFactory<IService2Channel>)Application[Global.CachedChannelFactory];

// Create and setup channel to talk to the backend service
IService2Channel channel;
lock (factory)
{
// Setup the ActAs to point to the caller's token so that we perform a 
// delegated call to the backend service
// on behalf of the original caller.
    channel = factory.CreateChannelActingAs<IService2Channel>(callerToken);
}

string retval = null;

// Call the backend service and handle the possible exceptions
try
{
    retval = channel.ComputeResponse(value);
    channel.Close();
} catch (Exception exception)
{
    StringBuilder sb = new StringBuilder();
    sb.AppendLine("An unexpected exception occurred.");
    sb.AppendLine(exception.StackTrace);
    channel.Abort();
    retval = sb.ToString();
}

```
## <a name="web-service-specific-changes"></a>Web 服務特定的變更

由於 Web 服務是以 WCF 建立並啟用 WIF，因此一旦系結設定了具有適當簽發者位址的 IssuedSecurityTokenParameters，ActAs 的驗證就會由 WIF 自動處理。 

Web 服務會公開應用程式所需的特定方法。 服務上不需要進行任何特定的程式碼變更。 下列程式碼範例示範如何使用 IssuedSecurityTokenParameters 設定 Web 服務：

```
// Configure the issued token parameters with the correct settings
IssuedSecurityTokenParameters itp = new IssuedSecurityTokenParameters( "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" );
itp.IssuerMetadataAddress = new EndpointAddress( "http://localhost:6000/STS/mex" );
itp.IssuerAddress = new EndpointAddress( "http://localhost:6000/STS" );

// Create the security binding element
SecurityBindingElement sbe = SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement( itp );
sbe.MessageSecurityVersion = MessageSecurityVersion.WSSecurity11WSTrust13WSSecureConversation13WSSecurityPolicy12BasicSecurityProfile10;

// Create the HTTP transport binding element
HttpTransportBindingElement httpBE = new HttpTransportBindingElement();

// Create the custom binding using the prepared binding elements
CustomBinding binding = new CustomBinding( sbe, httpBE );

using ( ServiceHost host = new ServiceHost( typeof( Service2 ), new Uri( "http://localhost:6002/Service2" ) ) )
{
    host.AddServiceEndpoint( typeof( IService2 ), binding, "" );
    host.Credentials.ServiceCertificate.SetCertificate( "CN=localhost", StoreLocation.LocalMachine, StoreName.My );

// Enable metadata generation via HTTP GET
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
    smb.HttpGetEnabled = true;
    host.Description.Behaviors.Add( smb );
    host.AddServiceEndpoint( typeof( IMetadataExchange ), MetadataExchangeBindings.CreateMexHttpBinding(), "mex" );

// Configure the service host to use WIF
    ServiceConfiguration configuration = new ServiceConfiguration();
    configuration.IssuerNameRegistry = new TrustedIssuerNameRegistry();

    FederatedServiceCredentials.ConfigureServiceHost( host, configuration );

    host.Open();

    Console.WriteLine( "Service2 started, press ENTER to stop ..." );
    Console.ReadLine();

    host.Close();
}
```

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
