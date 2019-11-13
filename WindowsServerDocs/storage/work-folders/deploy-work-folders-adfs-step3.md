---
title: 搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾 - 步驟 3 設定工作資料夾
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 5a43b104-4d02-4d73-a385-da1cfb67e341
ms.openlocfilehash: ef76b87928e696586356c499367051ff0d0e9ab4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365766"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-3-set-up-work-folders"></a>搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 3 設定工作資料夾

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明使用 Active Directory 同盟服務 (AD FS) 和 Web 應用程式 Proxy 部署工作資料夾的第三個步驟。 您可以在這些主題中找到這個程序的其他步驟︰  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：總覽](deploy-work-folders-adfs-overview.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟1，設定 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟2，AD FS 設定後的工作](deploy-work-folders-adfs-step2.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟4，設定 Web 應用程式 Proxy](deploy-work-folders-adfs-step4.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟5，設定用戶端](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   本節涵蓋的指示適用于 Windows Server 2019 或 Windows Server 2016 環境。 如果您使用 Windows Server 2012 R2，請依照 [Windows Server 2012 R2 指示](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

若要設定工作資料夾，請使用下列程序。  
  
## <a name="pre-installment-work"></a>Pre\-installment work  
為了安裝工作資料夾，您的伺服器必須加入網域並執行 Windows Server 2016。 伺服器的網路設定必須是有效的。  
  
在測試範例中，請將要執行工作資料夾的電腦加入 Contoso 網域，並依照下列各節所述設定網路介面。 

### <a name="set-the-server-ip-address"></a>設定伺服器 IP 位址  
將伺服器 IP 位址變更為靜態 IP 位址。 在測驗範例中，請使用 IP 類別 A，也就是 192.168.0.170 / 子網路遮罩︰255.255.0.0 / 預設閘道︰192.168.0.1 / 慣用 DNS︰192.168.0.150 (網域控制站的 IP 位址)。 
  
### <a name="create-the-cname-record-for-work-folders"></a>建立工作資料夾的 CNAME 記錄  
若要建立工作資料夾的 CNAME 記錄，請依照下列步驟執行︰  
  
1.  在網域控制站上開啟 **DNS 管理員**。  
  
2.  展開 \[正向對應區域\] 資料夾，在您的網域上按滑鼠右鍵，再按一下 **\[新增別名 (CNAME)\]** 。  
  
3.  在 **\[新增資源記錄\]** 視窗的 **\[別名名稱\]** 欄位中，輸入工作資料夾的別名。 在測試範例中，此別名為 **workfolders**。  
  
4.  在 **\[完整的網域名稱\]** 欄位中，該值應為 **workfolders.contoso.com**。  
  
5.  在 **\[目標主機的完整網域名稱\]** 欄位中，輸入工作資料夾伺服器的 FQDN。 在測試範例中，該值為 **2016-WF.contoso.com**。  
  
6.  按一下 **\[確定\]** 。  
  
若要透過 Windows PowerShell 完成相同的步驟，請使用下列命令。 命令必須在網域控制站中執行。  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name workfolders -CName  -HostNameAlias 2016-wf.contoso.com   
```  
  
### <a name="install-the-ad-fs-certificate"></a>安裝 AD FS 憑證  
將 AD FS 設定期間建立的 AD FS 憑證安裝至本機電腦憑證存放區，請使用下列步驟：  
  
1.  按一下 **\[開始\]** ，然後按一下 **\[執行\]** 。  
  
2.  輸入 **MMC**。  
  
3.  按一下 **[檔案]** 功能表上的 **[新增/移除嵌入式管理單元]** 。  
  
4.  在 **\[可用的嵌入式管理單元\]** 清單中，選取 **\[憑證\]** ，然後按一下 **\[新增\]** 。 \[憑證嵌入式管理單元精靈\] 就會啟動。  
  
5.  選取 **\[電腦帳戶\]** ，然後按 **\[下一步\]** 。  
  
6.  選取 **\[本機電腦 (執行這個主控台的電腦)\]** ，然後按一下 **\[完成\]** 。  
  
7.  按一下 **\[確定\]** 。  
  
8.  展開資料夾 **Console Root\Certificates\(Local Computer)\Personal\Certificates**。  
  
9. 以滑鼠右鍵按一下 **\[憑證\]** ，按一下 **\[所有工作\]** ，然後按一下 **\[匯入\]** 。  
  
10. 瀏覽至含有 AD FS 憑證的資料夾，然後依照精靈中的指示匯入檔案，並將它放在憑證存放區。

11. 展開資料夾 **Console Root\Certificates\(Local Computer)\Trusted Root Certification Authorities\Certificates**。  
  
12. 以滑鼠右鍵按一下 **\[憑證\]** ，按一下 **\[所有工作\]** ，然後按一下 **\[匯入\]** 。  
  
13. 瀏覽至含有 AD FS 憑證的資料夾，然後依照精靈中的指示匯入檔案，並將它放在「受信任的根憑證授權單位」存放區。  
  
### <a name="create-the-work-folders-self-signed-certificate"></a>建立工作資料夾自我簽署憑證  
若要建立工作資料夾自我簽署憑證，請依照下列步驟執行︰  
  
1.  下載 [搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap) 部落格文章中提供的指令碼，然後將檔案 makecert.ps1 複製到工作資料夾電腦。  
  
2.  以系統管理員權限開啟 Windows PowerShell 視窗。  
  
3.  將執行原則設為不受限制︰  
  
    ```powershell  
    PS C:\temp\scripts> Set-ExecutionPolicy -ExecutionPolicy Unrestricted   
    ```  
  
4.  變更至複製指令碼的目錄。  
  
5.  執行 makeCert 指令碼：  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  當系統提示您變更主體憑證時，請輸入主體的新值。 在此範例中，該值為 **workfolders.contoso.com**。  
  
7.  當系統提示您輸入主體別名 (SAN) 的名稱時，請按下 Y 鍵，然後輸入 SAN 名稱 (一次輸入一個)。  
  
    例如，輸入**workfolders.contoso.com**，然後按 Enter 鍵。 然後輸入 **2016-WF.contoso.com** 並按 Enter 鍵。  
  
    在輸入所有的 SAN 名稱後，在空白行上按 Enter 鍵。  
  
8.  當系統提示您將憑證安裝至受信任的根憑證授權單位存放區時，按下 Y 鍵。  
  
工作資料夾憑證必須是具有下列值的 SAN 憑證︰  
  
-   **工作資料夾**.**網域**  
  
-   **電腦名稱**.**網域**  
  
在測驗範例中，這些值為︰  
  
-   **workfolders.contoso.com**  
  
-   **2016-WF.contoso.com**  
  
## <a name="install-work-folders"></a>安裝工作資料夾  
若要安裝工作資料夾角色，請依照下列步驟執行︰  
  
1.  開啟 **\[伺服器管理員\]** ，按一下 **\[新增角色及功能\]** ，然後按 **\[下一步\]** 。  
  
2.  在 **\[安裝類型\]** 頁面上，選取 **\[角色型或功能型安裝\]** ，然後按 **\[下一步\]** 。  
  
3.  在 **\[選取伺服器\]** 頁面上，選取您目前的伺服器，然後按 **\[下一步\]** 。  
  
4.  在 **\[伺服器角色\]** 頁面上，依序展開 **\[檔案和存放服務\]** 、 **\[檔案和 iSCSI 服務\]** ，然後選取 **\[工作資料夾\]** 。  
  
5.  在 **\[新增角色及功能精靈\]** 頁面上，按一下 **\[新增功能\]** ，然後按 **\[下一步\]** 。  
  
6.  在 **\[功能\]** 頁面上，按 **\[下一步\]** 。  
  
7.  在 [確認] 頁面上，按一下 [安裝]。  
  
## <a name="configure-work-folders"></a>設定工作資料夾  
若要設定工作資料夾，請依照下列步驟執行：  
  
1.  開啟**伺服器管理員**。  
  
2.  選取 **\[檔案和存放服務\]** ，然後選取 **\[工作資料夾\]** 。  
  
3.  在 **\[工作資料夾\]** 頁面上，啟動 **\[新增同步共用精靈\]** ，然後按 **\[下一步\]** 。  
  
4.  在 **\[伺服器和路徑\]** 頁面上，選取將建立同步共用的伺服器，輸入要儲存工作資料夾資料的本機路徑，然後按 **\[下一步\]** 。  
  
    如果路徑不存在，系統將會提示您建立它。 按一下 **\[確定\]** 。  
  
5.  在 **\[使用者資料夾結構\]** 頁面上，選取 **\[使用者別名\]** ，然後按 **\[下一步\]** 。  
  
6.  在 **\[同步共用名稱\]** 頁面上，輸入同步共用的名稱。 在測試範例中，此別名為 **WorkFolders**。 按一下 **\[下一步\]** 。  
  
7.  在 **\[同步存取\]** 頁面上，新增將具備同步共用存取權限的使用者或群組。 在測試範例中會將存取權限授予所有網域使用者。 按一下 **\[下一步\]** 。  
  
8.  在 **\[電腦安全性原則\]** 頁面上，選取 **\[加密工作資料夾\]** 和 **\[自動鎖定頁面並要求輸入密碼\]** 。 按一下 **\[下一步\]** 。  
  
9. 在 **\[確認\]** 頁面中，按一下 **\[建立\]** 以完成設定程序。  
  
## <a name="work-folders-post-configuration-work"></a>工作資料夾後設定工作  
若要完成工作資料夾設定，請執行這些額外步驟︰  
  
-   將工作資料夾憑證繫結到 SSL 連接埠。  
  
-   設定工作資料夾以使用 AD FS 驗證  
  
-   匯出工作資料夾憑證 (如果您正在使用自我簽署憑證)  
  
### <a name="bind-the-certificate"></a>繫結憑證  
工作資料夾只能透過 SSL 通訊，且必須將您稍早建立的 (或憑證授權單位發行的) 自我簽署憑證繫結至連接埠。  
  
您可以使用兩種方式以透過 Windows PowerShell 將憑證繫結至連接埠：IIS Cmdlet 和 netsh。  
  
#### <a name="bind-the-certificate-by-using-netsh"></a>使用 netsh 繫結憑證  
若要在 Windows PowerShell 中使用 netsh 命令列指令碼公用程式，您必須使用管線將命令傳送至 netsh。 以下範例指令碼會使用主體 **workfolders.contoso.com** 尋找憑證，並使用 netsh 將其繫結至連接埠 443：  
  
```powershell  
$subject = "workfolders.contoso.com"   
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
$Command = "http add sslcert ipport=0.0.0.0:443 certhash=$thumbprint appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY"  
$Command | netsh  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
#### <a name="bind-the-certificate-by-using-iis-cmdlets"></a>使用 IIS Cmdlet 繫結憑證  
您也可以使用 IIS 管理 Cmdlet 將憑證繫結至連接埠，不過您必須安裝 IIS 管理工具和指令碼才可使用該 Cmdlet。  
  
> [!NOTE]  
> 安裝 IIS 管理工具並不會在工作資料夾電腦上啟用 Internet Information Services 的完整版，它只會啟用管理 Cmdlet。 此設定還可提供一些好處， (比如說) 要是您正在尋找 Cmdlet 以提供 netsh 所發揮之功能的話。 當憑證是透過 New-WebBinding Cmdlet 繫結至連接埠時，該繫結並不會以任何方式依存於 IIS。 在進行繫結之後，即使您已移除 Web-Mgmt-Console 功能，憑證仍可繫結至連接埠。 您可以輸入 **netsh http show sslcert** 以透過 netsh 驗證繫結。  
  
以下範例使用 New-WebBinding Cmdlet 以尋找具有主體**workfolders.contoso.com** 的憑證，並將其繫結至連接埠 443：  
  
```powershell  
$subject = "workfolders.contoso.com"  
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert =Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject } | sort $_.NotAfter -Descending | select -first 1   
$thumbprint = $cert.Thumbprint  
New-WebBinding -Name "Default Web Site" -IP * -Port 443 -Protocol https  
#The default IIS website name must be used for the binding. Because Work Folders uses Hostable Web Core and its own configuration file, its website name, 'ECSsite', will not work with the cmdlet. The workaround is to use the default IIS website name, even though IIS is not enabled, because the NewWebBinding cmdlet looks for a site in the default IIS configuration file.   
Push-Location IIS:\SslBindings  
Get-Item cert:\LocalMachine\MY\$thumbprint | new-item *!443  
Pop-Location  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
### <a name="set-up-ad-fs-authentication"></a>設定 AD FS 驗證  
若要設定工作資料夾以使用 AD FS 進行驗證，請依照下列步驟執行：  
  
1.  開啟**伺服器管理員**。  
  
2.  按一下 **\[伺服器\]** ，然後在清單中選取您的工作資料夾。  
  
3.  在伺服器名稱上按滑鼠右鍵，再按一下 **\[工作資料夾設定\]** 。  
  
4.  在 **\[工作資料夾設定\]** 視窗中，選取 **\[Active Directory 同盟服務\]** ，並輸入 Federation Service URL。 按一下 [套用]。  
  
    在測試範例中，URL 為 **https://blueadfs.contoso.com** 。  
  
透過 Windows PowerShell 完成相同工作的 Cmdlet 是：  
  
```powershell  
Set-SyncServerSetting -ADFSUrl "https://blueadfs.contoso.com"   
```  
  
如果您使用自我簽署憑證設定 AD FS，可能會收到表示 Federation Service URL 不正確、無法連線或未設定信賴憑證者信任的錯誤訊息。  
  
如果 AD FS 憑證未安裝在工作資料夾伺服器上，或者未正確設定 AD FS 的 CNAME 時，也會發生此錯誤。 您必須修正這些問題才能繼續。  
  
### <a name="export-the-work-folders-certificate"></a>匯出工作資料夾憑證  
您必須先將自我簽署工作資料夾憑證匯出，才能在測試環境中將其安裝於下列電腦上：  
  
-   用於 Web 應用程式 Proxy 的伺服器  
  
-   加入網域的 Windows 用戶端  
  
-   非加入網域的 Windows 用戶端  
  
若要匯出憑證，請執行與稍早用於匯出 AD FS 憑證相同的步驟，如 [搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾：步驟 2 AD FS 後續設定工作](deploy-work-folders-adfs-step2.md) 所述，匯出 AD FS 憑證。  
  
下一個步驟：[搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾：步驟 4 設定 Web 應用程式 Proxy](deploy-work-folders-adfs-step4.md)  
  
## <a name="see-also"></a>請參閱  
[工作資料夾總覽](Work-Folders-Overview.md)  
  

