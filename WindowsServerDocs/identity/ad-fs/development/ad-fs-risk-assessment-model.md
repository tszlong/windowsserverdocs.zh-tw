---
title: 建置使用 AD FS 2019 風險評估模型的外掛程式
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 80f42695af917084ee63297df052adc069340bb3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190518"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>建置使用 AD FS 2019 風險評估模型的外掛程式

您現在可以建置您自己外掛程式來封鎖或將驗證要求中的風險分數，不同的階段 – 接收、 預先驗證和後續驗證的要求。 這可以使用新的風險評估模型所導入的 AD FS 2019 來完成。 

## <a name="what-is-the-risk-assessment-model"></a>風險評估模型是什麼？

風險評估模型是一組介面和類別，可讓開發人員讀取 驗證要求標頭，以及實作它們自己的風險評估邏輯。 （外掛程式） 實作的程式碼接著會執行與 AD FS 驗證程序。 例如，使用的介面和類別隨附的模型，您可以實作其中一個區塊的程式碼或允許的要求標頭中包含的用戶端 IP 位址為基礎的驗證要求。 AD FS 會執行每個驗證要求的程式碼，並採取適當的動作，根據實作邏輯。 

在任何 AD FS 驗證管線，如下所示的三個階段的模型可讓外掛程式的程式碼

![模型](media\ad-fs-risk-assessment-model\risk1.png)

1.  **要求接收的階段**： 建置外掛程式可讓以允許或封鎖要求，AD FS 收到驗證要求，也就是之前使用者輸入認證時。 您可以使用要求內容 (例如，用戶端 IP、 Http 方法、 proxy 伺服器 DNS 等) 可在此階段中，執行風險評估。 例如，您可以建置外掛程式，以讀取要求內容中的 IP，並封鎖驗證要求，如果是在預先定義的清單中的具風險 Ip 的 IP。 

2.  **預先驗證階段**： 建置外掛程式可讓以允許或封鎖要求之處，其中使用者提供認證，但在 AD FS 加以評估之前。 在這個階段中，除了要求內容也提供您有關的安全性內容 （例如，使用者的語彙基元，使用者識別碼，等等） 和要在您的風險評估邏輯中使用的通訊協定內容 （例如，驗證通訊協定、 clientid、 resourceid、 等）。 例如，您可以建置外掛程式，以防止密碼噴灑攻擊使用者權杖讀取使用者的密碼，且封鎖驗證要求，如果密碼是在預先定義的清單中的具風險的密碼。 

3.  **後續驗證**： 可讓您建置外掛程式，以評估風險之後使用者已提供認證，且 AD FS 已經有執行驗證。 在這個階段，除了要求內容、 安全性內容和通訊協定的內容中，您也可以驗證結果 （成功或失敗） 的詳細資訊。 外掛程式可以評估為基礎的可用資訊的風險分數，並傳遞進一步評估的原則規則與宣告的風險分數。 

若要深入了解如何建置外掛程式的風險評估，並執行與 AD FS 的程序，讓我們建置範例外掛程式會封鎖來自特定要求**外部網路**Ip 識別為有風險、 註冊外掛程式搭配 AD FS和最後測試的功能。 

>[!NOTE]
>本逐步解說只是為了示範如何建立範例外掛程式。 不是我們要建立企業就緒解決方案的解決方案。  

## <a name="building-a-sample-plug-in"></a>建立外掛程式範例

### <a name="pre-requisites"></a>必要條件
以下是建置此範例中外掛程式所需的必要元件清單

- AD FS 2019 安裝和設定
- .NET framework 4.7 和更新版本
- Visual Studio

### <a name="build-plug-in-dll"></a>建立外掛程式的 dll
下列程序將引導您建置範例的外掛程式 dll。

 1. 下載外掛程式範例中，使用 Git Bash 並輸入下列命令： 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

 2. 建立 **.csv**在您的 AD FS 伺服器上的任何位置的檔案 (在本例中，我建立了**authconfigdb.csv**在提出**C:\extensions**) 並新增您想要封鎖到這個檔案的 Ip。 

   此外掛程式的範例將會封鎖來自任何驗證要求**外部網路的 Ip**列在此檔案。 

   >{!NOTE] 如果您有 AD FS 伺服器陣列時，您可以建立任何或所有的 AD FS 伺服器上的檔案。 中的任何檔案可以用來匯入到 AD FS 的具風險的 Ip。 我們將討論匯入程序，詳述[外掛程式 dll 註冊 AD FS](#register-the-plug-in-dll-with-ad-fs)下一節。 

 3. 開啟專案`ThreatDetectionModule.sln`使用 Visual Studio

 4. 移除`Microsoft.IdentityServer.dll`從 [方案總管] 中，如下所示：</br>
 ![model](media\ad-fs-risk-assessment-model\risk2.png)

 5. 將參考加入至`Microsoft.IdentityServer.dll`的 AD FS，如下所示

   a.   以滑鼠右鍵按一下**參考**中**解決方案總管**，然後選取**加入參考...**</br> 
   ![模型](media\ad-fs-risk-assessment-model\risk3.png)
   
   b.   在 **參考管理員**視窗中選取**瀏覽**。 在 **選取要參考的檔案...** ] 對話方塊中，選取`Microsoft.IdentityServer.dll`從您的 AD FS 安裝資料夾 (在我的例子**C:\Windows\ADFS**)，按一下 [**新增**。
   
   >[!NOTE]
    >在本例中我要建置外掛程式在 AD FS 伺服器本身。 如果您的開發環境不同的伺服器上，複製`Microsoft.IdentityServer.dll`從您的開發電腦至 AD FS 伺服器上 AD FS 安裝資料夾。</br> 
   
   ![模型](media\ad-fs-risk-assessment-model\risk4.png)
   
   c.    按一下 [ **[確定]** 上**參考管理員**] 視窗，請先確定`Microsoft.IdentityServer.dll`核取方塊已選取</br>
   ![model](media\ad-fs-risk-assessment-model\risk5.png)
 
 6. 所有類別和參考都已在進行組建的位置。   不過，因為此專案的輸出是 dll，就必須安裝到**Global Assembly Cache**，或需要先簽署 GAC 中的，AD FS 伺服器和 dll。 做法是，如下所示：

   a.   **以滑鼠右鍵按一下**ThreatDetectionModule 專案的名稱。 從功能表中，按一下**屬性**。</br>
   ![model](media\ad-fs-risk-assessment-model\risk6.png)
   
   b.   從**屬性**頁面上，按一下**簽署**，在左邊，並標示核取方塊，然後核取**簽署組件**。 從**選擇強式名稱金鑰檔**： 下拉功能表中，選取 **< 新...>**</br>
   ![model](media\ad-fs-risk-assessment-model\risk7.png)

   c.    在 [**建立強式名稱金鑰] 對話方塊**中，輸入 （您可以選擇任何名稱） 的名稱索引鍵，取消核取此核取方塊**保護我的密碼金鑰檔**。 然後，按一下**確定**。
   ![model](media\ad-fs-risk-assessment-model\risk8.png)</br>
 
   d.   儲存專案，如下所示</br>
   ![model](media\ad-fs-risk-assessment-model\risk9.png)

 7. 建置專案，依序按一下**建置**，然後**重建方案**，如下所示</br>
 ![model](media\ad-fs-risk-assessment-model\risk10.png)
 
 請檢查**輸出視窗**，底部的畫面上，以查看是否發生任何錯誤</br>
 ![model](media\ad-fs-risk-assessment-model\risk11.png)


外掛程式 (dll) 現在可供使用且位於 **\bin\Debug**專案資料夾的資料夾 (在本例中，這**C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**)。 

下一個步驟是使用 AD FS 時，註冊此 dll，讓它與 AD FS 驗證程序執行。 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>與 AD FS 註冊外掛程式的 dll

我們要使用 AD FS 中註冊 dll `Register-AdfsThreatDetectionModule` PowerShell 命令，在 AD FS 伺服器上，不過，我們註冊，我們必須先取得公開金鑰 Token。 當我們建立機碼，並簽署使用該金鑰的 dll，已建立此公開金鑰語彙基元。 若要了解公開金鑰 Token 的 dll，您可以使用**SN.exe** ，如下所示

 1. Dll 檔案複製 **\bin\Debug**到另一個位置的資料夾 (在這個案例中複製到**C:\extensions**)

 2. 開始**開發人員命令提示字元**Visual Studio 並前往目錄包含**sn.exe** (在這個案例中的目錄是**C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 工具**)![模型](media\ad-fs-risk-assessment-model\risk12.png)

 3. 執行**SN**命令搭配 **-T**參數和檔案的位置 (在我的案例`SN -T “C:\extensions\ThreatDetectionModule.dll”`)![模型](media\ad-fs-risk-assessment-model\risk13.png)</br>
 此命令會提供您公開金鑰語彙基元 (如我**公開金鑰 Token 是 714697626ef96b35**)

 4. 新增 dll **Global Assembly Cache**我們的最佳做法的 AD FS 伺服器是您為您的專案中建立適當的安裝程式，並使用安裝程式將檔案加入至 GAC。 另一個解決方案是使用**Gacutil.exe** (更多有關**Gacutil.exe**可用[這裡](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) 的開發電腦上。  由於我有我的 visual studio 在與 AD FS 相同的伺服器上時，我將使用**Gacutil.exe** ，如下所示

   a.   在 Visual Studio 並前往目錄包含 Developer Command Prompt **Gacutil.exe** (在這個案例中的目錄是**C:\Program Files (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.   執行**Gacutil**命令 (在我的案例`Gacutil /IF C:\extensions\ThreatDetectionModule.dll`)![模型](media\ad-fs-risk-assessment-model\risk14.png)
 
 >[!NOTE]
 >如果您有 AD FS 伺服器陣列，上述需要伺服器陣列中每個 AD FS 伺服器上執行。 

 5. 開啟**Windows PowerShell**並執行下列命令來註冊 dll
    ```
    Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
    ```
    在我的案例中，命令是： 
    ```
    Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
    ```
 
    >[!NOTE]
    >您需要將 dll 註冊一次，即使您有 AD FS 伺服器陣列。 

 6. 註冊 dll 之後重新啟動 AD FS 服務

就這麼簡單，dll 現在已向 AD FS 和準備就緒可供使用 ！

 >[!NOTE]
 > 如果外掛程式進行任何變更，就會重建專案，更新的 dll 必須再次登錄。 在之前註冊，您必須取消註冊目前的 dll，使用下列命令：</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> 在我的案例中，命令是：
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>測試外掛程式

 1. 開啟**authconfig.csv**我們稍早建立的檔案 (在這個案例中在位置**C:\extensions**)，並新增**外部網路的 Ip**您想要封鎖。 每個 IP 應該位於單獨的一行，並不應該有任何空格結尾</br>
 ![model](media\ad-fs-risk-assessment-model\risk18.png)
 
 2. 儲存並關閉檔案

 3. 執行下列 PowerShell 命令以匯入 AD FS 中更新的檔案 

  ```
  Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
  ```
 
  在我的案例中，命令是： 
  ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
 ```
 
 4. 起始驗證要求中具有相同的 IP，您在中新增的伺服器**authconfig.csv**。

 此示範中，我將使用[AD FS 說明 Claims X-Ray 工具](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)以起始要求。 如果您想要使用 x 光工具，請遵循的指示 

 輸入同盟伺服器執行個體，然後按**測試驗證** 按鈕。</br> 
 ![模型](media\ad-fs-risk-assessment-model\risk15.png) 

 5. 驗證會遭到封鎖，如下所示。</br>
 ![model](media\ad-fs-risk-assessment-model\risk16.png)
 
既然我們已經知道如何建置和登錄該外掛程式，讓我們逐步解說外掛程式的程式碼，以了解使用新的介面和類別的實作引進的模型。 

## <a name="plug-in-code-walkthrough"></a>外掛程式的程式碼逐步解說

開啟專案`ThreatDetectionModule.sln`使用 Visual Studio，然後開啟 [主檔案**UserRiskAnalyzer.cs**從**方案總管] 中**螢幕的右邊</br>
![model](media\ad-fs-risk-assessment-model\risk17.png)
 
檔案包含的主要類別會實作抽象類別 UserRiskAnalyzer [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)和介面[IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019)讀取要求的 IP內容中，比較與 AD FS DB，從載入的 Ip 取得的 IP，並封鎖要求，如果 IP 相符項目。 我們來看一下更多詳細資料中的這些型別

### <a name="threatdetectionmodule-abstract-class"></a>ThreatDetectionModule 抽象類別

此抽象類別載入外掛程式讓您可以執行外掛程式程式碼與 AD FS 的程序的 AD FS 管線。 

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
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Void|外掛程式載入它的管線時，由 AD FS 呼叫| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Void|呼叫此外掛程式時卸載其管線中的 AD FS| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Void|呼叫在更新設定的 AD FS |
|**屬性** |**型別** |**定義**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|字串 |取得擁有此外掛程式的廠商名稱|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|字串 |取得此外掛程式的識別項|

在我們的範例外掛程式，我們會使用[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019)並[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)方法，以從 AD FS 資料庫中讀取的預先定義的 Ip。 [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019)時外掛程式登錄與 AD FS 時，會呼叫[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)稱為時使用 匯入.csv `Import-AdfsThreatDetectionModuleConfiguration` cmdlet。 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>IRequestReceivedThreatDetectionModule Interface

這[介面](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019)可讓您在 AD FS 接收驗證要求，但在使用者輸入的認證也就是在驗證程序的階段中收到的要求之前實作風險評估的位置。 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

介面包含[EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) requestContext 撰寫您的風險評估邏輯輸入參數中傳遞的方法可以讓您使用的驗證要求的內容。 RequestContext 參數的類型是[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)。 

傳遞的其他輸入的參數是它是型別記錄器[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)。 參數可用來撰寫錯誤，稽核和/或偵錯 AD FS 記錄檔訊息。 

此方法會傳回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 NotEvaluated，區塊中，以 1 和 2 來允許) 對 AD FS 再封鎖或允許要求。

在我們的範例外掛程式[EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019)方法實作會剖析[clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses)從[requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)參數並比較它與所有 Ip從 AD FS 資料庫中載入。 如果找到相符項目，則方法會傳回 2**區塊**，否則會傳回 1**允許**。 根據傳回的值，AD FS 會封鎖或允許要求。 

>[!NOTE]
>此範例外掛程式實作唯一 IRequestReceivedThreatDetectionModule 介面上面所討論。 不過，風險評估模型會提供其他兩個介面 （若要實作風險評定邏輯期間預先驗證階段） – IPreAuthenticationThreatDetectionModule 和 IPostAuthenticationThreatDetectionModule （若要實作風險評估邏輯後續驗證階段期間）。 以下提供兩個介面的詳細資料。 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>IPreAuthenticationThreatDetectionModule 介面 

這[介面](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019)可讓您在使用者提供認證，但在 AD FS 會評估它們，也就是預先驗證階段之前實作風險評定邏輯之處。 

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
介面包含[EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019)傳入方法可讓您使用的資訊[RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext securityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)， [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)，以及[IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)輸入參數，將您的預先驗證的風險評估邏輯。 

>[!NOTE]
>內容傳遞與每個內容類型的清單，請瀏覽[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)，並[ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)類別定義。 

傳遞的其他輸入的參數是它是型別記錄器[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)。 參數可用來撰寫錯誤，稽核和/或偵錯 AD FS 記錄檔訊息。

此方法會傳回[ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 NotEvaluated，區塊中，以 1 和 2 來允許) 對 AD FS 再封鎖或允許要求。 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>IPostAuthenticationThreatDetectionModule 介面

這[介面](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019)可讓您實作風險評定邏輯之後使用者已提供認證，且 AD FS 已經有執行驗證也就是後續驗證階段。 

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

介面包含[EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019)傳入方法可讓您使用的資訊[RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext securityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)， [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)，以及[IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2)輸入參數，將您的後續驗證風險評估邏輯。 

>[!NOTE]
> 與每個內容型別傳遞內容的完整清單，請參閱[RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019)， [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)，並[ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)類別定義。 

傳遞的其他輸入的參數是它是型別記錄器[ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019)。 參數可用來撰寫錯誤，稽核和/或偵錯 AD FS 記錄檔訊息。 

此方法會傳回[風險分數](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019)這可以用於 AD FS 原則和宣告規則。 

>[!NOTE]
>外掛程式運作，（在此情況下 UserRiskAnalyzer） 的主要類別必須衍生[ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019)抽象類別，且應實作至少一個以上所述的三個介面。 一旦註冊 dll，AD FS 會檢查實作這些介面，並在管線中的適當階段呼叫它們。

### <a name="faqs"></a>常見問題集

**為何要建置這些外掛程式？**</br>
**答：** 這些外掛程式不只提供另一項功能來保護環境免於遭受攻擊，例如密碼噴灑攻擊，但也讓您彈性地建置您自己的風險評估邏輯，根據您的需求。 

**其中會擷取記錄檔？**</br>
**答：** 您可以寫入使用 WriteAdminLogErrorMessage 方法的 「 AD FS/系統管理員 」 事件記錄檔中的錯誤記錄檔，以"AD FS 稽核"安全性的稽核記錄檔記錄使用 WriteAuditMessage 方法，以及偵錯記錄檔，以"AD FS 追蹤 「 偵錯記錄檔使用 WriteDebugMessage 方法。 

**可以新增這些外掛程式會增加 AD FS 驗證處理序延遲嗎？**</br>
**答：** 延遲的影響取決於執行您所實作的風險評估邏輯所花費的時間。 建議您評估部署外掛程式在生產環境之前的延遲影響。 

**AD FS 為什麼不能建議使用者等等的具風險 Ip 的清單？**</br>
**答：** 目前無法使用，但我們正努力建置建議使用者等。 在隨插即用的風險評估模型的具風險 Ip 的智慧。 我們不會共用上市日期推出。 
