---
title: 搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾 - 步驟 1 設定 AD FS
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 10/18/2018
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: 0920d091d6e8b5f3db9bf945a966fdd577918179
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365793"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟1，設定 AD FS

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明使用 Active Directory 同盟服務 (AD FS) 和 Web 應用程式 Proxy 部署工作資料夾的第一個步驟。 您可以在這些主題中找到這個程序的其他步驟︰  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：總覽 @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟2，AD FS 設定後的工作 @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟3，設定工作資料夾 @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟4，設定 Web 應用程式 Proxy @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟5：設定用戶端 @ no__t-0  
  
> [!NOTE]
>   本節涵蓋的指示適用于 Windows Server 2019 或 Windows Server 2016 環境。 如果您使用 Windows Server 2012 R2，請依照 [Windows Server 2012 R2 指示](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

若要設定 AD FS 以搭配使用工作資料夾，請使用下列程序。  
  
## <a name="pre-installment-work"></a>Pre\-installment work  
如果您打算利用這些指示將您要設定的測試環境轉換為生產，在您開始之前有兩件事您可能需要執行︰  
  
-   設定 Active Directory 系統管理員帳戶，以用來執行 AD FS 服務。
  
-   取得伺服器驗證用的安全通訊端層 (SSL) 主體別名 (SAN) 憑證。 對於測試範例，您將使用自我簽署憑證，但對於生產您應該使用公開信任憑證。  
  
依據貴公司的原則而定，取得這些項目可能需要一些時間，所以在您開始建立測試環境之前，先展開項目的請求程序可能比較有利於您的作業。  
  
您可以從許多商業憑證授權單位購買憑證。 您可以在[知識庫 931125](https://support.microsoft.com/kb/931125) 中找到 Microsoft所受信任的 CA 清單。 另一個方法是從您公司的企業 CA 取得憑證。  
  
對於測試環境，您將使用由所提供的指令碼之一建立的自我簽署憑證。  
  
> [!NOTE]  
> AD FS 不支援新一代密碼編譯 (CNG) 憑證，這表示您無法使用 Windows PowerShell Cmdlet New-SelfSignedCertificate 建立自我簽署憑證。 不過，您可以使用[搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap)部落格文章內所含的 makecert.ps1 指令碼。 這個指令碼會建立可配合 AD FS 使用的自我簽署憑證，以及提示輸入建立憑證所需的 SAN 名稱。  
  
接著，執行以下區段中所述的其他預先安裝工作。  
  
### <a name="create-an-ad-fs-self-signed-certificate"></a>建立 AD FS 自我簽署憑證  
若要建立 AD FS 自我簽署憑證，請依照下列步驟執行︰  
  
1.  下載[搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap)部落格文章中提供的指令碼，然後將檔案 makecert.ps1 複製到 AD FS 電腦。  
  
2.  以系統管理員權限開啟 Windows PowerShell 視窗。  
  
3.  將執行原則設為不受限制︰  
  
    ```powershell  
    Set-ExecutionPolicy –ExecutionPolicy Unrestricted   
    ```  
  
4.  變更至複製指令碼的目錄。  
  
5.  執行 makecert 指令碼：  
  
    ```powershell  
    .\makecert.ps1  
    ```  
  
6.  當系統提示您變更主體憑證時，請輸入主體的新值。 在此範例中，該值為 **blueadfs.contoso.com**。  
  
7.  當系統提示您輸入 SAN 名稱時，請按下 Y 鍵，然後輸入 SAN 名稱 (一次輸入一個)。  
  
    例如，輸入 **blueadfs.contoso.com** 並按 Enter 鍵，然後輸入 **2016-adfs.contoso.com** 並按 Enter 鍵，再輸入 **enterpriseregistration.contoso.com** 並按 Enter 鍵。  
  
    在輸入所有的 SAN 名稱後，在空白行上按 Enter 鍵。  
  
8.  當系統提示您將憑證安裝至受信任的根憑證授權單位存放區時，按下 Y 鍵。  
  
AD FS 憑證必須是具有下列值的 SAN 憑證︰  

-   AD FS service name.domain

-   enterpriseregistration.domain

-   AD FS server name.domain

在測驗範例中，這些值為︰  
  
-   **blueadfs.contoso.com**
  
-   **enterpriseregistration.contoso.com**
  
-   **2016-adfs.contoso.com**
  
Workplace Join 需要 enterpriseregistration SAN。  
  
### <a name="set-the-server-ip-address"></a>設定伺服器 IP 位址  
將伺服器 IP 位址變更為靜態 IP 位址。 針對測試範例，使用 IP 類別 A，這是 192.168.0.160/子網路遮罩：255.255.0.0/預設閘道：192.168.0.1/慣用 DNS：192.168.0.150 （您的網域控制站 @ no__t-0 的 IP 位址。  
  
## <a name="install-the-ad-fs-role-service"></a>安裝 AD FS 角色服務  
若要安裝，請依照下列步驟執行：  
  
1.  登入您打算安裝 AD FS 的實體或虛擬機器，開啟 **\[伺服器管理員\]** ，然後啟動 \[新增角色及功能精靈\]。  
  
2.  在 **\[伺服器角色\]** 頁面上，選取 **\[Active Directory 同盟服務\]** ，然後按 **\[下一步\]** 。  
  
3.  在 **\[Active Directory 同盟服務 (AD FS)** \] 頁面上，您會看到一則訊息表示 Web 應用程式 Proxy 角色無法安裝在與 AD FS 同一部電腦上。 按一下 [下一步]。  
  
4.  在確認頁面上，按一下 **\[安裝\]** 。  
  
若要透過 Windows PowerShell 完成相同安裝的 AD FS，請使用下列命令︰  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation –IncludeManagementTools  
```  
  
## <a name="configure-ad-fs"></a>設定 AD FS  
接下來，使用伺服器管理員或 Windows PowerShell 設定 AD FS。  
  
### <a name="configure-ad-fs-by-using-server-manager"></a>使用伺服器管理員設定 AD FS  
若要使用伺服器管理員設定 AD FS，請依照下列步驟執行︰  
  
1.  開啟伺服器管理員。  
  
2.  在 \[伺服器管理員\] 視窗上方，按一下 **\[通知\]** 旗標，然後按一下 **\[設定此伺服器上的 Federation Service\]** 。  
  
3.  \[Active Directory 同盟服務設定精靈\] 隨即啟動。 在 **\[連線到 AD DS\]** 頁面上，輸入您想要用作 AD FS 帳戶的網域管理員帳戶，然後按 **\[下一步\]** 。  
  
4.  在 **\[指定服務內容\]** 頁面上，輸入 SSL 憑證的主體名稱，以用於 AD FS 通訊。 在測驗範例中，這是 **blueadfs.contoso.com**。  
  
5.  輸入同盟服務名稱。 在測驗範例中，這是 **blueadfs.contoso.com**。 按一下 [下一步]。  
  
    > [!NOTE]  
    > 同盟服務名稱不得使用環境中現有伺服器的名稱。 如果您使用現有伺服器的名稱，AD FS 安裝就會失敗且必須重新開始。  
  
6.  在 **\[指定服務帳戶\]** 頁面上，輸入您想要用於受管理服務帳戶的名稱。 對於測試範例，選取 **\[建立群組受管理的服務帳戶\]** ，並在 **\[帳戶名稱\]** 中輸入 **ADFSService**。 按一下 [下一步]。  
  
7.  在 **\[指定設定資料庫\]** 頁面上，選取 **\[在此伺服器上使用 Windows 內部資料庫來建立資料庫\]** ，然後按 **\[下一步\]** 。  
  
8.  **\[檢閱選項\]** 頁面會顯示您所選擇的選項的概觀。 按一下 [下一步]。  
  
9. **\[先決條件檢查\]** 頁面會指出所有必要條件是否成功通過檢查。 如果沒有任何問題，請按一下 **\[設定\]** 。  
  
    > [!NOTE]  
    > 如果您用了 AD FS 伺服器或同盟服務的任何其他現有電腦的名稱，則會顯示錯誤訊息。 您必須從頭開始安裝，然後選擇現有電腦名稱以外的名稱。  
  
10. 設定成功完成時， **\[結果\]** 頁面會確認 AD FS 已成功設定。  
  
### <a name="configure-ad-fs-by-using-powershell"></a>使用 PowerShell 設定 AD FS  
若要透過 Windows PowerShell 完成 AD FS 的相等設定，請使用下列命令。  
  
若要安裝 AD FS：  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation -IncludeManagementTools   
```  
  
若要建立受管理服務帳戶︰  
  
```powershell  
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com  
```  
  
在您設定 AD FS 之後，您必須使用您在上一個步驟所建立的受管理服務帳戶，以及您在預先設定步驟中建立的憑證，設定 AD FS 陣列。  
  
若要設定 AD FS 陣列︰  
  
```powershell  
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop  
```  
  
後續步驟：@no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟2，AD FS 設定後的工作 @ no__t-0  
  
## <a name="see-also"></a>另請參閱  
[工作資料夾總覽](Work-Folders-Overview.md)  
  

