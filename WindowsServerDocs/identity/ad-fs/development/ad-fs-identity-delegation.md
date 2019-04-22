---
title: 與 AD FS 的身分識別委派案例
description: 此案例中描述的應用程式需要存取需要執行存取控制檢查身分識別委派鏈結的後端資源。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819849"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>與 AD FS 的身分識別委派案例


[從.NET Framework 4.5 開始，Windows Identity Foundation (WIF) 已完全整合至.NET Framework。 本主題中，WIF 3.5 所定址的 WIF 的版本已被取代，應該只在針對.NET Framework 3.5 SP1 或.NET Framework 4 進行開發時才使用。 如需在.NET Framework 4.5 中，也稱為 WIF 4.5 中，WIF 的詳細資訊請參閱.NET Framework 4.5 開發指南中的 Windows Identity Foundation 文件。] 

此案例中描述的應用程式需要存取需要執行存取控制檢查身分識別委派鏈結的後端資源。 在初始呼叫者並立即呼叫端的身分識別資訊通常包含簡單身分識別委派鏈結。

立即在 Windows 平台上的 Kerberos 委派模型，與後端資源會有存取權才能立即呼叫端的身分識別和不的初始呼叫者。 此模型通常稱為 「 受信任的子系統模型。 WIF 會維護初始呼叫者，以及使用動作項目屬性的委派鏈結中的立即呼叫端的身分識別。

下圖說明典型的身分識別委派的情況下，在其中 Fabrikam 員工會存取 Contoso.com 應用程式中公開的資源。

![身分識別](media/ad-fs-identity-delegation/id1.png)

在此案例中參與的虛構使用者是：

- Frank:Fabrikam 員工想要存取 Contoso 資源。
- Daniel:Contoso 應用程式的開發人員在應用程式中實作必要的變更。
- Adam:Contoso IT 系統管理員。

在此案例中所涉及的元件如下：

- web1:Web 應用程式的連結需要委派的身分識別的初始呼叫者的後端資源。 使用 ASP.NET 建置此應用程式。
- 存取 SQL Server，而這需要委派的身分識別的初始呼叫者，以及立即呼叫端的 Web 服務。 此服務會使用 WCF 建置。
- sts1:STS 角色的宣告提供者，並發出宣告式預期的應用程式 (web1)。 它已建立 Fabrikam.com 與以及與應用程式的信任。
- sts2:位於 Fabrikam.com 的身分識別提供者的角色，並提供 Fabrikam 員工用來驗證端點 STS。 若要建立 contoso.com 的信任，它的讓 Fabrikam 員工可以存取 Contoso.com 上的資源。

>[!NOTE] 
>詞彙"actas 語彙基元 」，這通常用在此案例中，參考的某個 STS 所發出，且包含使用者的身分識別的權杖。 動作項目屬性會包含 STS 的身分識別。

上圖所示，在此案例中的流程就會是：


1. Contoso 應用程式會設定為取得 ActAs 權杖，其中包含 Fabrikam 員工的身分識別和動作項目屬性中的立即呼叫者身分識別。 Daniel 已實作這些變更應用程式。
2. Contoso 應用程式會設定為將 actas 語彙基元傳遞至後端服務。 Daniel 已實作這些變更應用程式。
3. Contoso Web 服務被設定為藉由呼叫 sts1 驗證 actas 語彙基元。 Adam 已經啟用 sts1 處理委派的要求。
4. Fabrikam 使用者 Frank 存取 Contoso 應用程式，並獲得存取權的後端資源。

## <a name="set-up-the-identity-provider-ip"></a>設定身分識別提供者 (IP)

有適用於 Fabrikam.com 系統管理員，Frank 的三個選項：


1. 購買並安裝 STS 產品，例如 Active Directory® Federation Services (AD FS)。
2. 訂閱的雲端 STS 產品，例如 LiveID STS。
3. 建置自訂 STS 使用 WIF。

此範例案例中，我們假設 Frank 選取選項 1，並將 AD FS 安裝為 IP-STS。 他也會設定為端點，名為 \windowsauth，來驗證使用者。 指的 AD FS 產品文件，並與 Adam，Contoso 的 IT 系統管理員中，Frank 會建立與 Contoso.com 網域的信任。

## <a name="set-up-the-claims-provider"></a>設定宣告提供者

選項適用於 Contoso.com 的系統管理員，Adam，會如同先前所述，身分識別提供者。 此範例案例中，我們假設 Adam 選取選項 1，並將 AD FS 2.0 安裝為 RP STS。

## <a name="set-up-trust-with-the-ip-and-application"></a>設定信任 IP 與應用程式

藉由參考的 AD FS 的文件，Adam 會建立 Fabrikam.com 與應用程式之間的信任。

## <a name="set-up-delegation"></a>設定委派

AD FS 提供委派處理。 藉由參考的 AD FS 的文件，Adam 會讓 ActAs 語彙基元的處理。

## <a name="application-specific-changes"></a>應用程式特定的變更

將身分識別委派支援新增至現有的應用程式，就必須進行下列的變更。 Daniel 使用 WIF 進行這些變更。


- 快取的啟動程序的權杖從 sts1 收到該 web1。
- 使用 CreateChannelActingAs 和發行的權杖，以建立後端 Web 服務的通道。
- 呼叫後端服務的方法。

## <a name="cache-the-bootstrap-token"></a>快取的啟動程序的權杖

啟動程序權杖的 STS 所發行的初始權杖，且應用程式會從其擷取宣告。 在此範例案例中，此權杖公布的 sts1 Frank 的使用者和應用程式快取它。 下列程式碼範例會示範如何擷取啟動程序權杖中的 ASP.NET 應用程式：

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
WIF 提供一種方法， [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)，建立指定型別的擴大為 ActAs 項目指定的安全性權杖的權杖發佈要求的通道。 您可以將啟動程序權杖傳遞給這個方法，並接著呼叫傳回的通道上的 必要的服務方法。 在此範例案例中，Frank 的身分識別具有[動作項目](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx)屬性設定為 web1 的身分識別。

下列程式碼片段示範如何呼叫 Web 服務[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) ，然後呼叫其中一種服務的方法，ComputeResponse，傳回的通道上：

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
## <a name="web-service-specific-changes"></a>Web 服務特有的變更

由於 Web 服務是以 WCF 建置，而且啟用 WIF，一旦繫結設定為使用 IssuedSecurityTokenParameters 正確的簽發者位址，WIF 會自動處理 ActAs 的驗證。 

Web 服務會公開應用程式所需的特定方法。 沒有服務上所需的特定程式碼變更。 下列程式碼範例顯示 IssuedSecurityTokenParameters 的 Web 服務的組態：

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
