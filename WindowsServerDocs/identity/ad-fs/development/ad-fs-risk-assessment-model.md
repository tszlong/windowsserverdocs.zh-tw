---
title: 組建具有 AD FS 2019 風險評估模型的外掛程式
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: af7a565fb5b3745531497ed9119976418eb6dcd7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857521"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>組建具有 AD FS 2019 風險評估模型的外掛程式

您現在可以建立自己的外掛程式來封鎖或指派風險分數給各種階段的驗證要求–已接收的要求、預先驗證和後續驗證。 您可以使用 AD FS 2019 引進的新風險評估模型來完成這項作業。 

## <a name="what-is-the-risk-assessment-model"></a>什麼是風險評估模型？

風險評估模型是一組介面和類別，可讓開發人員讀取驗證要求標頭，並實行自己的風險評估邏輯。 然後，已執行的程式碼（外掛程式）會以 AD FS authentication 程式執行。 例如，使用模型中所包含的介面和類別，您可以根據要求標頭中包含的用戶端 IP 位址，執行程式碼以封鎖或允許驗證要求。 AD FS 將會針對每個驗證要求執行程式碼，並根據實作為邏輯來採取適當的動作。 

此模型可讓您在 AD FS authentication 管線的三個階段中的任何一處插入外掛程式程式碼，如下所示

![模型](media/ad-fs-risk-assessment-model/risk1.png)

1.    **要求接收階段**–可讓您在 AD FS 收到驗證要求（例如使用者輸入認證之前）時，建立外掛程式來允許或封鎖要求。 您可以使用此階段所提供的要求內容（例如，用戶端 IP、Http 方法、proxy 伺服器 DNS 等）來執行風險評估。 例如，您可以建立外掛程式來讀取要求內容中的 IP，並在 IP 位於具風險 Ip 的預先定義清單中時，封鎖驗證要求。 

2.    **預先驗證階段**–可讓您在使用者提供認證但 AD FS 評估之前，建立外掛程式來允許或封鎖要求。 在這個階段中，除了要求內容之外，您還可以使用風險評估邏輯中的安全性內容（例如，使用者權杖、使用者識別碼等）和通訊協定內容（例如，驗證通訊協定、clientid、resourceid 等）的相關資訊。 例如，您可以建立外掛程式來防止密碼噴灑攻擊，方法是從使用者權杖讀取使用者密碼，並封鎖驗證要求（如果密碼在預先定義的具風險密碼清單中）。 

3.    **後驗證**–可讓建立外掛程式在使用者提供認證並 AD FS 執行驗證後，評估風險。 在這個階段，除了要求內容、安全性內容和通訊協定內容之外，您還可以取得驗證結果（成功或失敗）的相關資訊。 外掛程式可以根據可用的資訊來評估風險分數，並將風險分數傳遞給宣告和原則規則以進一步評估。 

為了進一步瞭解如何建立風險評估外掛程式，並在 AD FS 程式中執行它，讓我們建立一個範例外掛程式，它會封鎖來自特定**外部**網路 ip 的要求，並將該外掛程式向 AD FS 註冊，最後再測試功能。 

>[!NOTE]
>本逐步解說只是為了說明如何建立範例外掛程式。 這種解決方案的解決方法是，我們正在建立企業就緒的解決方案。  

## <a name="building-a-sample-plug-in"></a>建立範例外掛程式

### <a name="pre-requisites"></a>必要條件
以下是建立此範例外掛程式所需的必要條件清單

- 已安裝並設定 AD FS 2019
- .NET Framework 4.7 和更新版本
- Visual Studio

### <a name="build-plug-in-dll"></a>組建外掛程式 dll
下列程式將逐步引導您建立範例外掛程式 dll。

1. 下載範例外掛程式，使用 Git Bash 並輸入下列內容： 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. 在 AD FS 伺服器上的任何位置建立 **.csv**檔案（在我的案例中，我在**C:\extensions**建立了**authconfigdb .csv**檔案），並將您要封鎖的 ip 新增至這個檔案。 

   範例外掛程式會封鎖來自此檔案中所列**外部網路 ip**的任何驗證要求。 

   >{!注意] 如果您有 AD FS 伺服器陣列，您可以在任何或所有 AD FS 伺服器上建立檔案。 任何檔案都可以用來將有風險的 Ip 匯入 AD FS。 我們將在下面的[向 AD FS 註冊外掛程式 dll](#register-the-plug-in-dll-with-ad-fs)一節中詳細討論匯入程式。 

3. 使用 Visual Studio 開啟專案 `ThreatDetectionModule.sln`

4. 從 [方案瀏覽器] 中移除 `Microsoft.IdentityServer.dll`，如下所示：</br>
   ![模型](media/ad-fs-risk-assessment-model/risk2.png)

5. 將參考新增至 AD FS 的 `Microsoft.IdentityServer.dll`，如下所示

   a.    以滑鼠右鍵按一下**方案瀏覽器**中的 [**參考**]，然後選取 [**新增參考 ...** ]</br> 
   ![模型](media/ad-fs-risk-assessment-model/risk3.png)
   
   b。    在 [**參考管理員**] 視窗中，選取 **[流覽]** 。 在 [**選取要參考的檔案 ...** ] 對話方塊中，從您的 AD FS 安裝資料夾（在**C:\Windows\ADFS**中）選取 [`Microsoft.IdentityServer.dll`]，然後按一下 [**新增**]。
   
   >[!NOTE]
   >在我的案例中，我會在 AD FS 伺服器本身上建立外掛程式。 如果您的開發環境是在不同的伺服器上，請將 `Microsoft.IdentityServer.dll` 從 AD FS 伺服器上的 AD FS 安裝資料夾複製到您的開發箱。</br> 
   
   ![模型](media/ad-fs-risk-assessment-model/risk4.png)
   
   c.    確定已選取 [`Microsoft.IdentityServer.dll`] 核取方塊之後，請在 [**參考管理員**] 視窗中按一下 **[確定**]</br>
   ![模型](media/ad-fs-risk-assessment-model/risk5.png)
 
6. 所有類別和參考現在都已準備好執行組建。   不過，因為此專案的輸出是 dll，所以必須將它安裝到 AD FS 伺服器的**全域組件快取**或 GAC 中，而且必須先簽署 dll。 這可以透過下列方式來完成：

   a.    以**滑鼠右鍵按一下**專案的名稱，ThreatDetectionModule。 從功能表中，按一下 [**屬性**]。</br>
   ![模型](media/ad-fs-risk-assessment-model/risk6.png)
   
   b。    從 [**屬性**] 頁面上，按一下左側的 [簽署]，然後**核**取標示為 **[簽署元件**] 的核取方塊。 從 [**選擇強式名稱金鑰**檔：] 下拉式功能表中，選取 [ **< 新增]。>**</br>
   ![模型](media/ad-fs-risk-assessment-model/risk7.png)

   c.    在 [**建立強式名稱金鑰] 對話方塊**中，輸入金鑰的名稱（您可以選擇任何名稱），取消核取 [**以密碼保護我的金鑰**檔] 核取方塊。 然後按一下 **[確定]** 。
   ![模型](media/ad-fs-risk-assessment-model/risk8.png)</br>
 
   d.    儲存專案，如下所示</br>
   ![模型](media/ad-fs-risk-assessment-model/risk9.png)

7. 依序按一下 [**建立**] 和 [**重建方案**] 來建立專案，如下所示</br>
   ![模型](media/ad-fs-risk-assessment-model/risk10.png)
 
   檢查畫面底部的 [**輸出] 視窗**，查看是否發生任何錯誤</br>
   ![模型](media/ad-fs-risk-assessment-model/risk11.png)


外掛程式（dll）現在已可供使用，而且位於專案資料夾的 **\bin\Debug**資料夾中（在我的案例中，這是**C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**）。 

下一步是向 AD FS 註冊此 dll，因此它會在 AD FS 驗證程式中執行。 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>向 AD FS 註冊外掛程式 dll

我們需要在 AD FS 伺服器上使用 `Register-AdfsThreatDetectionModule` PowerShell 命令在 AD FS 中註冊 dll，不過，在註冊之前，我們必須先取得公開金鑰 Token。 當我們建立金鑰並使用該金鑰簽署 dll 時，就會建立此公開金鑰 token。 若要瞭解 dll 的公用金鑰 Token，您可以使用**sn.exe** ，如下所示

1. 將 dll 檔案從 **\bin\Debug**資料夾複製到另一個位置（在我的案例中，將它複製到**C:\extensions**）

2. 啟動 Visual Studio 的**開發人員命令提示字元**，並移至包含**sn.exe**的目錄（在我的案例中，目錄是**C:\Program Files （X86） \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**） ![model](media/ad-fs-risk-assessment-model/risk12.png)

3. 使用 **-T**參數和檔案的位置（在我的案例中 `SN -T "C:\extensions\ThreatDetectionModule.dll"`）來執行**SN**命令，![模型](media/ad-fs-risk-assessment-model/risk13.png)</br>
   此命令會為您提供公開金鑰權杖（對我而言，**公開金鑰 token 為 714697626ef96b35**）

4. 將 dll 加入 AD FS 伺服器的**全域組件快取**中，我們的最佳做法是為您的專案建立適當的安裝程式，並使用安裝程式將檔案新增至 GAC。 另一個解決方案是在您的開發電腦上使用**Gacutil** （如需有關**Gacutil**的詳細資訊，請參閱[這裡](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)提供）。  由於我的 visual studio 與 AD FS 在同一部伺服器上，因此我將使用**Gacutil** ，如下所示

   a.    在 Visual Studio 的開發人員命令提示字元上，並移至包含**Gacutil**的目錄（在我的案例中，目錄是**C:\Program Files （x86） \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**）

   b。    執行**Gacutil**命令（在我的案例中 `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`） ![模型](media/ad-fs-risk-assessment-model/risk14.png)
 
   >[!NOTE]
   >如果您有 AD FS 伺服器陣列，則必須在伺服器陣列中的每個 AD FS 伺服器上執行上述動作。 

5. 開啟**Windows PowerShell**並執行下列命令來註冊 dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>"
   ```
   在我的案例中，命令為： 
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv"
   ```
 
   >[!NOTE]
   >您只需要註冊一次 dll，即使您有 AD FS 的伺服器陣列也一樣。 

6. 在註冊 dll 之後重新開機 AD FS 服務

這就是，dll 現在已向 AD FS 註冊，並可供使用！

 >[!NOTE]
 > 如果對外掛程式進行任何變更，並重建專案，則必須重新註冊更新的 dll。 在註冊之前，您必須使用下列命令來取消註冊目前的 dll：</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> 在我的案例中，命令為：
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>測試外掛程式

1. 開啟我們稍早建立的**authconfig** （在位置**C:\extensions**的案例中），並新增您想要封鎖的**外部網路 ip** 。 每個 IP 都應該位於不同的行，結尾處不應該有空格</br>
   ![模型](media/ad-fs-risk-assessment-model/risk18.png)
 
2. 儲存並關閉檔案。

3. 藉由執行下列 PowerShell 命令，在 AD FS 中匯入更新的檔案 

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```
 
   在我的案例中，命令為： 
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```
 
4. 使用您在**authconfig**中新增的相同 IP，起始伺服器的驗證要求。

   在此示範中，我將使用[AD FS 協助宣告 X 光線工具](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)來起始要求。 如果您想要使用 X 光線工具，請遵循指示 

   輸入同盟伺服器實例，然後點擊**測試驗證**按鈕。</br> 
   ![模型](media/ad-fs-risk-assessment-model/risk15.png) 

5. 驗證會遭到封鎖，如下所示。</br>
   ![模型](media/ad-fs-risk-assessment-model/risk16.png)
 
既然我們已經知道如何建立和註冊外掛程式，讓我們逐步解說外掛程式程式碼，以使用模型引進的新介面和類別來瞭解此實作為的執行方式。 

## <a name="plug-in-code-walkthrough"></a>外掛程式程式碼逐步解說

使用 Visual Studio 開啟專案 `ThreatDetectionModule.sln`，然後從畫面右側的 [**方案瀏覽器**] 開啟主要檔案**UserRiskAnalyzer.cs**</br>
![模型](media/ad-fs-risk-assessment-model/risk17.png)
 
檔案包含主要類別 UserRiskAnalyzer，其會執行抽象類別[ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)和介面[IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) ，以從要求內容讀取 IP、將取得的 IP 與從 AD FS DB 載入的 ip 進行比較，並在有 ip 相符的情況下封鎖要求。 讓我們更詳細地說明這些類型

### <a name="threatdetectionmodule-abstract-class"></a>ThreatDetectionModule 抽象類別

這個抽象類別會將外掛程式載入 AD FS 管線，讓您可以使用 AD FS 進程來執行外掛程式程式碼。 

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

|方法 |Type|定義|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Void|當外掛程式載入其管線時，由 AD FS 呼叫| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Void|當外掛程式從其管線中卸載時，由 AD FS 呼叫| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Void|由 AD FS on config update 呼叫 |
|**Property** |**類型** |**定義**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|String |取得擁有此外掛程式的廠商名稱|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|String |取得外掛程式的識別碼|

在我們的範例外掛程式中，我們會使用[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019)和[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)方法，從 AD FS DB 讀取預先定義的 ip。 當外掛程式向 AD FS 註冊時，會呼叫[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) ，而當使用 `Import-AdfsThreatDetectionModuleConfiguration` Cmdlet 匯入 .csv 時，會呼叫[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) 。 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>IRequestReceivedThreatDetectionModule 介面

此[介面](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019)可讓您在 AD FS 收到驗證要求時，但在使用者輸入認證（也就是在驗證程式的已收到要求階段）之前，執行風險評估。 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

此介面包含[EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019)方法，可讓您使用傳入 requestCoNtext 輸入參數的驗證要求內容來撰寫您的風險評估邏輯。 RequestCoNtext 參數的類型是[requestCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)。 

傳遞的另一個輸入參數是[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)類型的記錄器。 參數可以用來將錯誤、audit 和/或 debug 訊息寫入 AD FS 記錄。 

方法會傳回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) （0如果 NotEvaluated，1到 Block，2則允許）以 AD FS，然後封鎖或允許要求。

在我們的範例外掛程式中， [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019)方法執行會從[RequestCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)參數剖析[clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) ，並將它與從 AD FS DB 載入的所有 ip 進行比較。 如果找到相符的，方法會針對**Block**傳回2，否則會針對**Allow**傳回1。 根據傳回的值，AD FS 封鎖或允許要求。 

>[!NOTE]
>上面所討論的範例外掛程式只會執行 IRequestReceivedThreatDetectionModule 介面。 不過，風險評估模型提供兩個額外的介面– IPreAuthenticationThreatDetectionModule （以執行風險評估邏輯 duing 預先驗證階段）和 IPostAuthenticationThreatDetectionModule （在驗證後階段中執行風險評估邏輯）。 以下提供兩個介面的詳細資料。 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>IPreAuthenticationThreatDetectionModule 介面 

此[介面](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019)可讓您在使用者提供認證的位置，但在 AD FS 評估之前執行風險評估邏輯，例如預先驗證階段。 

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
此介面包含[EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019)方法，可讓您使用[RequestCoNtext RequestCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityCoNtext SecurityCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)、 [ProtocolCoNtext ProtocolCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)和 IList 中傳遞的資訊， [<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)輸入參數來撰寫預先驗證風險評估邏輯。 

>[!NOTE]
>如需每個內容類型所傳遞屬性的清單，請造訪[RequestCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)和[ProtocolCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)類別定義。 

傳遞的另一個輸入參數是[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)類型的記錄器。 參數可以用來將錯誤、audit 和/或 debug 訊息寫入 AD FS 記錄。

方法會傳回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) （0如果 NotEvaluated，1到 Block，2則允許）以 AD FS，然後封鎖或允許要求。 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>IPostAuthenticationThreatDetectionModule 介面

此[介面](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019)可讓您在使用者提供認證，並 AD FS 執行驗證（亦即驗證後階段）後，執行風險評估邏輯。 

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

此介面包含[EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019)方法，可讓您使用[RequestCoNtext RequestCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityCoNtext SecurityCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)、 [ProtocolCoNtext ProtocolCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)和 IList 中傳遞的資訊， [<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)輸入參數來撰寫您的驗證後風險評估邏輯。 

>[!NOTE]
> 如需每個內容類型所傳遞之屬性的完整清單，請參閱[RequestCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)、 [SecurityCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)和[ProtocolCoNtext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)類別定義。 

傳遞的另一個輸入參數是[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)類型的記錄器。 參數可以用來將錯誤、audit 和/或 debug 訊息寫入 AD FS 記錄。 

方法會傳回可用於 AD FS 原則和宣告規則中的[風險分數](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019)。 

>[!NOTE]
>若要讓外掛程式能夠正常執行，主要類別（在此案例中為 UserRiskAnalyzer）必須衍生[ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)抽象類別，而且應該至少實作為上述三個介面的其中一個。 一旦註冊 dll 之後，AD FS 會檢查哪些介面已執行，並在管線中適當的階段呼叫它們。

### <a name="faqs"></a>常見問題集

**為什麼要建立這些外掛程式？**</br>
**答：** 這些外掛程式不僅能提供額外的功能來保護您的環境免于遭受攻擊，例如密碼噴灑攻擊，還可讓您根據需求，彈性地建立自己的風險評估邏輯。 

**記錄檔會在何處捕捉？**</br>
**答：** 您可以使用 WriteAdminLogErrorMessage 方法，將錯誤記錄檔寫入「AD FS/系統管理員」事件記錄檔、使用 WriteAuditMessage 方法將記錄到「AD FS 審核」安全性記錄檔，並使用 WriteDebugMessage 方法，將記錄檔「AD FS 追蹤」的 debug 記錄檔。 

**新增這些外掛程式會增加 AD FS 驗證進程延遲嗎？**</br>
**答：** 延遲影響將取決於執行您所實行的風險評估邏輯所花費的時間。 我們建議您在生產環境中部署外掛程式之前，先評估延遲的影響。 

**為什麼無法 AD FS 建議有風險的 Ip、使用者等等的清單？**</br>
**答：** 雖然目前無法使用，但我們正致力於在可插入的風險評估模型中建立智慧，以建議有風險的 Ip、使用者等等。 我們很快就會分享發行日期。 
