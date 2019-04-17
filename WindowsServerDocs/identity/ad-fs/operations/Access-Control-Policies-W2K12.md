---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: "AD FS 中存取控制原則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0b4549192bc4dab9edf3b2a10421325144ed0cd2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>在 Windows Server 2012 R2 和 Windows Server 2012 AD FS 存取控制原則

>適用於：Windows Server 2012 R2 和 Windows Server 2012 

這篇文章中所述的原則讓使用兩種類型的宣告  
  
1.  宣告 AD FS 建立依據 proxy AD FS 和 Web 應用程式可以檢查並確認，請直接連接到 AD FS 或 WAP client 的 IP 位址等資訊。  
  
2.  宣告 AD FS 建立根據為 HTTP 標頭轉寄給 AD FS client 的資訊  
  
>**重要**：如如下所述的原則會封鎖加入網域的 Windows 10 並登入需要存取下列其他端點案例

AD FS 端點上所需的 Windows 10 加入網域並登入
- [同盟服務名稱] / [adfs 日服務日信任日 2005 年日 windowstransport
- [同盟服務名稱] / [adfs 日服務日信任月 13 日 windowstransport
- [同盟服務名稱] / [adfs 日服務日信任日 2005 年日 usernamemixed
- [同盟服務名稱] / [adfs 日服務日信任月 13 日 usernamemixed 
- [同盟服務名稱] / [adfs 日服務日信任日 2005 年日 certificatemixed
- [同盟服務名稱] / [adfs 日服務日信任月 13 日 certificatemixed

若要解析，更新拒絕根據允許例外上述的端點的端點宣告任何原則。

例如，規則下列：

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

想要更新：

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  從這個分類宣告只能實作商務原則，而不是安全性原則，以保護您的網路的存取。  很可能用未經授權戶端傳送標頭 false 資訊的方式來存取。  
  
這篇文章中所述的原則永遠搭配另一個驗證方法，例如使用者名稱與密碼或使用多監視器因素驗證。  
  
## <a name="client-access-policies-scenarios"></a>Client 存取原則案例  
  
|**案例**|**描述**| 
| --- | --- | 
|案例 1：封鎖所有外部存取 Office 365|Office 365 存取允許從企業連絡，在所有但拒絕要求外部從依據外部 client 的 IP 位址。|  
|案例 2：封鎖所有外部存取 Exchange ActiveSync 以外的 Office 365|Office 365 存取允許從企業連絡，在所有以及從任何 client 的外部裝置，例如智慧型手機，請使用 Exchange ActiveSync。 封鎖所有其他外部戶端，例如使用 Outlook。|  
|案例 3：封鎖所有外部存取 Office 365 以外的瀏覽器為基礎的應用程式|區塊外部存取 Office 365，除了被動式（瀏覽器為基礎）應用程式 Outlook Web Access 或 SharePoint Online。|  
|案例 4：封鎖所有外部存取 Office 365 除了指定 Active Directory 群組|本案例可用於測試及驗證 client 存取原則部署。 它會封鎖外部存取 Office 365 只會針對一或多個 Active Directory 群組成員。 它還可以用於提供外部存取僅限群組成員。|  
  
## <a name="enabling-client-access-policy"></a>讓 Client 存取原則  
 若要讓 client 存取原則 AD FS 在 Windows Server 2012 R2，您必須更新 Microsoft Office 365 的身分平台廠商信任做為基礎。 選擇其中一項下列設定理賠要求規則示範案例**Microsoft Office 365 的身分平台**信賴廠商信任最符合您的組織的需求。  
  
###  <a name="scenario1"></a>案例 1：封鎖所有外部存取 Office 365  
 此 client 存取原則案例可讓您存取所有內部戶端和封鎖所有的外部戶端根據外部 client 的 IP 位址。 您可以使用下列程序新增正確的發行授權規則與 Office 365 可以廠商信任您所選擇的案例。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>若要建立封鎖所有外部存取 Office 365 規則  
  
1.  從**伺服器管理員**，按一下 [**工具**，然後按**AD FS 管理**。  
  
2.  主控台中在**AD FS\Trust 關係**，按一下 [**可以廠商信任**，以滑鼠右鍵按一下**Microsoft Office 365 的身分平台**信任]，然後按一下**編輯理賠要求規則**。  
  
3.  在**編輯理賠要求規則**對話方塊中，選取**發行授權規則**索引標籤，然後按一下 [ **[新增規則**以開始理賠要求規則精靈。  
  
4.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**，，然後按一下 [**下一步**。  
  
5.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱此規則，例如「如果您想要的範圍，以外的任何 IP 宣告拒絕」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法（取代上方的值為「x ms-轉送-client-ip」的有效的 IP 運算式）貼上：  
`c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  按一下**完成**。 確認 [新增規則預設之前發行授權規則清單中出現**允許所有使用者存取**規則（即使出現在清單中稍早拒絕規則將會優先）。  如果您不需要允許存取規則預設值，您可以新增結尾，如下所示使用理賠要求規則語言清單的一項：  </br>
    
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  在儲存新規則，**編輯理賠要求規則**對話方塊中，按**[確定]**。 結果清單應該如下所示。  
  
     ![發行驗證規則](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a>案例 2：封鎖所有外部存取 Exchange ActiveSync 以外的 Office 365  
 下列範例可讓存取所有的 Office 365 應用程式，包括 Online 換貨，從內部包含 Outlook。 它會封鎖從位於以外的公司網路的存取（如同指示）client IP 位址，除了 Exchange ActiveSync 戶端，例如智慧型手機。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>若要建立封鎖所有外部存取 Office 365 以外 Exchange ActiveSync 規則  
  
1.  從**伺服器管理員**，按一下 [**工具**，然後按**AD FS 管理**。  
  
2.  主控台中在**AD FS\Trust 關係**，按一下 [**可以廠商信任**，以滑鼠右鍵按一下**Microsoft Office 365 的身分平台**信任]，然後按一下**編輯理賠要求規則**。  
  
3.  在**編輯理賠要求規則**對話方塊中，選取**發行授權規則**索引標籤，然後按一下 [ **[新增規則**以開始理賠要求規則精靈。  
  
4.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**，，然後按一下 [**下一步**。  
  
5.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱，則本規則的範例「如果您想要的範圍，以外的任何 IP 宣告發出 ipoutsiderange 宣告」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法（取代上方的值為「x ms-轉送-client-ip」的有效的 IP 運算式）貼上：  
    
    `c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  按一下**完成**。 請確認新規則出現在**發行授權規則**清單中。  
  
7.  接著，在**編輯理賠要求規則**對話方塊中，於**發行授權規則**索引標籤上，按一下 [**新增規則**開始理賠要求規則精靈再試一次。  
  
8.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**，，然後按一下 [**下一步**。  
  
9. 在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱此規則，例如「IP 超過所需的範圍，還有非 EAS x ms-client 的應用程式理賠要求，如果拒絕」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法貼上：  
  

    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. 按一下**完成**。 請確認新規則出現在**發行授權規則**清單中。  
  
11. 接著，在**編輯理賠要求規則**對話方塊中，於**發行授權規則**索引標籤上，按一下 [**新增規則**開始理賠要求規則精靈再試一次。  
  
12. 在**選取 [規則範本**頁面上，在**理賠要求規則範本，**選取**傳送主張使用自訂規則**，，然後按一下**下一步**。  
  
13. 在**設定規則**頁面上，在**理賠要求規則名稱**，輸入此規則的顯示名稱，例如「檢查是否宣告應用程式]。 在**自訂規則**中，輸入或下列理賠要求規則語言語法貼上：  
  
    ```  
    NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. 按一下**完成**。 請確認新規則出現在**發行授權規則**清單中。  
  
15. 接著，在**編輯理賠要求規則**對話方塊中，於**發行授權規則**索引標籤上，按一下 [**新增規則**開始理賠要求規則精靈再試一次。  
  
16. 在**選取 [規則範本**頁面上，在**理賠要求規則範本，**選取**傳送主張使用自訂規則**，，然後按一下**下一步**。  
  
17. 在**設定規則**頁面上，在**理賠要求規則名稱**，輸入此規則的顯示名稱，例如「拒絕 ipoutsiderange true 與應用程式的使用者失敗」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法貼上：  
  
`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. 按一下**完成**。 確認新規則立即下方的上一個規則及前允許所有使用者存取預設規則（即使出現在清單中稍早拒絕規則將會優先）發行授權規則清單中。  </br>如果您不需要允許存取規則預設值，您可以新增結尾，如下所示使用理賠要求規則語言清單的一項：</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. 在儲存新規則，**編輯理賠要求規則**對話方塊中，按一下 [確定]。 結果清單應該如下所示。  
  
     ![發行授權規則](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a>案例 3：封鎖所有外部存取 Office 365 以外的瀏覽器為基礎的應用程式  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要建立封鎖所有外部存取 Office 365 以外的瀏覽器為基礎的應用程式規則  
  
1.  從**伺服器管理員**，按一下 [**工具**，然後按**AD FS 管理**。  
  
2.  主控台中在**AD FS\Trust 關係**，按一下 [**可以廠商信任**，以滑鼠右鍵按一下**Microsoft Office 365 的身分平台**信任]，然後按一下**編輯理賠要求規則**。  
  
3.  在**編輯理賠要求規則**對話方塊中，選取**發行授權規則**索引標籤，然後按一下 [ **[新增規則**以開始理賠要求規則精靈。  
  
4.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**，，然後按一下 [**下一步**。  
  
5.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱，則本規則的範例「如果您想要的範圍，以外的任何 IP 宣告發出 ipoutsiderange 宣告」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法（取代上方的值為「x ms-轉送-client-ip」的有效的 IP 運算式）貼上：  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  按一下**完成**。 請確認新規則出現在**發行授權規則**清單中。  
  
7.  接著，在**編輯理賠要求規則**對話方塊中，於**發行授權規則**索引標籤上，按一下 [**新增規則**開始理賠要求規則精靈再試一次。  
  
8.  在**選取 [規則範本**頁面上，在**理賠要求規則範本，**選取**傳送主張使用自訂規則**，，然後按一下**下一步**。  
  
9. 在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱此規則，例如「IP 超過所需的範圍，端點不日 adfs 日 ls，如果拒絕」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法貼上：  
  
 
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. 按一下**完成**。 確認 [新增規則預設之前發行授權規則清單中出現**允許所有使用者存取**規則（即使出現在清單中稍早拒絕規則將會優先）。  </br></br> 如果您不需要允許存取規則預設值，您可以新增結尾，如下所示使用理賠要求規則語言清單的一項：  
  
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. 在儲存新規則，**編輯理賠要求規則**對話方塊中，按**[確定]**。 結果清單應該如下所示。  
  
     ![發行](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a>案例 4：封鎖所有外部存取 Office 365 除了指定 Active Directory 群組  
 下列範例可讓存取從內部根據 IP 位址。 它會封鎖存取從位於以外的公司網路有外部 client IP 位址，除了這些中指定的 Active Directory Group.Use 的個人下列步驟來新增到正確的發行授權規則**Microsoft Office 365 的身分平台**使用理賠要求規則精靈信賴廠商信任：  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>若要建立封鎖所有外部存取 Office 365，除了規則指定 Active Directory 群組  
  
1.  從**伺服器管理員**，按一下 [**工具**，然後按**AD FS 管理**。  
  
2.  主控台中在**AD FS\Trust 關係**，按一下 [**可以廠商信任**，以滑鼠右鍵按一下**Microsoft Office 365 的身分平台**信任]，然後按一下**編輯理賠要求規則**。  
  
3.  在**編輯理賠要求規則**對話方塊中，選取**發行授權規則**索引標籤，然後按一下 [ **[新增規則**以開始理賠要求規則精靈。  
  
4.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**、選取**傳送主張使用自訂規則**，，然後按一下 [**下一步**。  
  
5.  在**設定規則**頁面上，在**理賠要求規則名稱**，輸入顯示名稱，則本規則的範例「如果您想要的範圍，以外的任何 IP 宣告發行 ipoutsiderange 理賠要求。」 在**自訂規則**中，輸入或下列理賠要求規則語言語法（取代上方的值為「x ms-轉送-client-ip」的有效的 IP 運算式）貼上：  
  
      
    `c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  按一下**完成**。 請確認新規則出現在**發行授權規則**清單中。  
  
7.  接著，在**編輯理賠要求規則**對話方塊中，於**發行授權規則**索引標籤上，按一下 [**新增規則**開始理賠要求規則精靈再試一次。  
  
8.  在**選取 [規則範本**頁面上，在**理賠要求規則範本，**選取**傳送主張使用自訂規則**，，然後按一下**下一步**。  
  
9. 在**設定規則**頁面上，在**理賠要求規則名稱**，輸入此規則的顯示名稱，例如「請群組 SID」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法 (取代」groupsid「與您正在使用的廣告群組的實際 SID) 貼上：  
   
    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. 按一下**完成**。 請確認新規則出現在**發行授權規則**清單中。  
  
11. 接著，在**編輯理賠要求規則**對話方塊中，於**發行授權規則**索引標籤上，按一下 [**新增規則**開始理賠要求規則精靈再試一次。  
  
12. 在**選取 [規則範本**頁面上，在**理賠要求規則範本，**選取**傳送主張使用自訂規則**，，然後按一下**下一步**。  
  
13. 在**設定規則**頁面上，在**理賠要求規則名稱**，輸入此規則的顯示名稱，例如「拒絕使用者 ipoutsiderange true 與 groupsid 失敗」。 在**自訂規則**中，輸入或下列理賠要求規則語言語法貼上：  
   
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. 按一下**完成**。 確認新規則立即下方的上一個規則及前允許所有使用者存取預設規則（即使出現在清單中稍早拒絕規則將會優先）發行授權規則清單中。  </br></br>如果您不需要允許存取規則預設值，您可以新增結尾，如下所示使用理賠要求規則語言清單的一項：  
   
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. 在儲存新規則，**編輯理賠要求規則**對話方塊中，按一下 [確定]。 結果清單應該如下所示。  
  
     ![發行](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a>建置 IP 位址範圍運算式  
 HTTP 標頭目前設定僅供換貨 Online 填入標頭，以 AD FS 傳遞驗證要求時，會填入 x ms-轉送-client-ip 理賠要求。 宣告值可能下列其中一個動作：  
  
> [!NOTE]
>  換貨 Online 目前支援只 IPV4 與不 IPV6 位址。  
  
-   單一 IP 位址：直接連接至換貨 Online client 的 IP 位址  
  
> [!NOTE]
>  -   Client 公司網路上的 IP 位址會出現外部介面組織的輸出 proxy 或閘道 IP 位址。  
> -   為內部公司戶端或外部戶端 VPN 或 DA.的設定而定，可能會出現戶端的或由 Microsoft DirectAccess (DA) VPN 連接到企業網路  
  
-   一或多個 IP 位址：時 Exchange Online 無法判斷連接 client 的 IP 位址，它將會設定 x 轉送的標頭，會納入 HTTP 為基礎的要求，支援許多戶端、負載平衡器，與市面上的 proxy 的非標準標頭的值為基礎的值。  
  
> [!NOTE]
>  1.  將會以逗號分隔多個 IP 位址，指出 client IP 位址和傳遞要求，每個 proxy 的地址。  
> 2.  不在清單上，將有關 Exchange Online 基礎結構的 IP 位址。  
  
### <a name="regular-expressions"></a>一般運算式  
 當您必須符合 IP 位址時，您需要建構運算式來進行比較。 在接下來的步驟，我們將會提供範例，以了解如何建立這類運算式符合下列地址範圍（請注意，您將會有變更成符合您公用 IP 範圍這些範例）：  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 第一次，符合單一 IP 位址的基本模式時，如下所示：\b###\\.###\\.###\\.###\b  
  
 將它擴展這，我們可以符合兩個或運算式使用不同的 IP 位址，如下所示：\b###\\.###\\.###\\.###\b 與 #124;\b###\\.###\\.###\\.###\b  
  
 因此，以符合兩個（例如 192.168.1.1 或 10.0.0.1）的地址就會：\b192\\.168\\.1\\.1\b 與 #124;\b10\\.0\\.0\\.1\b  
  
 這會讓您可以輸入地址任何數字的技術。 當您需要有各種不同的地址允許，例如 192.168.1.1 – 192.168.1.25，符合必須完成字元依字元：\b192\\.168\\.1\\。([1-9] 和 #124; 1 [0 9] 與 #124; [0 5] 2) \b  
  
 請注意下列動作：  
  
-   IP 位址會被視為字串，並不是數字。  
  
-   規則作業切割，如下所示：\b192\\.168\\.1\\。  
  
-   這比對任何值 192.168.1 開頭。  
  
-   下列符合最終小數點之後所需的地址的部分的範圍：  
  
    -   （[1-9] 相符項目的地址結尾 1-9  
  
    -   與 #124; 1 [0 9] 符合 10 至 19 結尾的地址  
  
    -   與 #124;2[0-5]) 相符項目的地址結尾 20-25  
  
-   請注意，必須正確位於括號，以便開始不符合其他部分的 IP 位址。  
  
-   符合 192 區塊時，我們可以撰寫 10 封鎖類似運算式：\b10\\.0\\.0\\。([1-9] 和 #124; 1 [0 4]) \b  
  
-   並將它們放在一起，為以下運算式應該符合」192.168.1.1~25」和「10.0.0.1~14」的所有地址：\b192\\.168\\.1\\。([1-9] 和 #124; 1 [0 9] 與 #124; [0 5] 2) \b 和 #124;\b10\\.0\\.0\\。([1-9] 和 #124; 1 [0 4]) \b  
  
### <a name="testing-the-expression"></a>測試運算式  
 Regex 運算式變成很難，因此建議使用 regex 驗證工具。 如果您在網際網路上的搜尋適用於「online regex 運算式」，您會發現幾個良好的 online 公用程式，可讓您可以嘗試範例資料對您運算式。  
  
 在測試運算式時，很重要，您知道必須符合的預期行為。 換貨 online 系統可能會傳送，以逗號分隔的許多 IP 位址。 這能運算式上面提供。 不過，請務必思考這測試 regex 運算式時。 例如，一可能會使用輸入驗證上述範例以下的範例：  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>宣告類型  
 在 Windows Server 2012 R2 AD FS 提供要求操作資訊使用宣告下列類型：  
  
### <a name="x-ms-forwarded-client-ip"></a>X MS-轉送-Client-IP  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 此 AD FS 宣告代表「盡量嘗試」，請確實提出要求的使用者 (例如，Outlook client) 的 IP 位址。 此宣告可能包含以多個 IP 位址，包括每個 proxy 轉寄要求的位址。  此宣告 HTTP 會填入。 宣告值可以下列其中一個動作：  
  
-   單一 IP 位址直接連接至換貨 Online client 的 IP 位址  
  
> [!NOTE]
>  Client 公司網路上的 IP 位址會出現外部介面組織的輸出 proxy 或閘道 IP 位址。  
  
-   一或多個 IP 位址  
  
    -   如果換貨 Online 無法判斷連接 client 的 IP 位址，它將會設定 x 轉送的標頭的值為基礎的可以根據 http 包含非標準標頭要求和支援許多戶端、負載平衡器，與市面上的 proxy 的值。  
  
    -   將會以逗號分隔指出 client IP 位址和每個 proxy 傳遞要求的地址多個 IP 位址。  
  
> [!NOTE]
>  將不會出現在清單中相關 Exchange Online 基礎結構的 IP 位址。  
  
> [!WARNING]
>  換貨 Online 目前支援只 IPV4 位址。不支援 IPV6 位址。  
  
### <a name="x-ms-client-application"></a>X MS-Client 的應用程式  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 此 AD FS 宣告代表結束 client，彈性對應至所使用的應用程式使用的通訊協定。  此宣告會填入 HTTP 標頭目前僅限設定換貨 Online，以 AD FS 傳遞驗證要求時，會填入標頭。 而定，應用程式的此宣告值將下列其中一個動作：  
  
-   如果使用 Exchange 使用同步的裝置的值為 Microsoft.Exchange.ActiveSync。  
  
-   使用 Microsoft Outlook client 可能會導致任何下列值：  
  
    -   Microsoft.Exchange.Autodiscover  
  
    -   Microsoft.Exchange.OfflineAddressBook  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
-   下列其他此標頭可能的值：  
  
    -   Microsoft.Exchange.Powershell  
  
    -   Microsoft.Exchange.SMTP  
  
    -   Microsoft.Exchange.Pop  
  
    -   Microsoft.Exchange.Imap  
  
### <a name="x-ms-client-user-agent"></a>X-MS-Client-使用者代理程式  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 此 AD FS 理賠要求提供代表 client 存取服務使用的裝置類型的字串。 這可以針對想要避免存取特定裝置 （例如智慧型手機的特定類型） 時使用。  此宣告值範例包括 （但不是限於） 下列值。  
  
 以下是範例 x ms-使用者代理值可能會包含對其 x ms-client 的應用程式是「Microsoft.Exchange.ActiveSync「client  
  
-   1.0 漩渦日  
  
-   蘋果-iPad1C1 日 812.1  
  
-   蘋果-iPhone3C1 日 811.2  
  
-   蘋果-iPhone 日 704.11  
  
-   Moto-DROID2/4.5.1  
  
-   100.202 SAMSUNGSPHD700 日  
  
-   Android 0.3 日  
  
 它也可是空的這個值。  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 此 AD FS 宣告指示要求已經通過 proxy Web 應用程式。  此宣告會填入 Web 應用程式 proxy 後端服務聯盟傳遞驗證要求時，會填入標頭。 AD FS 再將它轉換為理賠要求。  
  
 宣告的值為傳遞要求 Web 應用程式 proxy 的 DNS 名稱。  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 宣告類型： `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 類似上述 x-ms-proxy 宣告類型、此宣告類型表示要求已經通過 web proxy 應用程式。 然而 x ms proxy，insidecorporatenetwork 為 True 指出聯盟服務的公司網路中的直接要求布林值。  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X MS-端點-絕對值-路徑 （作用中與被動式）  
 宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 此宣告類型可用來判斷來自從 「 作用中的 「 （進階） 與 「 被動式 」 （web-瀏覽器為基礎） 戶端要求。 這可讓外部瀏覽器為基礎的應用程式例如 Outlook Web Access、 SharePoint Online 或 Office 365 入口網站時，會被封鎖來自從豐富例如 Microsoft Outlook 要求允許要求。  
  
 宣告的值為收到要求 AD FS 服務的名稱。  
  
## <a name="see-also"></a>也了  
 [AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md)
