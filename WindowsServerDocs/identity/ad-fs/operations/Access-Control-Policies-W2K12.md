---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: AD FS 中的存取控制原則
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7355ff9ed49a5e4ee8bca3a3d266a0ec1ecc0780
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814891"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Windows Server 2012 R2 和 Windows Server 2012 中的存取控制原則 AD FS


本文所述的原則會使用兩種宣告  

1.  宣告 AD FS 根據 AD FS 和 Web 應用程式 proxy 可以檢查和驗證的資訊來建立，例如直接連接到 AD FS 或 WAP 的用戶端 IP 位址。  

2.  AD FS 根據轉送至用戶端 AD FS 的資訊建立宣告，做為 HTTP 標頭  

>**重要**事項：如下所述的原則將會封鎖 Windows 10 網域加入和登入需要存取下列其他端點的案例

Windows 10 網域加入和登入所需的 AD FS 端點
- [federation service 名稱]/adfs/services/trust/2005/windowstransport
- [federation service 名稱]/adfs/services/trust/13/windowstransport
- [federation service 名稱]/adfs/services/trust/2005/usernamemixed
- [federation service 名稱]/adfs/services/trust/13/usernamemixed 
- [federation service 名稱]/adfs/services/trust/2005/certificatemixed
- [federation service 名稱]/adfs/services/trust/13/certificatemixed

>**重要**事項：/adfs/services/trust/2005/windowstransport 和/adfs/services/trust/13/windowstransport 端點應僅啟用內部網路存取，因為它們的目的是在 HTTPS 上使用 WIA 系結的內部網路面向端點。 將它們公開到外部網路，可能會允許對這些端點的要求略過鎖定保護。 這些端點應該停用在 proxy 上（也就是從外部網路停用），以保護 AD 帳戶鎖定。 

若要解決此問題，請更新根據端點宣告拒絕的任何原則，以允許上述端點發生例外狀況。

例如，下列規則：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

會更新為：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  此類別的宣告只能用來執行商務原則，而不是用來保護您的網路存取的安全性原則。  未經授權的用戶端可能會傳送具有假資訊的標頭，以取得存取權。  

本文所述的原則應一律與其他驗證方法搭配使用，例如使用者名稱和密碼或多重要素驗證。  

## <a name="client-access-policies-scenarios"></a>用戶端存取原則案例  

|**案例**|**描述**| 
| --- | --- | 
|案例1：封鎖所有對 Office 365 的外部存取|您可以從內部公司網路上的所有用戶端存取 Office 365，但來自外部用戶端的要求會根據外部用戶端的 IP 位址而遭到拒絕。|  
|案例2：封鎖 Exchange ActiveSync 以外的所有 Office 365 外部存取|您可以從內部公司網路上的所有用戶端，以及使用 Exchange ActiveSync 的任何外部用戶端裝置（例如智慧型手機）來存取 Office 365。 所有其他外部用戶端（例如使用 Outlook 的其他）都會遭到封鎖。|  
|案例3：封鎖以瀏覽器為基礎的應用程式以外的所有 Office 365 外部存取|封鎖對 Office 365 的外部存取，但被動（以瀏覽器為基礎）的應用程式（例如 Outlook Web 存取或 SharePoint Online）除外。|  
|案例4：除了指定的 Active Directory 群組以外，封鎖所有對 Office 365 的外部存取|此案例是用來測試和驗證用戶端存取原則部署。 它只會封鎖一或多個 Active Directory 群組的成員對 Office 365 的外部存取。 它也可以用來提供僅限群組成員的外部存取權。|  

## <a name="enabling-client-access-policy"></a>啟用用戶端存取原則  
 若要在 Windows Server 2012 R2 的 AD FS 中啟用用戶端存取原則，您必須更新 Microsoft Office 365 身分識別平臺信賴憑證者信任。 選擇下列其中一個範例案例，在最符合您組織需求的**Microsoft Office 365 身分識別平臺**信賴憑證者信任上設定宣告規則。  

###  <a name="scenario-1-block-all-external-access-to-office-365"></a><a name="scenario1"></a>案例1：封鎖所有對 Office 365 的外部存取  
 此用戶端存取原則案例允許從所有內部用戶端進行存取，並根據外部用戶端的 IP 位址封鎖所有外部用戶端。 您可以使用下列程式，將正確的發佈授權規則新增至您所選擇案例的 Office 365 信賴憑證者信任。  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>建立規則以封鎖所有對 Office 365 的外部存取  

1.  從**伺服器管理員**按一下 [**工具**]，然後按一下 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD Fs\ 信任關係關聯**性] 底下，按一下 [**信賴**憑證者信任]，以滑鼠右鍵按一下 [ **Microsoft Office 365 身分識別平臺**信任]，然後按一下 [**編輯宣告規則**]。  

3.  在 [**編輯宣告規則**] 對話方塊中，選取 [**發佈授權規則**] 索引標籤，然後按一下 [**新增規則**] 以啟動 [宣告規則嚮導]。  

4.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

5.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「如果有任何 IP 宣告超出所需的範圍，拒絕」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法（將上面的值取代為 "x------------------------  
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  按一下 **[完成]** 。 確認新的規則會出現在 [預設**允許存取所有使用者**] 規則之前的 [發行授權規則] 清單中（拒絕規則的優先順序，即使它稍早出現在清單中也一樣）。  如果您沒有預設允許存取規則，您可以使用宣告規則語言在清單結尾新增一個，如下所示：  </br>

    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  若要儲存新規則，請在 [**編輯宣告規則**] 對話方塊中按一下 **[確定]** 。 產生的清單看起來應該如下所示。  

     ![發行驗證規則](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  

###  <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a><a name="scenario2"></a>案例2：封鎖 Exchange ActiveSync 以外的所有 Office 365 外部存取  
 下列範例可讓您從內部用戶端（包括 Outlook）存取所有的 Office 365 應用程式，包括 Exchange Online。 除了 Exchange ActiveSync 用戶端（例如智慧型手機）之外，它還會封鎖位於公司網路外部的用戶端存取（如用戶端 IP 位址所示）。  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>建立規則以封鎖 Exchange ActiveSync 以外的所有 Office 365 外部存取  

1.  從**伺服器管理員**按一下 [**工具**]，然後按一下 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD Fs\ 信任關係關聯**性] 底下，按一下 [**信賴**憑證者信任]，以滑鼠右鍵按一下 [ **Microsoft Office 365 身分識別平臺**信任]，然後按一下 [**編輯宣告規則**]。  

3.  在 [**編輯宣告規則**] 對話方塊中，選取 [**發佈授權規則**] 索引標籤，然後按一下 [**新增規則**] 以啟動 [宣告規則嚮導]。  

4.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

5.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「如果有任何 IP 宣告超出所需的範圍，請發出 ipoutsiderange 宣告」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法（將上面的值取代為 "x------------------------  

    `c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  按一下 **[完成]** 。 確認新的規則出現在 [**發佈授權規則**] 清單中。  

7.  接下來，在 [**編輯宣告規則**] 對話方塊的 [**發佈授權規則**] 索引標籤上，按一下 [**新增規則**] 以再次啟動 [宣告規則嚮導]。  

8.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

9. 在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「如果有超出所需範圍的 IP，而且有非 EAS x-ms-用戶端-應用程式宣告，拒絕」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法：  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. 按一下 **[完成]** 。 確認新的規則出現在 [**發佈授權規則**] 清單中。  

11. 接下來，在 [**編輯宣告規則**] 對話方塊的 [**發佈授權規則**] 索引標籤上，按一下 [**新增規則**] 以再次啟動 [宣告規則嚮導]。  

12. 在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

13. 在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「檢查應用程式宣告是否存在」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法：  

   ```  
   NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. 按一下 **[完成]** 。 確認新的規則出現在 [**發佈授權規則**] 清單中。  

15. 接下來，在 [**編輯宣告規則**] 對話方塊的 [**發佈授權規則**] 索引標籤上，按一下 [**新增規則**] 以再次啟動 [宣告規則嚮導]。  

16. 在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

17. 在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「拒絕具有 ipoutsiderange true 和應用程式失敗的使用者」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法：  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. 按一下 **[完成]** 。 確認新的規則會出現在 [發佈授權規則] 清單中的 [允許存取所有使用者的預設值] 規則之前，以及 [拒絕規則] 的優先順序（即使它稍早出現在清單中）。  </br>如果您沒有預設允許存取規則，您可以使用宣告規則語言在清單結尾新增一個，如下所示：</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. 若要儲存新規則，請在 [**編輯宣告規則**] 對話方塊中按一下 [確定]。 產生的清單看起來應該如下所示。  

    ![發行授權規則](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a><a name="scenario3"></a>案例3：封鎖以瀏覽器為基礎的應用程式以外的所有 Office 365 外部存取  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>建立規則以封鎖以瀏覽器為基礎的應用程式以外的所有 Office 365 外部存取  

1.  從**伺服器管理員**按一下 [**工具**]，然後按一下 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD Fs\ 信任關係關聯**性] 底下，按一下 [**信賴**憑證者信任]，以滑鼠右鍵按一下 [ **Microsoft Office 365 身分識別平臺**信任]，然後按一下 [**編輯宣告規則**]。  

3.  在 [**編輯宣告規則**] 對話方塊中，選取 [**發佈授權規則**] 索引標籤，然後按一下 [**新增規則**] 以啟動 [宣告規則嚮導]。  

4.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

5.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「如果有任何 IP 宣告超出所需的範圍，請發出 ipoutsiderange 宣告」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法（將上面的值取代為 "x------------------------  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  按一下 **[完成]** 。 確認新的規則出現在 [**發佈授權規則**] 清單中。  

7.  接下來，在 [**編輯宣告規則**] 對話方塊的 [**發佈授權規則**] 索引標籤上，按一下 [**新增規則**] 以再次啟動 [宣告規則嚮導]。  

8.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

9. 在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「如果有超出所需範圍的 IP，而且端點不是/adfs/ls，deny」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法：  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. 按一下 **[完成]** 。 確認新的規則會出現在 [預設**允許存取所有使用者**] 規則之前的 [發行授權規則] 清單中（拒絕規則的優先順序，即使它稍早出現在清單中也一樣）。  </br></br> 如果您沒有預設允許存取規則，您可以使用宣告規則語言在清單結尾新增一個，如下所示：  

   `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. 若要儲存新規則，請在 [**編輯宣告規則**] 對話方塊中按一下 **[確定]** 。 產生的清單看起來應該如下所示。  

    ![發佈](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario-4-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a><a name="scenario4"></a>案例4：除了指定的 Active Directory 群組以外，封鎖所有對 Office 365 的外部存取  
 下列範例會根據 IP 位址啟用內部用戶端的存取。 它會封鎖來自公司網路外部用戶端 IP 位址的用戶端存取權，但指定的 Active Directory 群組中的人員除外。使用下列步驟，將正確的發佈授權規則新增至**Microsoft Office 365 身分識別平臺**信賴憑證者信任，並使用 [宣告規則] 嚮導：  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>建立規則以封鎖所有外部存取 Office 365，但指定的 Active Directory 群組除外  

1.  從**伺服器管理員**按一下 [**工具**]，然後按一下 [ **AD FS 管理**]。  

2.  在主控台樹的 [ **AD Fs\ 信任關係關聯**性] 底下，按一下 [**信賴**憑證者信任]，以滑鼠右鍵按一下 [ **Microsoft Office 365 身分識別平臺**信任]，然後按一下 [**編輯宣告規則**]。  

3.  在 [**編輯宣告規則**] 對話方塊中，選取 [**發佈授權規則**] 索引標籤，然後按一下 [**新增規則**] 以啟動 [宣告規則嚮導]。  

4.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

5.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「如果有任何 IP 宣告超出所需的範圍，請發出 ipoutsiderange 宣告」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法（將上面的值取代為 "x------------------------  


~~~
`c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. 按一下 **[完成]** 。 確認新的規則出現在 [**發佈授權規則**] 清單中。  

7. 接下來，在 [**編輯宣告規則**] 對話方塊的 [**發佈授權規則**] 索引標籤上，按一下 [**新增規則**] 以再次啟動 [宣告規則嚮導]。  

8. 在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

9. 在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「檢查群組 SID」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法（將 "groupsid" 取代為您所使用之 AD 群組的實際 SID）：  

    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. 按一下 **[完成]** 。 確認新的規則出現在 [**發佈授權規則**] 清單中。  

11. 接下來，在 [**編輯宣告規則**] 對話方塊的 [**發佈授權規則**] 索引標籤上，按一下 [**新增規則**] 以再次啟動 [宣告規則嚮導]。  

12. 在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。  

13. 在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，例如「拒絕具有 ipoutsiderange true 和 groupsid 失敗的使用者」。 在 [**自訂規則**] 底下，輸入或貼上下列宣告規則語言語法：  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. 按一下 **[完成]** 。 確認新的規則會出現在 [發佈授權規則] 清單中的 [允許存取所有使用者的預設值] 規則之前，以及 [拒絕規則] 的優先順序（即使它稍早出現在清單中）。  </br></br>如果您沒有預設允許存取規則，您可以使用宣告規則語言在清單結尾新增一個，如下所示：  

   `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. 若要儲存新規則，請在 [**編輯宣告規則**] 對話方塊中按一下 [確定]。 產生的清單看起來應該如下所示。  

     ![發佈](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="building-the-ip-address-range-expression"></a><a name="buildingip"></a>建立 IP 位址範圍運算式  
 X 毫秒轉送的用戶端 ip 宣告是從目前僅由 Exchange Online 設定的 HTTP 標頭填入，在將驗證要求傳遞至 AD FS 時，會填入標頭。 宣告的值可以是下列其中一項：  

> [!NOTE]
>  Exchange Online 目前僅支援 IPV4，而非 IPV6 位址。  

-   單一 IP 位址：直接連線至 Exchange Online 之用戶端的 IP 位址  

> [!NOTE]
> - 公司網路上用戶端的 IP 位址將會顯示為組織的輸出 proxy 或閘道的外部介面 IP 位址。  
>   -   透過 VPN 或 Microsoft DirectAccess （DA）連線到公司網路的用戶端，可能會顯示為內部公司用戶端，或作為外部用戶端（視 VPN 或 DA 的設定而定）。  

-   一或多個 IP 位址：當 Exchange Online 無法判斷連線用戶端的 IP 位址時，它會根據 x 轉送的標頭值來設定值，這是可包含在 HTTP 要求中的非標準標頭，而且市場上有許多用戶端、負載平衡器和 proxy 支援。  

> [!NOTE]
> 1. 多個 IP 位址，指出通過要求的每個 proxy 的用戶端 IP 位址和位址，將以逗號分隔。  
>    2. 與 Exchange Online 基礎結構相關的 IP 位址將不會列在清單中。  

### <a name="regular-expressions"></a>規則運算式  
 當您必須符合某個範圍的 IP 位址時，就必須建立正則運算式來執行比較。 在下一系列的步驟中，我們將提供如何建立這類運算式以符合下列位址範圍的範例（請注意，您必須變更這些範例以符合您的公用 IP 範圍）：  

- 192.168.1.1 –192.168.1.25  

- 10.0.0.1 –10.0.0.14  

  首先，將符合單一 IP 位址的基本模式如下： \b # # #\\. # # #\\. # # #\\. # # # \b  

  擴充此項，我們可以使用 OR 運算式來比對兩個不同的 IP 位址，如下所示： \b # # #\\. # # #\\\\. # #&#124;# _ \b # # #\\. # # #\\. # # #\\. # # # \b  

  因此，只比對兩個位址的範例（例如192.168.1.1 或10.0.0.1）會是： \b192\\. 168\\.1\\.1 \ b&#124;\b10\\.0\\. 0\\. 1 \ b  

  如此一來，您就可以輸入任意數目的位址。 需要允許的位址範圍（例如192.168.1.1 –192.168.1.25）時，比對必須以字元完成字元： \b192\\. 168\\。 1\\。（[1-9]&#124;1 [0-9]&#124;2 [0-5]） \b  

  請注意以下事項：  

- IP 位址會被視為字串，而不是數位。  

- 此規則的細分方式如下： \b192\\. 168\\. 1\\。  

- 這會比對開頭為 192.168.1. 的任何值。  

- 下列專案符合最後一個小數點之後的位址部分所需的範圍：  

  -   （[1-9] 符合以1-9 結尾的位址  

  -   &#124;1 [0-9] 符合以10-19 結尾的位址  

  -   &#124;2 [0-5]）符合以20-25 結尾的位址  

- 請注意，括弧必須正確定位，如此您才不會開始比對 IP 位址的其他部分。  

- 在與192區塊相符的情況下，我們可以為10個區塊撰寫類似的運算式： \b10\\.0\\. 0\\。（[1-9]&#124;1 [0-4]） \b  

- 並將它們放在一起，下列運算式應符合 "192.168.1.1 ~ 25" 和 "10.0.0.1 ~ 14" 的所有位址： \b192\\168\\. 1\\。（[1-9]&#124;1 [0-9]&#124;2 [0-5]） \b&#124;\b10\\.0\\. 0\\。（[1-9]&#124;1 [0-4]） \b  

### <a name="testing-the-expression"></a>測試運算式  
 Regex 運算式可能會變得相當棘手，因此強烈建議使用 RegEx 驗證工具。 如果您在網際網路上搜尋「線上 RegEx 運算式產生器」，您將會發現幾個良好的線上公用程式，可讓您針對範例資料嘗試運算式。  

 測試運算式時，請務必瞭解預期必須符合的事項。 Exchange online 系統可能會傳送多個 IP 位址，並以逗號分隔。 以上提供的運算式適用于此。 不過，在測試 RegEx 運算式時，請務必考慮這一點。 例如，您可以使用下列範例輸入來驗證上述範例：  

 192.168.1.1、192.168.1.2、192.169.1.1。 192.168.12.1、192.168.1.10、192.168.1.25、192.168.1.26、192.168.1.30、1192.168.1.20  

 10.0.0.1、10.0.0.5、10.0.0.10、10.0.1.0、10.0.1.1、110.0.0.1、10.0.0.14、10.0.0.15、10.0.0.10、10、0.0。1  

## <a name="claim-types"></a>宣告類型  
 Windows Server 2012 R2 中的 AD FS 會使用下列宣告類型來提供要求內容資訊：  

### <a name="x-ms-forwarded-client-ip"></a>X-毫秒-轉送-用戶端 IP  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 此 AD FS 宣告代表在查明使用者的 IP 位址（例如 Outlook 用戶端）提出要求時的「最佳嘗試」。 此宣告可以包含多個 IP 位址，包括轉送要求的每個 proxy 的位址。  此宣告會從 HTTP 填入。 宣告的值可以是下列其中一項：  

-   單一 IP 位址-直接連線至 Exchange Online 之用戶端的 IP 位址  

> [!NOTE]
>  公司網路上用戶端的 IP 位址將會顯示為組織的輸出 proxy 或閘道的外部介面 IP 位址。  

-   一或多個 IP 位址  

    -   如果 Exchange Online 無法判斷連線用戶端的 IP 位址，它會根據 x 轉送的標頭值來設定值，這是一個非標準標頭，可包含在 HTTP 要求中，並由市場上的許多用戶端、負載平衡器和 proxy 支援。  

    -   指出用戶端 IP 位址和每個傳遞要求之 proxy 位址的多個 IP 位址，會以逗號分隔。  

> [!NOTE]
>  與 Exchange Online 基礎結構相關的 IP 位址將不會出現在清單中。  

> [!WARNING]
>  Exchange Online 目前僅支援 IPV4 位址;它不支援 IPV6 位址。  

### <a name="x-ms-client-application"></a>X-MS-用戶端-應用程式  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 此 AD FS 宣告代表終端用戶端所使用的通訊協定，其對應鬆散于所使用的應用程式。  此宣告是從目前僅由 Exchange Online 設定的 HTTP 標頭填入，在將驗證要求傳遞至 AD FS 時，會填入標頭。 根據應用程式而定，此宣告的值將會是下列其中一項：  

-   如果是使用 Exchange Active Sync 的裝置，則值為 [Microsoft. Exchange ActiveSync]。  

-   使用 Microsoft Outlook 用戶端可能會產生下列任何值：  

    -   Microsoft. Exchange. 自動探索  

    -   OfflineAddressBook  

    -   RPCMicrosoft. Exchange. WebServices  

    -   RPCMicrosoft. Exchange. WebServices  

-   此標頭的其他可能值包括下列各項：  

    -   Microsoft. Exchange Powershell  

    -   Microsoft. Exchange SMTP  

    -   Microsoft. Exchange Pop  

    -   Microsoft。  

### <a name="x-ms-client-user-agent"></a>X-MS-用戶端-使用者-代理程式  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 此 AD FS 宣告會提供字串，以代表用戶端用來存取服務的裝置類型。 當客戶想要防止特定裝置（例如特定類型的智慧型手機）的存取權時，可以使用此方法。  此宣告的範例值包括（但不限於）以下的值。  

 以下範例為 x-ms-用戶端-應用程式的用戶端---------------------------------  

- Vortex/1。0  

- Apple-iPad1C1/812。1  

- Apple-iPhone3C1/811。2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5。1  

- SAMSUNGSPHD700/100.202  

- Android/0。3  

  這個值也可能是空的。  

### <a name="x-ms-proxy"></a>X-MS-Proxy  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 此 AD FS 宣告表示要求已通過 Web 應用程式 proxy。  此宣告會由 Web 應用程式 proxy 填入，在將驗證要求傳遞給後端同盟服務時，會填入標頭。 AD FS 然後將它轉換為宣告。  

 宣告的值是通過要求的 Web 應用程式 proxy 的 DNS 名稱。  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 宣告類型： `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 與上述的 [x-ms-proxy] 宣告類型類似，此宣告類型會指出要求是否通過 web 應用程式 proxy。 不同于 x-ms-proxy，insidecorporatenetwork 是布林值，其中 True 表示直接從公司網路內部對 federation service 的要求。  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-端點-絕對路徑（主動與被動）  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 此宣告類型可用來判斷源自「作用中」（豐富）用戶端與「被動」（以 web 瀏覽器為基礎）用戶端的要求。 這可讓來自瀏覽器型應用程式（例如 Outlook Web 存取、SharePoint Online 或 Office 365 入口網站）的外部要求允許，而源自豐富用戶端（例如 Microsoft Outlook）的要求會遭到封鎖。  

 宣告的值是收到要求的 AD FS 服務名稱。  

## <a name="see-also"></a>另請參閱  
 [AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
