---
title: "AD FS 使用的身分委派案例"
description: "本案例描述需要存取後端資源需要身分委派鏈結進行存取控制檢查應用程式。"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>AD FS 使用的身分委派案例


[開始使用.NET Framework 4.5、Windows 身分基礎 (WIF) 已被完整地整合.NET Framework。 WIF WIF 3.5，此主題中所述的版本會取代，應該時開發.NET Framework 3.5 SP1 或.NET Framework 4 僅限使用。 如需有關的.NET Framework 4.5，也就是 WIF 4.5 WIF 查看 Windows 的身分基本知識的文件中的.NET Framework 4.5 開發。] 

本案例描述需要存取後端資源需要身分委派鏈結進行存取控制檢查應用程式。 簡單的身分委派鏈結通常組成初始播報來電者和立即播報來電者的身分的資訊。

Kerberos 委派模型今天 Windows 平台上，使用端資源存取，才能立即播報來電者的身分，而不的初始播報來電者。 此模型通常稱為信任的子系統模型。 WIF 維護初始播報來電者，以及使用演員屬性委派鏈結中的立即播報來電者的身分。

下圖顯示一般身分委派案例中 Fabrikam 員工存取公開 Contoso.com 應用程式中的資源。

![身分](media/ad-fs-identity-delegation/id1.png)

本案例中為參與虛構使用者︰

- Frank: Fabrikam 員工想要存取 Contoso 資源。
- 奧地：Contoso 應用程式開發人員的應用程式中執行所需的變更。
- Adam: Contoso IT 系統管理員。

本案例中相關元件︰

- web1: Web 應用程式連結後端資源需要委派的身分初始播報來電者。 建置 ASP.NET 使用這個應用程式。
- 存取 SQL Server、需要委派的身分初始播報來電者，以及立即播報來電者的 Web 服務。 這項服務以 WCF 建置。
- sts1: STS 中的角色宣告提供者，並會發出宣告應用程式 (web1) 預期的。 它已建立信任的 Fabrikam.com 和應用程式。
- sts2: STS，Fabrikam.com 身分提供者的角色是提供 Fabrikam 員工使用驗證結束點。 它已建立與 Contoso.com 信任使 Fabrikam 員工已獲授權存取 Contoso.com 資源。

>[!NOTE] 
>「ActAs 權杖」，這通常中使用此案例，指到預付碼由 STS 發出，其中包含使用者的身分。 演員屬性包含 STS 的身分。

在上圖所示，流程在本案例中的為：


1. Contoso 應用程式設定為取得 ActAs 權杖包含 Fabrikam 員工的身分和演員屬性身分立即播報來電者。 奧地已實作這些應用程式的變更。
2. Contoso 應用程式設定為傳遞至後端服務 ActAs 預付碼。 奧地已實作這些應用程式的變更。
3. 設定 Contoso Web 服務為呼叫 sts1 驗證 ActAs 預付碼。 Adam 已支援 sts1 處理委派要求。
4. Fabrikam 使用者 Frank 存取 Contoso 應用程式，都可以存取後端資源。

## <a name="set-up-the-identity-provider-ip"></a>設定的身分提供者 (IP)

適用於 Fabrikam.com 系統管理員，Frank 的三個選項：


1. 購買並安裝 STS product，例如 Active Directory（) 聯盟 Services (AD FS)。
2. 希望例如 LiveID STS 雲端 STS product。
3. 組建使用 WIF 自訂 STS。

針對此範例，我們假設 Frank 選取選項 1 並安裝 IP STS AD FS。 他也設定名 \windowsauth，以便驗證使用者結束點。 透過推薦 AD FS product 文件和到 Adam，以 Contoso IT 系統管理員，Frank 建立與 Contoso.com 網域信任。

## <a name="set-up-the-claims-provider"></a>設定宣告提供者

選項適用於 Contoso.com 系統管理員，Adam，都相同如之前所述，身分提供者。 針對此範例，我們假設 Adam 選取選項 1 並安裝資源點數 STS AD FS 2.0。

## <a name="set-up-trust-with-the-ip-and-application"></a>信任的 IP 和應用程式設定

透過推薦 AD FS 文件，Adam 建立 Fabrikam.com 和應用程式之間的信任。

## <a name="set-up-delegation"></a>設定委派

AD FS 提供委派處理。 透過推薦 AD FS 文件，Adam 可讓 ActAs 權杖處理。

## <a name="application-specific-changes"></a>應用程式特定的變更

下列需要進行變更的身分委派支援加入現有的應用程式。 奧地使用 WIF 完成這些改變。


- 快取開機權杖收到 sts1 該 web1。
- 發行的權杖 CreateChannelActingAs 使用建立後端服務網路的通道。
- 呼叫後端服務的方法。

## <a name="cache-the-bootstrap-token"></a>快取開機權杖

開機預付碼是由 STS，發行的初始權杖和應用程式中擷取時宣告。 在此範例案例中，此預付碼由 sts1 使用者 Frank 發出和應用程式將其快取。 下列程式碼範例顯示如何擷取開機權杖 ASP.NET 應用程式中：

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
WIF 提供一種方法，[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)，建立權杖發行要求，以指定的安全性權杖為 ActAs 項目，指定類型的通道。 您可以將開機權杖傳遞此方法，接著呼叫必要服務方法傳回頻道。 在此範例案例中，Frank 的身分有[演員](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx)屬性設 web1 的身分。

下列程式碼片段示範如何使用 Web 服務呼叫[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)，然後在傳回通道呼叫其中一個 ComputeResponse，服務的方法：

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

Web 服務建置 wcf 和支援 WIF，繫結的 IssuedSecurityTokenParameters 設定適當的發行者地址後，由於 WIF 會自動處理 ActAs 的驗證。 

Web 服務公開應用程式所需的特定方法。 有需要服務任何特定的程式碼變更。 下列程式碼範例顯示 IssuedSecurityTokenParameters Web 服務的設定：

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
