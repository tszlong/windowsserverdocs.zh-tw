---
title: 搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾 - 步驟 2 AD FS 後續設定工作
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 06/06/2019
ms.assetid: 0a48852e-48cc-4047-ae58-99f11c273942
ms.openlocfilehash: 6364c3f8dc35fbafa518a106780ae6b767d4d40c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365777"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-2-ad-fs-post-configuration-work"></a>搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 2 AD FS 後續設定工作

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明使用 Active Directory 同盟服務 (AD FS) 和 Web 應用程式 Proxy 部署工作資料夾的第二個步驟。 您可以在這些主題中找到這個程序的其他步驟︰  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：總覽](deploy-work-folders-adfs-overview.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟1，設定 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟3、設定工作資料夾](deploy-work-folders-adfs-step3.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟4，設定 Web 應用程式 Proxy](deploy-work-folders-adfs-step4.md)  
  
-   [使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：步驟5，設定用戶端](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
> 本節涵蓋的指示適用于 Windows Server 2019 或 Windows Server 2016 環境。 如果您使用 Windows Server 2012 R2，請依照 [Windows Server 2012 R2 指示](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

在步驟 1 中，您可以安裝並設定 AD FS。 現在，您需要為 AD FS 執行下列後續設定步驟。  
  
## <a name="configure-dns-entries"></a>設定 DNS 項目

您必須為 AD FS 建立兩個 DNS 項目。 這些是當您建立主體別名 (SAN) 憑證時在預先安裝步驟中所使用的兩個相同項目。  
  
DNS 項目的格式如下︰  
  
-   AD FS service name.domain  
  
-   enterpriseregistration.domain  
  
-   AD FS server name.domain (DNS 項目應已經存在， 例如 2016-ADFS.contoso.com)
  
在測驗範例中，這些值為︰  
  
-   **blueadfs.contoso.com**  
  
-   **enterpriseregistration.contoso.com**  
  
## <a name="create-the-a-and-cname-records-for-ad-fs"></a>為 AD FS 建立 A 和 CNAME 記錄

若要為 AD FS 建立 A 和 CNAME 記錄，請依照下列步驟執行︰  
  
1.  在網域控制站上開啟 DNS 管理員。  
  
2.  展開 \[正向對應區域\] 資料夾，以滑鼠右鍵按一下您的網域，然後選取 **\[新增主機 (A)\]** 。  
  
3.  **\[新增主機\]** 視窗隨即開啟。 在 **\[名稱\]** 欄位中，輸入 AD FS 服務名稱的別名。 在測試範例中，此別名為 **blueadfs**。  
  
    別名必須與用於 AD FS 的憑證主體相同。 例如，如果主體是 adfs.contoso.com，則在此輸入的別名就會是 **adfs**。  
  
    > [!IMPORTANT]  
    > 當您使用 Windows Server 使用者介面 (UI)，而不是 Windows PowerShell 設定 AD FS 時，您必須為 AD FS 建立 A 記錄而不是 CNAME 記錄。 原因是透過 UI 建立的服務主體名稱 (SPN) 只包含用來設定 AD FS 服務為主機的別名。  

4.  在 **\[IP 位址\]** 中，輸入 AD FS 伺服器的 IP 位址。 在測試範例中，這是 **192.168.0.160**。 按一下 [新增主機]。  
  
5.  在 \[正向對應區域\] 資料夾中，再次以滑鼠右鍵按一下您的網域，然後選取 **\[新增別名 (CNAME)\]** 。  
  
6.  在 **\[新增資源記錄\]** 視窗中，新增別名 **enterpriseregistration** 並輸入 AD FS 伺服器的 FQDN。 此別名是用於裝置加入，而且必須呼叫 **enterpriseregistration**。
  
7.  按一下 **\[確定\]** 。  
  
若要透過 Windows PowerShell 完成相同的步驟，請使用下列命令。 命令必須在網域控制站中執行。  
  
```Powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name blueadfs -A -IPv4Address 192.168.0.160   
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name enterpriseregistration -CName  -HostNameAlias 2016-ADFS.contoso.com
```  
  
## <a name="set-up-the-ad-fs-relying-party-trust-for-work-folders"></a>設定工作資料夾的 AD FS 信賴憑證者信任

您可以設定工作資料夾的信賴憑證者信任，即使尚未設定工作資料夾。 信賴憑證者信任必須設定以啟用要使用 AD FS 的工作資料夾。 因為您已經在設定 AD FS 的程序中，現在是執行此步驟的好時機。  
  
若要設定信賴憑證者信任︰  
  
1.  開啟 **\[伺服器管理員\]** ，在 **\[工具\]** 功能表中，選取 **\[AD FS 管理\]** 。  
  
2.  在右窗格中，按一下 **\[動作\]** 下的 **\[新增信賴憑證者信任\]** 。  
  
3.  在 **\[歡迎\]** 頁面上，選取 **\[宣告感知\]** ，然後按一下 **\[啟動\]** 。  
  
4.  在 **\[選取資料來源\]** 頁面上，選取 **\[手動輸入信賴憑證者相關資料\]** ，然後按 **\[下一步\]** 。  
  
5.  在 **\[顯示名稱\]** 欄位中，輸入 **WorkFolders**，然後按 **\[下一步\]** 。  
  
6.  在 **\[設定憑證\]** 頁面上，按 **\[下一步\]** 。 權杖加密憑證是選擇性的並不需要進行測試設定。  
  
7.  在 **\[設定 URL\]** 頁面上，按 **\[下一步\]** 。  
  
8. 在 [**設定識別碼**] 頁面上，新增下列識別碼： `https://windows-server-work-folders/V1`。 此識別碼是工作資料夾所使用的硬式編碼，「工作資料夾」服務在與 AD FS 通訊時會進行傳送。 按一下 **\[下一步\]** 。  
  
9. 在 \[選擇存取控制原則\] 頁面上，選取 **\[允許所有人\]** ，然後按 **\[下一步\]** 。  
  
10. 在 [準備新增信任] 頁面上，按一下 [下一步]。  
  
11. 設定完成後，精靈中的最後一頁會指出設定成功。 選取核取方塊以編輯宣告規則，然後按一下 **\[關閉\]** 。  
  
12. 在 AD FS 嵌入式管理單元中，選取 **WorkFolders** 信賴憑證者信任，並按一下 \[動作\] 下的 **\[編輯宣告發行原則\]** 。

13. **\[編輯 WorkFolders 的宣告發行原則\]** 視窗隨即開啟。 按一下 **\[新增規則\]** 。  
  
14. 在 **\[宣告規則範本\]** 下拉式清單中，選取 **\[以宣告方式傳送 LDAP 屬性\]** ，然後按 **\[下一步\]** 。  
  
15. 在 **\[設定宣告規則\]** 頁面上的 **\[宣告規則名稱\]** 欄位中，輸入 **WorkFolders**。  
  
16. 在 **\[屬性存放區\]** 下拉式清單中，選取 **\[Active Directory\]** 。  
  
17. 在對應表格中，輸入這些值︰  
  
    -   User-Principal-Name: UPN  
  
    -   Display Name: Name  
  
    -   Surname: Surname  
  
    -   Given-Name: Given Name  
  
18. 按一下 **[完成]** 。 您會看到 \[發佈轉換規則\] 標籤上列出的 WorkFolders 規則，按一下 **\[確定\]** 。  
  
### <a name="set-relying-part-trust-options"></a>設定信賴憑證者信任選項

AD FS 的信賴憑證者信任設定好之後，您必須執行 Windows PowerShell 中的五個命令來完成設定。 這些命令集選項是工作資料夾所需，才能與 AD FS 成功通訊，無法透過 UI 設定。 這些選項包括：  
  
-   使用 JSON Web 權杖 (JWT)  
  
-   停用加密的宣告  
  
-   啟用自動更新  
  
-   設定發行 Oauth 重新整理權杖給所有裝置。  

-   授與用戶端權存取信賴憑證者信任的權限

若要設定這些選項，請使用下列命令︰  
  
```powershell  
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -EnableJWT $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -Encryptclaims $false   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -AutoupdateEnabled $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -IssueOAuthRefreshTokensTo AllDevices
Grant-AdfsApplicationPermission -ServerRoleIdentifier "https://windows-server-work-folders/V1" -AllowAllRegisteredClients -ScopeNames openid,profile  
```  
  
## <a name="enable-workplace-join"></a>啟用 Workplace Join

啟用 Workplace Join 是選擇性的，但是當您想讓使用者能夠使用個人裝置存取工作地點資源時會很實用。  
  
若要啟用 Workplace Join 的裝置註冊，您必須執行下列 Windows PowerShell 命令，它會設定裝置註冊，以及設定通用驗證原則︰  
  
```powershell  
Initialize-ADDeviceRegistration -ServiceAccountName <your AD FS service account>
    Example: Initialize-ADDeviceRegistration -ServiceAccountName contoso\adfsservice$
Set-ADFSGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true   
```  
  
## <a name="export-the-ad-fs-certificate"></a>匯出 AD FS 憑證

接下來，匯出自我簽署 AD FS 憑證，如此它才可安裝在測試環境中的下列電腦上︰  
  
-   用於工作資料夾的伺服器  
  
-   用於 Web 應用程式 Proxy 的伺服器  
  
-   加入網域的 Windows 用戶端  
  
-   非加入網域的 Windows 用戶端  
  
若要匯出憑證，請依照下列步驟執行︰  
  
1.  按一下 **\[開始\]** ，然後按一下 **\[執行\]** 。  
  
2.  輸入 **MMC**。  
  
3.  按一下 **[檔案]** 功能表上的 **[新增/移除嵌入式管理單元]** 。  
  
4.  在 **\[可用的嵌入式管理單元\]** 清單中，選取 **\[憑證\]** ，然後按一下 **\[新增\]** 。 \[憑證嵌入式管理單元精靈\] 就會啟動。  
  
5.  選取 **\[電腦帳戶\]** ，然後按 **\[下一步\]** 。  
  
6.  選取 **\[本機電腦 (執行這個主控台的電腦)\]** ，然後按一下 **\[完成\]** 。  
  
7.  按一下 **\[確定\]** 。  
  
8.  展開資料夾 **Console Root\Certificates\(Local Computer)\Personal\Certificates**。  
  
9.  以滑鼠右鍵按一下 **\[AD FS 憑證\]** ，按一下 **\[所有工作\]** ，然後按一下 **\[匯出...\]** 。  
  
10. \[憑證匯出精靈\] 隨即開啟。 選取 **\[是，匯出私密金鑰\]** 。  
  
11. 在 **\[匯出檔案格式\]** 頁面上，維持選取預設選項，然後按 **\[下一步\]** 。  
  
12. 建立憑證的密碼。 這是您在匯入憑證到其他裝置時稍後將會使用密碼。 按一下 **\[下一步\]** 。  
  
13. 輸入憑證的位置和名稱，然後按一下 **\[完成\]** 。  
  
憑證的安裝涵蓋在稍後的部署程序中。  
  
## <a name="manage-the-private-key-setting"></a>管理私密金鑰設定

您必須提供 AD FS 服務帳戶權限，才能存取新憑證的私密金鑰。 在通訊憑證過期後，您在取代該憑證時將需要再次授與此權限。 若要授與權限，請依照下列步驟執行︰  
  
1.  按一下 **\[開始\]** ，然後按一下 **\[執行\]** 。  
  
2.  輸入 **MMC**。  
  
3.  按一下 **[檔案]** 功能表上的 **[新增/移除嵌入式管理單元]** 。  
  
4.  在 **\[可用的嵌入式管理單元\]** 清單中，選取 **\[憑證\]** ，然後按一下 **\[新增\]** 。 \[憑證嵌入式管理單元精靈\] 就會啟動。  
  
5.  選取 **\[電腦帳戶\]** ，然後按 **\[下一步\]** 。  
  
6.  選取 **\[本機電腦 (執行這個主控台的電腦)\]** ，然後按一下 **\[完成\]** 。  
  
7.  按一下 **\[確定\]** 。  
  
8.  展開資料夾 **Console Root\Certificates\(Local Computer)\Personal\Certificates**。  
  
9.  以滑鼠右鍵按一下 **\[AD FS 憑證\]** ，按一下 **\[所有工作\]** ，然後按一下 **\[管理私密金鑰\]** 。  
  
10. 在 **\[權限\]** 視窗中，按一下 **\[新增\]** 。  
  
11. 在 **\[物件類型\]** 視窗中，選取 **\[服務帳戶\]** ，然後按一下 **\[確定\]** 。  
  
12. 輸入執行 AD FS 的帳戶名稱。 在測試範例中，這是 ADFSService。 按一下 **\[確定\]** 。  
  
13. 在 **\[權限\]** 視窗中，提供帳戶至少讀取的權限，然後按一下 **\[確定\]** 。  
  
如果您沒有管理私密金鑰的選項，您可能需要執行下列命令： `certutil -repairstore my *`  
  
## <a name="verify-that-ad-fs-is-operational"></a>請確認 AD FS 可操作

若要確認 AD FS 可運作，請開啟瀏覽器視窗，然後移至 `https://blueadfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml`，將 URL 變更為符合您的環境。
  
瀏覽器視窗將會顯示不含任何格式的同盟伺服器中繼資料。 您可以查看資料，而不會有任何 SSL 錯誤或警告，您的同盟伺服器是可操作的。  
  
後續步驟：[搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾︰步驟 3 設定工作資料夾](deploy-work-folders-adfs-step3.md)  
  
## <a name="see-also"></a>請參閱  
[工作資料夾總覽](Work-Folders-Overview.md)