---
title: 使用 AD FS 2019 風險評估模型建置外掛程式
description: 深入瞭解：使用 AD FS 2019 風險評估模型建立外掛程式
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/05/2020
ms.topic: article
ms.openlocfilehash: 7df064f4afe5fa8536bd35003a397ab85ea1bf67
ms.sourcegitcommit: 5f234fb15c1d0365b60e83a50bf953e317d6239c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879647"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>使用 AD FS 2019 風險評估模型建置外掛程式

您現在可以建立自己的外掛程式來封鎖或指派風險分數給驗證要求，在不同的階段中-已接收要求、預先驗證和後續驗證。 您可以使用 AD FS 2019 引進的新風險評估模型來完成這項作業。

## <a name="what-is-the-risk-assessment-model"></a>什麼是風險評估模型？

風險評估模型是一組介面和類別，可讓開發人員讀取驗證要求標頭，並執行自己的風險評定邏輯。 執行的程式碼 (外掛程式) 接著會在 AD FS 驗證程式行執行。 例如，使用模型所包含的介面和類別，您可以根據要求標頭中包含的用戶端 IP 位址，執行程式碼以封鎖或允許驗證要求。 AD FS 會執行每個驗證要求的程式碼，並根據已執行的邏輯採取適當的動作。

模型可讓您在 AD FS authentication 管線的三個階段中的任一個外掛程式程式碼，如下所示

![model](media/ad-fs-risk-assessment-model/risk1.png)

1.    **要求收到的階段** ：當 AD FS 收到驗證要求（亦即使用者輸入認證之前）時，可讓建立外掛程式允許或封鎖要求。 您可以在這個階段使用要求內容 (例如用戶端 IP、Http 方法、proxy 伺服器 DNS 等等 ) 來執行風險評估。 例如，您可以建立外掛程式以從要求內容讀取 IP，並在 IP 位於具風險 Ip 的預先定義清單中時封鎖驗證要求。

2.    **預先驗證階段** –可讓建立外掛程式在使用者提供認證但 AD FS 評估之前，允許或封鎖要求。 在這個階段，除了要求內容之外，您還可以取得安全性內容的相關資訊 (例如，使用者權杖、使用者識別碼等) 和通訊協定內容 (例如驗證通訊協定、clientid、) resourceid 等，以在您的風險評量邏輯中使用。 例如，您可以建立外掛程式以防止密碼噴灑攻擊，方法是讀取使用者權杖中的使用者密碼，並封鎖驗證要求（如果密碼在預先定義的具風險密碼清單中）。

3.    **後續驗證** –可讓您在使用者提供認證並 AD FS 執行驗證之後，讓大樓外掛程式評估風險。 在這個階段，除了要求內容、安全性內容和通訊協定內容之外，您還可以取得驗證結果的相關資訊 (成功或失敗) 。 外掛程式可以根據可用的資訊來評估風險分數，並將風險分數傳遞給宣告和原則規則，以供進一步評估之用。

為了進一步瞭解如何建立風險評定外掛程式，並將其與 AD FS 程式一起執行，讓我們建立一個範例外掛程式，以封鎖來自某些 **外部** 網路 ip 識別為有風險的要求、向 AD FS 註冊外掛程式，最後測試該功能。

>[!NOTE]
>或者，您可以建立具 [風險的使用者外掛程式](https://github.com/microsoft/adfs-sample-block-user-on-adfs-marked-risky-by-AzureAD-IdentityProtection)，此範例外掛程式會利用 Azure AD Identity Protection 所決定的使用者風險層級來封鎖驗證，或 (MFA) 強制執行多重要素驗證。 [這裡](https://github.com/microsoft/adfs-sample-block-user-on-adfs-marked-risky-by-AzureAD-IdentityProtection)提供建立具風險的使用者外掛程式的步驟

## <a name="building-a-sample-plug-in"></a>建立範例外掛程式

>[!NOTE]
>本逐步解說只示範如何建立範例外掛程式。 也就是說，我們要建立企業就緒的解決方案。

### <a name="pre-requisites"></a>必要條件
以下是建立此範例外掛程式所需的先決條件清單

- 已安裝並設定 AD FS 2019
- .NET Framework 4.7 和更新版本
- Visual Studio

### <a name="build-plug-in-dll"></a>組建外掛程式 dll
下列程式將逐步引導您建立範例外掛程式 dll。

1. 下載範例外掛程式、使用 Git Bash，然後輸入下列內容：

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. 在 AD FS 伺服器上的任何位置建立 **.csv** 檔案 (在我的案例中，我在 **C:\extensions**) 建立了 **authconfigdb.csv** 檔案，然後將您要封鎖的 ip 新增至這個檔案。

   範例外掛程式將會封鎖來自此檔案中所列外部網路 **ip** 的任何驗證要求。

   >{!注意] 如果您有 AD FS 伺服器陣列，您可以在任何或所有的 AD FS 伺服器上建立檔案。 任何檔案都可以用來將具風險的 Ip 匯入 AD FS。 我們將在下面的 AD FS 一節中詳細討論匯入 [程式](#register-the-plug-in-dll-with-ad-fs) 。

3. `ThreatDetectionModule.sln`使用 Visual Studio 開啟專案

4. `Microsoft.IdentityServer.dll`從 [方案瀏覽器] 中移除，如下所示：</br>
   ![model](media/ad-fs-risk-assessment-model/risk2.png)

5. 將參考新增至 `Microsoft.IdentityServer.dll` 您的 AD FS，如下所示

   a.    以滑鼠右鍵按一下 [**方案瀏覽器**] 中的 [**參考**]，然後選取 [**加入參考**]。</br>
   ![model](media/ad-fs-risk-assessment-model/risk3.png)

   b.    在 [ **參考管理員** ] 視窗中，選取 **[流覽]**。 在 [**選取要參考的檔案 ...** ] 對話方塊中， `Microsoft.IdentityServer.dll` 從 [我的案例 **C:\Windows\ADFS** ] 中 AD FS 的安裝資料夾 (選取) 然後按一下 [ **新增**]。

   >[!NOTE]
   >在我的案例中，我會在 AD FS 伺服器本身上建立外掛程式。 如果您的開發環境位於不同的伺服器上，請 `Microsoft.IdentityServer.dll` 從 AD FS server 上的 AD FS 安裝資料夾，複製到您的開發箱。</br>

   ![model](media/ad-fs-risk-assessment-model/risk4.png)

   c.    確定已選取核取方塊之後，請按一下 [**參考管理員**] 視窗上的 **[確定**] `Microsoft.IdentityServer.dll`</br>
   ![model](media/ad-fs-risk-assessment-model/risk5.png)

6. 所有類別和參考現在都已就緒，可進行組建。   不過，由於此專案的輸出是 dll，因此必須將它安裝到 AD FS 伺服器的 **全域組件快取**（GAC）中，而且必須先簽署 dll。 以下步驟可以達到此目的：

   a.    以 **滑鼠右鍵按一下** 專案的名稱 ThreatDetectionModule。 從功能表按一下 [ **屬性**]。</br>
   ![model](media/ad-fs-risk-assessment-model/risk6.png)

   b.    在 [ **屬性** ] 頁面上，按一下左側的 [簽署]，然後 **核** 取標示為 **[簽署元件**] 的核取方塊。 從 [ **選擇強式名稱金鑰** 檔：] 下拉式功能表中，選取 **<新增] >**</br>
   ![model](media/ad-fs-risk-assessment-model/risk7.png)

   c.    在 [ **建立強式名稱金鑰] 對話方塊** 中，輸入名稱 (您可以為金鑰選擇任何名稱) ，取消核取 [ **以密碼保護我的金鑰** 檔] 核取方塊。 然後按一下 **[確定]**。</br>
   ![model](media/ad-fs-risk-assessment-model/risk8.png)

   d.    儲存專案，如下所示</br>
   ![model](media/ad-fs-risk-assessment-model/risk9.png)

7. 按一下 [ **建立** ] 然後 **重建方案** 以建立專案，如下所示</br>
   ![model](media/ad-fs-risk-assessment-model/risk10.png)

   檢查畫面底部的 [ **輸出] 視窗**，查看是否發生任何錯誤</br>
   ![model](media/ad-fs-risk-assessment-model/risk11.png)


外掛程式 (dll) 現在已可供使用，而且位於專案資料夾的 [ **\bin\Debug** ] 資料夾中， (在我的案例中， **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**) 。

下一步是向 AD FS 登錄此 dll，因此它會與 AD FS 驗證程式一起執行。

### <a name="register-the-plug-in-dll-with-ad-fs"></a>使用 AD FS 註冊外掛程式 dll

我們需要在 AD FS 伺服器上使用 PowerShell 命令在 AD FS 中登錄 dll `Register-AdfsThreatDetectionModule` ，但在註冊之前，我們需要取得公開金鑰 Token。 當我們建立金鑰，並使用該金鑰簽署 dll 時，就會建立此公開金鑰標記。 若要瞭解 dll 的公開金鑰標記，您可以使用 **SN.exe** ，如下所示。

1. 將 dll 檔案從 **\bin\Debug** 資料夾複製到另一個位置， (在我的案例中，將它複製到 **C:\extensions**) 

2. 開始 Visual Studio 的 **開發人員命令提示字元** ，然後移至包含 **sn.exe** (的目錄。在我的案例中，目錄是 **C:\Program Files (X86) \microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**) ![ model](media/ad-fs-risk-assessment-model/risk12.png)

3. 使用 **-T** 參數和檔案在我的案例中的位置) 模型來執行 **SN** 命令 `SN -T "C:\extensions\ThreatDetectionModule.dll"` (![](media/ad-fs-risk-assessment-model/risk13.png)</br>
   此命令會為您提供公開金鑰 token (， **公開金鑰 token 為 714697626ef96b35**) 

4. 將 dll 加入 AD FS server 的 **全域組件快取** 中，我們的最佳作法是為您的專案建立適當的安裝程式，並使用安裝程式將檔案加入至 GAC。 另一個解決方案是使用 **Gacutil.exe** (您的開發電腦上 [) 所](/dotnet/framework/tools/gacutil-exe-gac-tool)提供 **Gacutil.exe** 的詳細資訊。  由於我在 AD FS 的相同伺服器上有 visual studio，因此我將使用 **Gacutil.exe** ，如下所示

   a.    在 Visual Studio 的開發人員命令提示字元，並移至在我的案例中包含 **Gacutil.exe** (的目錄，目錄是 **C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**) 

   b.    在 `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`) 模型的案例中執行 Gacutil 命令 ![ (](media/ad-fs-risk-assessment-model/risk14.png)

   >[!NOTE]
   >如果您有 AD FS 伺服器陣列，則必須在伺服器陣列中的每個 AD FS 伺服器上執行上述程式。

5. 開啟 **Windows PowerShell** ，然後執行下列命令以註冊 dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>"
   ```
   在我的案例中，命令是：
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv"
   ```

   >[!NOTE]
   >您只需要註冊一次 dll，即使您有 AD FS 伺服器陣列也一樣。

6. 註冊 dll 之後，重新開機 AD FS 服務

這樣就完成了，dll 現在已向 AD FS 註冊並可供使用！

 >[!NOTE]
 > 如果對外掛程式進行了任何變更，並重建專案，則必須重新註冊更新的 dll。 在註冊之前，您必須使用下列命令取消註冊目前的 dll：</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br>
 >在我的案例中，命令是：
 >```
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>測試外掛程式

1. 開啟我們稍早在位置 **C:\extensions** 的案例中所建立的 **authconfig.csv** 檔案 () 然後新增您想要封鎖的 **外部網路 ip** 。 每個 IP 都應該在不同的行上，且結尾不能有空格。</br>
   ![model](media/ad-fs-risk-assessment-model/risk18.png)

2. 儲存並關閉檔案

3. 藉由執行下列 PowerShell 命令，在 AD FS 中匯入更新的檔案

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```

   在我的案例中，命令是：
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```

4. 使用您在 **authconfig.csv** 中新增的相同 IP，從伺服器起始驗證要求。

   在此示範中，我將使用 [AD FS 協助宣告 X 光線工具](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) 來起始要求。 如果您想要使用 X 光線工具，請依照指示進行。

   輸入同盟伺服器實例和點擊 **測試驗證** 按鈕。</br>
   ![model](media/ad-fs-risk-assessment-model/risk15.png)

5. 驗證會遭到封鎖，如下所示。</br>
   ![model](media/ad-fs-risk-assessment-model/risk16.png)

現在我們已知道如何建立和註冊外掛程式，接下來讓我們逐步解說外掛程式程式碼，以瞭解如何使用模型引進的新介面和類別來瞭解執行。

## <a name="plug-in-code-walkthrough"></a>外掛程式程式碼逐步解說

`ThreatDetectionModule.sln`使用 Visual Studio 開啟專案，然後從畫面右側的 **方案瀏覽器** 開啟主要檔案 **UserRiskAnalyzer.cs**</br>
![model](media/ad-fs-risk-assessment-model/risk17.png)

檔案包含主要類別 UserRiskAnalyzer，它會執行抽象類別 [ThreatDetectionModule](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule) 和介面 [IRequestReceivedThreatDetectionModule](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule) ，從要求內容中讀取 ip、將取得的 IP 與從 AD FS DB 載入的 ip 進行比較，並在 ip 相符時封鎖要求。 讓我們更詳細地探討這些類型

### <a name="threatdetectionmodule-abstract-class"></a>ThreatDetectionModule 抽象類別

這個抽象類別會將外掛程式載入 AD FS 管線，讓您能夠在 AD FS 進程的程式列中執行外掛程式程式碼。

```
public abstract class ThreatDetectionModule
{
    protected ThreatDetectionModule();

    public abstract string VendorName { get; }
    public abstract string ModuleIdentifier { get; }

 public abstract void OnAuthenticationPipelineLoad(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData);
 public abstract void OnAuthenticationPipelineUnload(ThreatDetectionLogger logger);
  public abstract void OnConfigurationUpdate(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData);
 }
```
類別包含下列方法和屬性。

|方法 |類型|定義|
|-----|-----|-----|
|[OnAuthenticationPipelineLoad](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload) |Void|外掛程式載入至其管線時由 AD FS 呼叫|
|[OnAuthenticationPipelineUnload](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload) |Void|從管線卸載外掛程式時由 AD FS 呼叫|
|[OnConfigurationUpdate](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate)| Void|在設定更新時由 AD FS 呼叫 |
|**屬性** |**型別** |**[定義]**|
|[VendorName](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname)|String |取得擁有外掛程式之廠商的名稱|
|[ModuleIdentifier](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier)|String |取得外掛程式的識別碼|

在我們的範例外掛程式中，我們會使用 [OnAuthenticationPipelineLoad](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload) 和 [OnConfigurationUpdate](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate) 方法來讀取 AD FS DB 中預先定義的 ip。 當外掛程式註冊 AD FS 時，會呼叫[OnAuthenticationPipelineLoad](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload) ，而當使用 Cmdlet 匯入 .csv 時，會呼叫[OnConfigurationUpdate](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate) 。 `Import-AdfsThreatDetectionModuleConfiguration`

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>IRequestReceivedThreatDetectionModule 介面

此 [介面](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule) 可讓您在 AD FS 收到驗證要求時，但在使用者輸入認證（也就是在驗證程式的收到要求階段）之前，執行風險評估。

```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger,
RequestContext requestContext );
}
```

此介面包含 [EvaluateRequest](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest) 方法，可讓您使用 requestCoNtext 輸入參數中所傳遞之驗證要求的內容，以撰寫您的風險評量邏輯。 RequestCoNtext 參數的類型為 [requestCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext)。

傳遞的另一個輸入參數是 [ThreatDetectionLogger](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger)類型的記錄器。 參數可以用來將錯誤、audit 和/或 debug 訊息寫入 AD FS 記錄。

如果 NotEvaluated、1至 Block，方法會傳回 [ThrottleStatus](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus) (0，而2則會允許) AD FS 然後封鎖或允許要求。

在我們的範例外掛程式中， [EvaluateRequest](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest)方法執行會從[RequestCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext)參數剖析[clientIpAddress](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) ，並將其與從 AD FS DB 載入的所有 ip 進行比較。 如果找到相符的，方法會針對 **Block** 傳回2，否則會針對 **Allow** 傳回1。 根據傳回的值，AD FS 封鎖或允許要求。

>[!NOTE]
>上面所討論的範例外掛程式只會實 IRequestReceivedThreatDetectionModule 介面。 不過，風險評估模型提供兩個額外的介面： IPreAuthenticationThreatDetectionModule (來實行風險評定邏輯 duing 預先驗證階段) 和 IPostAuthenticationThreatDetectionModule (，在驗證後階段) 執行風險評估邏輯。 以下提供兩個介面上的詳細資料。

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>IPreAuthenticationThreatDetectionModule 介面

此 [介面](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule) 可讓您在使用者提供認證的時間點，但在 AD FS 評估之前（即預先驗證階段）來執行風險評定邏輯。

```
public interface IPreAuthenticationThreatDetectionModule
{
Task<ThrottleStatus> EvaluatePreAuthentication (
ThreatDetectionLogger logger,
RequestContext requestContext,
SecurityContext securityContext,
ProtocolContext protocolContext,
IList<Claim> additionalClams
);
}
```
此介面包含 [EvaluatePreAuthentication](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication) 方法，可讓您使用 [RequestCoNtext RequestCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext)、 [SecurityCoNtext SecurityCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext)、 [ProtocolCoNtext ProtocolCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext)和 [IList <Claim> additionalClams](/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2&preserve-view=true) 輸入參數中傳遞的資訊，來撰寫預先驗證風險評定邏輯。

>[!NOTE]
>如需每個內容類型傳遞的屬性清單，請造訪 [RequestCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext)、 [SecurityCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext)和 [ProtocolCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext) 類別定義。

傳遞的另一個輸入參數是 [ThreatDetectionLogger](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger)類型的記錄器。 參數可以用來將錯誤、audit 和/或 debug 訊息寫入 AD FS 記錄。

如果 NotEvaluated、1至 Block，方法會傳回 [ThrottleStatus](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus) (0，而2則會允許) AD FS 然後封鎖或允許要求。

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>IPostAuthenticationThreatDetectionModule 介面

此 [介面](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule) 可讓您在使用者提供認證，並 AD FS 執行驗證（即驗證後階段）之後，執行風險評定邏輯。

```
public interface IPostAuthenticationThreatDetectionModule
{
Task<RiskScore> EvaluatePostAuthentication (
ThreatDetectionLogger logger,
RequestContext requestContext,
SecurityContext securityContext,
ProtocolContext protocolContext,
AuthenticationResult authenticationResult,
IList<Claim> additionalClams
);
}
```

此介面包含 [EvaluatePostAuthentication](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication) 方法，可讓您使用 [RequestCoNtext RequestCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext)、 [SecurityCoNtext SecurityCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext)、 [ProtocolCoNtext ProtocolCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext)和 [IList <Claim> additionalClams](/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2&preserve-view=true) 輸入參數中傳遞的資訊，來撰寫驗證後風險評定邏輯。

>[!NOTE]
> 如需每個內容類型傳遞之屬性的完整清單，請參閱 [RequestCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext)、 [SecurityCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext)和 [ProtocolCoNtext](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext) 類別定義。

傳遞的另一個輸入參數是 [ThreatDetectionLogger](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger)類型的記錄器。 參數可以用來將錯誤、audit 和/或 debug 訊息寫入 AD FS 記錄。

方法會傳回可用於 AD FS 原則和宣告規則的 [風險分數](/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants) 。

>[!NOTE]
>若要讓外掛程式運作， (在此案例中的主要類別) 需要衍生 [ThreatDetectionModule](/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule) 抽象類別，且應至少執行上述三個介面的其中一個。 登錄 dll 之後，AD FS 會檢查所要執行的介面，並在管線中適當的階段呼叫這些介面。

### <a name="faqs"></a>常見問題集

**為什麼要建立這些外掛程式？**</br>
**答：** 這些外掛程式不僅能提供您額外的功能來保護您的環境免于遭受像是密碼噴灑攻擊的攻擊，還可讓您根據自己的需求彈性地建立自己的風險評定邏輯。

**記錄檔的捕獲位置為何？**</br>
**答：** 您可以使用 WriteAdminLogErrorMessage 方法將錯誤記錄檔寫入「AD FS/系統管理員」事件記錄檔，並使用 WriteAuditMessage 方法將記錄檔審核記錄至「AD FS 審核」安全性記錄，並使用 WriteDebugMessage 方法將記錄檔偵測至「AD FS 追蹤」的調試記錄。

**新增這些外掛程式會增加 AD FS authentication 進程延遲？**</br>
**答：** 延遲影響將取決於執行您所實行的風險評估邏輯所花的時間。 在生產環境中部署外掛程式之前，建議您先評估延遲的影響。

**為什麼無法 AD FS 建議具風險的 Ip、使用者等清單？**</br>
**答：** 雖然目前無法使用，但我們正致力於在插入風險評估模型中建立智慧，以建議具風險的 Ip、使用者等等。 我們很快就會分享啟動日期。

**還有其他可用的範例外掛程式？**</br>
**答：** 下列 (s) 範例可供使用：

|名稱|描述|
|-----|-----|
|[具風險的使用者外掛程式](https://github.com/microsoft/adfs-sample-block-user-on-adfs-marked-risky-by-AzureAD-IdentityProtection)|根據 Azure AD Identity Protection 所決定的使用者風險層級，封鎖驗證或強制執行 MFA 的範例外掛程式。|
