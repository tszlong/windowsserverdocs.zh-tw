---
title: Nano 伺服器上的 IIS
description: 在 Nano Server 上設定 IIS 的詳細資料
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.topic: article
ms.date: 09/06/2017
ms.assetid: 16984724-2d77-4d7b-9738-3dff375ed68c
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: d2522dc94c0d3b68c75e14fec19466529256aad0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826861"
---
# <a name="iis-on-nano-server"></a>Nano 伺服器上的 IIS

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 1709 版開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 

您可以使用 -Package 參數搭配 Microsoft-NanoServer-IIS-Package，在 Nano Server 上安裝 Internet Information Services (IIS) 伺服器角色。 如需設定 Nano Server (包括安裝套件) 的資訊，請參閱[安裝 Nano Server](Getting-Started-with-Nano-Server.md)。  

在此版本的 Nano Server 中，可以使用下列 IIS 功能：  

|功能|預設已啟用|  
|-----------|----------------------|  
|**一般 HTTP 功能**||  
|預設文件|x|  
|瀏覽目錄|x|  
|HTTP 錯誤|x|  
|靜態內容|x|  
|HTTP 重新導向||  
|**狀況及診斷**||  
|HTTP 記錄|x|  
|自訂記錄||  
|要求監視器||  
|追蹤||  
|**效能**||  
|靜態內容壓縮|x|  
|動態內容壓縮||  
|**安全性**||  
|要求篩選|x|  
|基本驗證||  
|用戶端憑證對應驗證||  
|摘要式驗證||  
|IIS 用戶端憑證對應驗證||  
|IP 及網域限制||  
|URL 授權||  
|Windows 驗證||  
|**應用程式開發**||  
|應用程式初始化||  
|CGI||  
|ISAPI 擴充程式||  
|ISAPI 篩選器||  
|伺服器端包含||  
|WebSocket 通訊協定||  
|**管理工具**||  
|適用於 Windows PowerShell 的 IIS 系統管理模組|x|  

[http://iis.net/learn](https://iis.net/learn) 已發佈其他 IIS 設定 (例如使用 ASP.NET、PHP 和 Java) 的系列文章，以及其他相關內容。  

## <a name="installing-iis-on-nano-server"></a>在 Nano Server 上安裝 IIS  
您可以離線 (Nano Server 已關閉時) 或線上 (Nano Server 正在執行時) 安裝此伺服器角色；離線安裝是建議選項。  

若要離線安裝，請使用 New-NanoServerImage 的 -Packages 參數新增套件，如下列範例所示：  

`New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1.vhd -ComputerName Nano1 -Package Microsoft-NanoServer-IIS-Package`  

如果您有現成的 VHD 檔案，您可以使用 DISM.exe 離線安裝 IIS，方法是掛接 VHD，然後使用 **Add-Package** 選項。   
下列範例步驟假設您正從 BasePath 選項所指定的目錄執行，此目錄是在執行 New-NanoServerImage 之後所建立。  

1.  mkdir mountdir
2.  .\Tools\dism.exe /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir
3.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\Microsoft-NanoServer-IIS-Package.cab /Image:.\mountdir
4.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab /Image:.\mountdir
5.  .\Tools\dism.exe /Unmount-Image /MountDir:.\MountDir /Commit


> [!NOTE]  
> 請注意，步驟 4 會新增語言套件 - 此範例會安裝 EN-US。  

此時您可以使用 IIS 來啟動 Nano Server。  

### <a name="installing-iis-on-nano-server-online"></a>在 Nano Server 上線上安裝 IIS  
雖然建議離線安裝伺服器角色，但在容器案例中，您可能需要在線上 (Nano Server 正在執行時) 進行安裝。 若要這樣做，請執行下列步驟：  

1.  將 Packages 資料夾從安裝媒體複製到執行中的本機 Nano Server (例如複製到 C:\packages)。  

2.  在另一部電腦上建立新的 Unattend.xml 檔案，然後將它複製到 Nano Server。 您可以將此 XML 內容複製並貼到您建立的 XML 檔案：  



```  

    <unattend xmlns=urn:schemas-microsoft-com:unattend>  
    <servicing>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=neutral />  
            <source location=c:\packages\Microsoft-NanoServer-IIS-Package.cab />  
        </package>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=en-US />  
            <source location=c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source= xmlns:cpi=urn:schemas-microsoft-com:cpi />  
</unattend>  
```  




3. 在您建立 (或複製) 的新 XML 檔案中，將 C:\packages 修改為您複製套件內容的目的地目錄。  

4. 切換至具有新建立之 XML 檔案的目錄，然後執行  

   **dism /online /apply-unattend:.\unattend.xml**  


5. 執行下列命令，確認已正確安裝 IIS 套件及其關聯的語言套件：  

   **dism /online /get-packages**  

   您應該會看到 Package Identity :Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~~10.0.14393.1000 列出兩次，一次針對 Release Type :Language Pack，另一次針對 Release Type :Feature Pack。  

6. 使用 **net start w3svc** 或藉由重新啟動 Nano Server 來啟動 W3SVC 服務。  

## <a name="starting-iis"></a>啟動 IIS  
安裝並執行 IIS 之後，即準備好處理 Web 要求。 瀏覽位於 http://\<Nano Server 的 IP 位址> 的預設 IIS 網頁，以確認 IIS 是否正在執行。 在實體電腦上，您可以使用修復主控台來判斷 IP 位址。 在虛擬機器上，您可以使用 Windows PowerShell 命令提示字元並執行下列命令來取得 IP 位址：  

`Get-VM -name <VM name> | Select -ExpandProperty networkadapters | select IPAddresses`  

如果您無法存取預設 IIS 網頁，請找到 Nano Server 上的 **c:\inetpub** 目錄以再次檢查 IIS 安裝。  

## <a name="enabling-and-disabling-iis-features"></a>啟用和停用 IIS 功能  
當您安裝 IIS 角色時，預設會啟用一些 IIS 功能 (請參閱本主題之＜Nano Server 上的 IIS 概觀＞中的表格)。 您可以使用 DISM.exe 啟用 (或停用) 其他功能

IIS 的每項功能會以一組設定元素的形式來提供。 例如，Windows 驗證功能包含下列元素：  

|區段|設定元素|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name=WindowsAuthenticationModule image=%windir%\System32\inetsrv\authsspi.dll`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true \/>`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled=false authPersistNonNTLM\=true><providers><add value=Negotiate /><add value=NTLM /><br /></providers><br /></windowsAuthentication>`|  

完整的 IIS 子功能集包含在本主題的＜附錄 1＞中，而其對應的設定元素則包含在本主題的＜附錄 2＞中。  


### <a name="example-installing-windows-authentication"></a>範例︰安裝 Windows 驗證  

1.  在 Nano Server 上開啟 Windows PowerShell 遠端工作階段主控台。  

2.  使用 `DISM.exe` 安裝 Windows 驗證模組：

    ```
    dism /Enable-Feature /online /featurename:IIS-WindowsAuthentication /all
    ```

    `/all` 參數將會安裝所選功能相依的任何功能。

### <a name="example-uninstalling-windows-authentication"></a>範例︰解除安裝 Windows 驗證  

1.  在 Nano Server 上開啟 Windows PowerShell 遠端工作階段主控台。  

2.  使用 `DISM.exe` 解除安裝 Windows 驗證模組：

    ```
    dism /Disable-Feature /online /featurename:IIS-WindowsAuthentication
    ```

## <a name="other-common-iis-configuration-tasks"></a>其他常見的 IIS 設定工作  
**建立網站**  

使用下列 Cmdlet：  

`PS D:\> New-IISSite -Name TestSite -BindingInformation *:80:TestSite -PhysicalPath c:\test`  

您接著可以執行 `Get-IISSite` 來確認網站的狀態 (傳回網站名稱、識別碼、狀態、實體路徑和繫結)。  

**刪除網站**  

執行 `Remove-IISSite -Name TestSite -Confirm:$false`。  

**建立虛擬目錄**  

您可以使用 Get-IISServerManager 傳回的 IISServerManager 物件來建立虛擬目錄，這會公開 .NET Microsoft.Web.Administration.ServerManager API。 在此範例中，這些命令會存取 Sites 集合的預設網站元素及 Applications 區段的根應用程式元素 (/)。 接著會呼叫該應用程式元素之 VirtualDirectories 集合的 Add() 方法，來建立新的目錄：  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.Sites[Default Web Site].Applications[/].VirtualDirectories.Add(/DemoVirtualDir1, c:\test\virtualDirectory1)  
PS C:\> $sm.Sites[Default Web Site].Applications[/].VirtualDirectories.Add(/DemoVirtualDir2, c:\test\virtualDirectory2)  
PS C:\> $sm.CommitChanges()  
```  

**建立應用程式集區**  

同樣地，您可以使用 Get-IISServerManager 建立應用程式集區：  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.ApplicationPools.Add(DemoAppPool)  
```  

**設定 HTTPS 和憑證**  

您可以如下列範例所示使用 Certoc.exe 公用程式來匯入憑證，此範例示範如何在 Nano Server 上設定網站的 HTTPS：  

1.  在另一部未執行 Nano Server 的電腦上，建立憑證 (使用您自己的憑證名稱和密碼)，然後將它匯出至 c:\temp\test.pfx。  

    `$newCert = New-SelfSignedCertificate -DnsName www.foo.bar.com -CertStoreLocation cert:\LocalMachine\my`  

    `$mypwd = ConvertTo-SecureString -String YOUR_PFX_PASSWD -Force -AsPlainText`  

    `Export-PfxCertificate -FilePath c:\temp\test.pfx -Cert $newCert -Password $mypwd`  

2.  將 test.pfx 檔案複製到 Nano Server 電腦。  

3.  在 Nano Server 上，使用下列命令，將憑證匯入 My 存放區：  

    **certoc.exe -ImportPFX -p YOUR_PFX_PASSWD My c:\temp\test.pfx**  

4.  使用 `Get-ChildItem Cert:\LocalMachine\my` 擷取此新憑證的指紋 (在此範例中為 61E71251294B2A7BB8259C2AC5CF7BA622777E73)。  

5.  使用下列 Windows PowerShell 命令，將 HTTPS 繫結新增至預設網站 (或您要新增繫結的任何網站)：  

    ```  
    $certificate = get-item Cert:\LocalMachine\my\61E71251294B2A7BB8259C2AC5CF7BA622777E73  
    # Use your actual thumbprint instead of this example  
    $hash = $certificate.GetCertHash()  

    Import-Module IISAdministration  
    $sm = Get-IISServerManager  
    $sm.Sites[Default Web Site].Bindings.Add(*:443:, $hash, My, 0)    # My is the certificate store name  
    $sm.CommitChanges()  
    ```  

    您也可以使用伺服器名稱指示 (SNI) 與特定的主機名稱，並搭配下列語法：`$sm.Sites[Default Web Site].Bindings.Add(*:443: www.foo.bar.com, $hash, My, Sni.`  

## <a name="appendix-1-list-of-iis-sub-features"></a>附錄 1：IIS 子功能清單

- IIS-WebServer
- IIS-CommonHttpFeatures
- IIS-StaticContent
- IIS-DefaultDocument
- IIS-DirectoryBrowsing
- IIS-HttpErrors
- IIS-HttpRedirect
- IIS-ApplicationDevelopment
- IIS-CGI
- IIS-ISAPIExtensions
- IIS-ISAPIFilter
- IIS-ServerSideIncludes
- IIS-WebSockets
- IIS-ApplicationInit
- IIS-Security
- IIS-BasicAuthentication
- IIS-WindowsAuthentication
- IIS-DigestAuthentication
- IIS-ClientCertificateMappingAuthentication
- IIS-IISCertificateMappingAuthentication
- IIS-URLAuthorization
- IIS-RequestFiltering
- IIS-IPSecurity
- IIS-CertProvider
- IIS-Performance
- IIS-HttpCompressionStatic
- IIS-HttpCompressionDynamic
- IIS-HealthAndDiagnostics
- IIS-HttpLogging
- IIS-LoggingLibraries
- IIS-RequestMonitor
- IIS-HttpTracing
- IIS-CustomLogging

## <a name="appendix-2-elements-of-http-features"></a>附錄 2：HTTP 功能的項目  
IIS 的每項功能會以一組設定元素的形式來提供。 本附錄列出此版 Nano Server 中所有功能的設定元素  

### <a name="common-http-features"></a>一般 HTTP 功能  
**預設文件**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=DefaultDocumentModule image=%windir%\System32\inetsrv\defdoc.dll />`|  
|`<modules>`|`<add name=DefaultDocumentModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=DefaultDocumentModule resourceType=EiSecther requireAccess=Read />`|  
|`<defaultDocument>`|`<defaultDocument enabled=true><br /><files><br /> <add value=Default.htm /><br />        <add value=Default.asp /><br />        <add value=index.htm /><br />        <add value=index.html /><br />        <add value=iisstart.htm /><br />    </files><br /></defaultDocument>`|  

`StaticFile <handlers>` 項目可能已經存在；如果是的話，只要將 DefaultDocumentModule 新增至 \<modules> 屬性並以逗號分隔即可。  

**瀏覽目錄**  

|區段|設定元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=DirectoryListingModule image=%windir%\System32\inetsrv\dirlist.dll />`|  
|`<modules>`|`<add name=DirectoryListingModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=DirectoryListingModule resourceType=Either requireAccess=Read />`|  

`StaticFile <handlers>` 項目可能已經存在；如果是的話，只要將 DirectoryListingModule 新增至 \<modules> 屬性並以逗號分隔即可。  

**HTTP 錯誤**  

|區段|設定元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=CustomErrorModule image=%windir%\System32\inetsrv\custerr.dll />`|  
|`<modules>`|`<add name=CustomErrorModule lockItem=true />`|  
|`<httpErrors>`|`<httpErrors lockAttributes=allowAbsolutePathsWhenDelegated,defaultPath><br />    <error statusCode=401    prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=401.htm ><br />    <error statusCode=403 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=403.htm /><br />    <error statusCode=404 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=404.htm /><br />    <error statusCode=405 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=405.htm /><br />    <error statusCode=406 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=406.htm /><br />    <error statusCode=412 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=412.htm /><br />    <error statusCode=500 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=500.htm /><br />    <error statusCode=501 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=501.htm /><br />    <error statusCode=502 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=502.htm /><br /></httpErrors>`|  

**靜態內容**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=StaticFileModule image=%windir%\System32\inetsrv\static.dll />`|  
|`<modules>`|`<add name=StaticFileModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=StaticFileModule resourceType=Either requireAccess=Read />`|  

`StaticFile \<handlers>` 項目可能已經存在；如果是的話，只要將 StaticFileModule 新增至 \<modules> 屬性並以逗號分隔即可。  

**HTTP 重新導向**  

|區段|設定元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=HttpRedirectionModule image=%windir%\System32\inetsrv\redirect.dll />`|  
|`<modules>`|`<add name=HttpRedirectionModule lockItem=true />`|  
|`<httpRedirect>`|`<httpRedirect enabled=false />`|  

### <a name="health-and-diagnostics"></a>狀況及診斷  
**HTTP 記錄**  

|區段|設定元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=HttpLoggingModule image=%windir%\System32\inetsrv\loghttp.dll />`|  
|`<modules>`|`<add name=HttpLoggingModule lockItem=true />`|  
|`<httpLogging>`|`<httpLogging dontLog=false />`|  

**自訂記錄**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CustomLoggingModule image=%windir%\System32\inetsrv\logcust.dll />`|  
|`<modules>`|`<add name=CustomLoggingModule lockItem=true />`|  

**要求監視器**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=RequestMonitorModule image=%windir%\System32\inetsrv\iisreqs.dll />`|  

**追蹤**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=TracingModule image=%windir%\System32\inetsrv\iisetw.dll \/><br /><add name=FailedRequestsTracingModule image=%windir%\System32\inetsrv\iisfreb.dll />`|  
|`<modules>`|`<add name=FailedRequestsTracingModule lockItem=true />`|  
|`<traceProviderDefinitions>`|`<traceProviderDefinitions><br />    <add name=WWW Server guid\={3a2a4e84-4c21-4981-ae10-3fda0d9b0f83}><br />        <areas><br />            <clear /><br />            <add name=Authentication value=2 /><br />            <add name=Security value=4 /><br />            <add name=Filter value=8 /><br />            <add name=StaticFile value=16 /><br />            <add name=CGI value=32 /><br />            <add name=Compression value=64 /><br />            <add name=Cache value=128 /><br />            <add name=RequestNotifications value=256 /><br />            <add name=Module value=512 /><br />            <add name=FastCGI value=4096 /><br />            <add name=WebSocket value=16384 /><br />        </areas><br />    </add><br />    <add name=ISAPI Extension guid={a1c2040e-8840-4c31-ba11-9871031a19ea}><br />        <areas><br />            <clear /><br />        </areas><br />    </add><br /></traceProviderDefinitions>`|  

### <a name="performance"></a>效能  
**靜態內容壓縮**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=StaticCompressionModule image=%windir%\System32\inetsrv\compstat.dll />`|  
|`<modules>`|`<add name=StaticCompressionModule lockItem=true />`|  
|`<httpCompression>`|`<httpCompression directory=%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files><br />    <scheme name=gzip dll=%Windir%\system32\inetsrv\gzip.dll /><br />   <staticTypes><br />        <add mimeType=text/* enabled=true /><br />        <add mimeType=message/* enabled=true /><br />        <add mimeType=application/javascript enabled=true \/><br />        <add mimeType=application/atom+xml enabled=true /><br />        <add mimeType=application/xaml+xml enabled=true /><br />        <add mimeType=\*\* enabled=false /><br />    </staticTypes><br /></httpCompression>`|  

**動態內容壓縮**  

|區段|設定元素|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name=DynamicCompressionModule image=%windir%\System32\inetsrv\compdyn.dll />`|  
|`<modules>`|`<add name=DynamicCompressionModule lockItem=true />`|  
|`<httpCompression>`|`<httpCompression directory\=%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files><br />    <scheme name=gzip dll=%Windir%\system32\inetsrv\gzip.dll \/><br />    \<dynamicTypes><br />        <add mimeType=text/* enabled=true \/><br />        <add mimeType=message/* enabled=true /><br />        <add mimeType=application/x-javascript enabled=true /><br />        <add mimeType=application/javascript enabled=true /><br />        <add mimeType=*/* enabled=false /><br />    <\/dynamicTypes><br /></httpCompression>`|  

### <a name="security"></a>安全性  
**要求篩選**  


|       區段        |                                                                                                                                        設定元素                                                                                                                                        |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `<globalModules>`   |                                                                                                        `<add name=RequestFilteringModule image=%windir%\System32\inetsrv\modrqflt.dll />`                                                                                                        |
|     `<modules>`      |                                                                                                                       `<add name=RequestFilteringModule lockItem=true />`                                                                                                                        |
| \`<requestFiltering> | `<requestFiltering><br />    <fileExtensions allowUnlisted=true applyToWebDAV=true /><br />    <verbs allowUnlisted=true applyToWebDAV=true /><br />    <hiddenSegments applyToWebDAV=true><br />        <add segment=web.config /><br />    </hiddenSegments><br /></requestFiltering>` |

**基本驗證**  

|區段|設定元素|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=BasicAuthenticationModule image=%windir%\System32\inetsrv\authbas.dll />`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true />`|  
|`<basicAuthentication>`|`<basicAuthentication enabled=false />`|  

**用戶端憑證對應驗證**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CertificateMappingAuthentication image=%windir%\System32\inetsrv\authcert.dll />`|  
|`<modules>`|`<add name=CertificateMappingAuthenticationModule lockItem=true />`|  
|`<clientCertificateMappingAuthentication>`|`<clientCertificateMappingAuthentication enabled=false />`|  

**摘要式驗證**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=DigestAuthenticationModule image=%windir%\System32\inetsrv\authmd5.dll />`|  
|`<modules>`|`<add name=DigestAuthenticationModule lockItem=true />`|  
|`<other>`|`<digestAuthentication enabled=false />`|  

**IIS 用戶端憑證對應驗證**  


|                  區段                   |                                         設定元素                                         |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------|
|             `<globalModules>`              | `<add name=CertificateMappingAuthenticationModule image=%windir%\System32\inetsrv\authcert.dll />` |
|                `<modules>`                 |               `<add name=CertificateMappingAuthenticationModule lockItem=true `/>\`                |
| `<clientCertificateMappingAuthentication>` |                      `<clientCertificateMappingAuthentication enabled=false />`                      |

**IP 及網域限制**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|```<add name=IpRestrictionModule image=%windir%\System32\inetsrv\iprestr.dll /><br /><add name=DynamicIpRestrictionModule image=%windir%\System32\inetsrv\diprestr.dll />```|  
|`<modules>`|`<add name=IpRestrictionModule lockItem=true \/><br /><add name=DynamicIpRestrictionModule lockItem=true \/>`|  
|`<ipSecurity>`|`<ipSecurity allowUnlisted=true />`|  

**URL 授權**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=UrlAuthorizationModule image=%windir%\System32\inetsrv\urlauthz.dll />`|  
|`<modules>`|`<add name=UrlAuthorizationModule lockItem=true />`|  
|`<authorization>`|`<authorization><br />    <add accessType=Allow users=* /><br /></authorization>`|  

**Windows 驗證**  

|區段|設定元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=WindowsAuthenticationModule image=%windir%\System32\inetsrv\authsspi.dll />`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true />`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled=false authPersistNonNTLM\=true><br />    <providers><br />        <add value=Negotiate /><br />        <add value=NTLM /><br />    <\providers><br /><\windowsAuthentication><windowsAuthentication enabled=false authPersistNonNTLM\=true><br />    <providers><br />        <add value=Negotiate /><br />        <add value=NTLM /><br />    <\/providers><br /><\/windowsAuthentication>`|  

### <a name="application-development"></a>應用程式開發  
**應用程式初始化**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=ApplicationInitializationModule image=%windir%\System32\inetsrv\warmup.dll />`|  
|`<modules>`|`<add name=ApplicationInitializationModule lockItem=true />`|  

**CGI**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CgiModule image=%windir%\System32\inetsrv\cgi.dll /><br /><add name=FastCgiModule image=%windir%\System32\inetsrv\iisfcgi.dll />`|  
|`<modules>`|`<add name=CgiModule lockItem=true /><br /><add name=FastCgiModule lockItem=true />`|  
|`<handlers>`|`<add name=CGI-exe path=*.exe verb=\* modules=CgiModule resourceType=File requireAccess=Execute allowPathInfo=true />`|  

**ISAPI 延伸模組**  

|區段|設定元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=IsapiModule image=%windir%\System32\inetsrv\isapi.dll />`|  
|`<modules>`|`<add name=IsapiModule lockItem=true />`|  
|`<handlers>`|`<add name=ISAPI-dll path=*.dll verb=* modules=IsapiModule resourceType=File requireAccess=Execute allowPathInfo=true />`|  

**ISAPI 篩選器**  

|區段|設定元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=IsapiFilterModule image=%windir%\System32\inetsrv\filter.dll />`|  
|`<modules>`|`<add name=IsapiFilterModule lockItem=true />`|  

**伺服器端包含**  

|區段|設定元素|  
|----------------|--------------------------|  
|`<globalModules>`|<`add name=ServerSideIncludeModule image=%windir%\System32\inetsrv\iis_ssi.dll />`|  
|`<modules>`|`<add name=ServerSideIncludeModule lockItem=true />`|  
|`<handlers>`|`<add name=SSINC-stm path=*.stm verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File \/><br /><add name=SSINC-shtm path=*.shtm verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File /><br /><add name=SSINC-shtml path=*.shtml verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File />`|  
|`<serverSideInclude>`|`<serverSideInclude ssiExecDisable=false />`|  

**WebSocket 通訊協定**  

|區段|設定元素|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=WebSocketModule image=%windir%\System32\inetsrv\iiswsock.dll />`|  
|`<modules>`|`<add name=WebSocketModule lockItem=true />`|  