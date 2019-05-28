---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: AD FS 中的存取控制原則
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13969958c9b4e0539993142d680cb6c34dc10750
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190247"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>在 Windows Server 2012 R2 和 Windows Server 2012 AD FS 存取控制原則


這篇文章中所述的原則會讓使用兩種類型的宣告  
  
1.  宣告 AD FS 會建立根據 AD FS 和 Web 應用程式 proxy 可以檢查並驗證，例如直接連線到 AD FS 或 WAP 的用戶端的 IP 位址的資訊。  
  
2.  AD FS 的宣告會建立根據資訊轉送至 AD FS 用戶端以 HTTP 標頭  
  
>**重要**:如下所述的原則會封鎖 Windows 10 加入網域並登入需要下列的其他端點的存取權的案例

AD FS 端點上所需的 Windows 10 的加入網域並登入。
- [federation service 名稱] / adfs/services/信任/2005年/windowstransport
- [federation service name]/adfs/services/trust/13/windowstransport
- [federation service 名稱] / adfs/services/信任/2005年/usernamemixed
- [federation service 名稱] / adfs/services/信任/13/usernamemixed 
- [federation service 名稱] / adfs/services/信任/2005年/certificatemixed
- [federation service 名稱] / adfs/services/信任/13/certificatemixed

若要解決，更新的任何原則，拒絕根據端點宣告，以允許上述端點的例外狀況。

例如，下列規則：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

會更新為：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  此類別的宣告應該只用來實作商務原則，而不是安全性原則來保護您的網路存取。  它是未經授權的用戶端可以用來存取傳送標頭與錯誤的資訊。  
  
這篇文章中所述的原則永遠應該搭配其他驗證方法，例如使用者名稱和密碼或多重要素驗證。  
  
## <a name="client-access-policies-scenarios"></a>用戶端存取原則案例  
  
|**案例**|**描述**| 
| --- | --- | 
|案例 1：封鎖所有對 Office 365 的外部存取|Office 365 存取允許從內部的公司網路上的所有用戶端，但從外部用戶端的要求會遭到拒絕外部用戶端的 IP 位址為基礎。|  
|案例 2：封鎖所有對 Office 365 Exchange ActiveSync 以外的外部存取|Office 365 存取允許從內部的公司網路上的所有用戶端，以及從任何外部的用戶端裝置，例如智慧型手機，使用 Exchange ActiveSync。 所有其他外部用戶端，例如使用 Outlook，將會遭到封鎖。|  
|案例 3：封鎖所有對外部存取 Office 365 除了瀏覽器為基礎的應用程式|區塊外部存取 Office 365，多半只有被動的 （以瀏覽器為基礎） 應用程式，例如 Outlook Web Access 或 SharePoint Online。|  
|案例 4:封鎖所有對外部存取 Office 365 除了指定的 Active Directory 群組|此案例中用於測試和驗證用戶端存取原則部署。 只能為一個或多個 Active Directory 群組的成員，它就會封鎖對 Office 365 的外部存取。 它也可以用來群組的成員，才能提供外部存取。|  
  
## <a name="enabling-client-access-policy"></a>啟用用戶端存取原則  
 若要啟用 Windows Server 2012 R2 中的 AD FS 中的用戶端存取原則，您必須更新 Microsoft Office 365 識別平台信賴憑證者信任。 選擇其中一個範例案例來設定宣告規則**Microsoft Office 365 識別平台**信賴憑證者信任最符合您組織的需求。  
  
###  <a name="scenario1"></a> 案例 1:封鎖所有對 Office 365 的外部存取  
 此用戶端存取原則案例可讓您存取所有的內部用戶端和區塊外部的用戶端的 IP 位址為基礎的所有外部用戶端。 您可以使用下列程序，將正確的發行授權規則新增至 Office 365 信賴憑證者信任您所選擇的案例。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>若要建立規則來封鎖所有對 Office 365 外部存取  
  
1.  從**伺服器管理員**，按一下**工具**，然後按一下**AD FS 管理**。  
  
2.  在主控台樹狀目錄中，在**AD fs\ 信任關係**，按一下**信賴憑證者信任**，以滑鼠右鍵按一下**Microsoft Office 365 識別平台**信任，然後按一下**編輯宣告規則**。  
  
3.  在**編輯宣告規則**對話方塊中，選取**發佈授權規則**索引標籤，然後再按**新增規則**開始宣告規則精靈。  
  
4.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
5.  在 **設定規則**頁面的 **宣告規則名稱**，這項規則，如範例的顯示名稱 」 的所需的範圍之外的任何 IP 宣告時拒絕 」 的型別。 底下**自訂規則**中，輸入或貼上以下的宣告規則語言語法 （取代 「 x-ms-轉送-用戶端-ip 」 是有效的 IP 運算式使用上述的值）：  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  按一下 **[完成]** 。 確認新的規則，會出現在 發行授權規則清單之前為預設值**允許所有使用者存取**（雖然看起來稍早在清單中，「 拒絕 」 規則都有優先權） 的規則。  如果您沒有預設值允許的存取規則，您可以加入一個，如下所示使用宣告規則語言的程式清單的結尾：  </br>
    
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  若要將新的規則，儲存在**編輯宣告規則** 對話方塊中，按一下**確定**。 產生的清單看起來應該如下所示。  
  
     ![發佈授權規則](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a> 案例 2:封鎖所有對 Office 365 Exchange ActiveSync 以外的外部存取  
 下列範例可讓所有的 Office 365 應用程式，包括 來自內部用戶端，包括 Outlook 的 Exchange Online 的存取。 用戶端的 IP 位址，例如智慧型手機的 Exchange ActiveSync 用戶端除了所示，它會封鎖從位於公司網路之外的用戶端存取。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>若要建立規則來封鎖對 Exchange ActiveSync 以外的 Office 365 的所有外部存取  
  
1.  從**伺服器管理員**，按一下**工具**，然後按一下**AD FS 管理**。  
  
2.  在主控台樹狀目錄中，在**AD fs\ 信任關係**，按一下**信賴憑證者信任**，以滑鼠右鍵按一下**Microsoft Office 365 識別平台**信任，然後按一下**編輯宣告規則**。  
  
3.  在**編輯宣告規則**對話方塊中，選取**發佈授權規則**索引標籤，然後再按**新增規則**開始宣告規則精靈。  
  
4.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
5.  在 **設定規則**頁面的 **宣告規則名稱**，輸入範例 」 的所需的範圍之外的任何 IP 宣告是否發出 ipoutsiderange 宣告 」 這項規則的顯示名稱。 底下**自訂規則**中，輸入或貼上以下的宣告規則語言語法 （取代 「 x-ms-轉送-用戶端-ip 」 是有效的 IP 運算式使用上述的值）：  
    
    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  按一下 **[完成]** 。 確認新的規則，會出現在**發佈授權規則**清單。  
  
7.  接下來，在**編輯宣告規則**對話方塊的 **發佈授權規則**索引標籤上，按一下 **新增規則**再次啟動 宣告規則精靈。  
  
8.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
9. 在 **設定規則**頁面的 **宣告規則名稱**，這項規則，如範例的顯示名稱 「 如果沒有所需的範圍之外的 IP，而且有非 EAS x-ms-用戶端-應用程式宣告，拒絕的類型"。 底下**自訂規則**中，輸入或貼上下列宣告規則語言語法：  
  

    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. 按一下 **[完成]** 。 確認新的規則，會出現在**發佈授權規則**清單。  
  
11. 接下來，在**編輯宣告規則**對話方塊的 **發佈授權規則**索引標籤上，按一下 **新增規則**再次啟動 宣告規則精靈。  
  
12. 在 **選取規則範本**頁面的 **宣告規則範本，** 選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
13. 在 **設定規則**頁面的 **宣告規則名稱**中，輸入此規則的顯示名稱，例如 「 檢查應用程式宣告是否存在 」。 底下**自訂規則**中，輸入或貼上下列宣告規則語言語法：  
  
    ```  
    NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. 按一下 **[完成]** 。 確認新的規則，會出現在**發佈授權規則**清單。  
  
15. 接下來，在**編輯宣告規則**對話方塊的 **發佈授權規則**索引標籤上，按一下 **新增規則**再次啟動 宣告規則精靈。  
  
16. 在 **選取規則範本**頁面的 **宣告規則範本，** 選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
17. 在 **設定規則**頁面的 **宣告規則名稱**中，輸入此規則的顯示名稱，例如 「 拒絕 ipoutsiderange true 與應用程式的使用者失敗 」。 底下**自訂規則**中，輸入或貼上下列宣告規則語言語法：  
  
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. 按一下 **[完成]** 。 確認新的規則會立即顯示在上一個規則之下，之前為預設值允許所有使用者存取規則 （雖然看起來稍早在清單中，「 拒絕 」 規則都有優先權） 的發行授權規則清單中。  </br>如果您沒有預設值允許的存取規則，您可以加入一個，如下所示使用宣告規則語言的程式清單的結尾：</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. 若要將新的規則，儲存在**編輯宣告規則**對話方塊方塊中，按一下 [確定]。 產生的清單看起來應該如下所示。  
  
     ![發佈授權規則](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a> 案例 3:封鎖所有對外部存取 Office 365 除了瀏覽器為基礎的應用程式  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要建立規則來封鎖所有對外部存取 Office 365 除了瀏覽器為基礎的應用程式  
  
1.  從**伺服器管理員**，按一下**工具**，然後按一下**AD FS 管理**。  
  
2.  在主控台樹狀目錄中，在**AD fs\ 信任關係**，按一下**信賴憑證者信任**，以滑鼠右鍵按一下**Microsoft Office 365 識別平台**信任，然後按一下**編輯宣告規則**。  
  
3.  在**編輯宣告規則**對話方塊中，選取**發佈授權規則**索引標籤，然後再按**新增規則**開始宣告規則精靈。  
  
4.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
5.  在 **設定規則**頁面的 **宣告規則名稱**，輸入範例 」 的所需的範圍之外的任何 IP 宣告是否發出 ipoutsiderange 宣告 」 這項規則的顯示名稱。 底下**自訂規則**中，輸入或貼上以下的宣告規則語言語法 （取代 「 x-ms-轉送-用戶端-ip 」 是有效的 IP 運算式使用上述的值）：  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  按一下 **[完成]** 。 確認新的規則，會出現在**發佈授權規則**清單。  
  
7.  接下來，在**編輯宣告規則**對話方塊的 **發佈授權規則**索引標籤上，按一下 **新增規則**再次啟動 宣告規則精靈。  
  
8.  在 **選取規則範本**頁面的 **宣告規則範本，** 選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
9. 在 **設定規則**頁面的 **宣告規則名稱**，這項規則，如範例的顯示名稱 「 如果沒有所需的範圍之外的 IP，而且端點不是/adfs/ls，拒絕 」 的型別。 底下**自訂規則**中，輸入或貼上下列宣告規則語言語法：  
  
 
    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. 按一下 **[完成]** 。 確認新的規則，會出現在 發行授權規則清單之前為預設值**允許所有使用者存取**（雖然看起來稍早在清單中，「 拒絕 」 規則都有優先權） 的規則。  </br></br> 如果您沒有預設值允許的存取規則，您可以加入一個，如下所示使用宣告規則語言的程式清單的結尾：  
  
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. 若要將新的規則，儲存在**編輯宣告規則** 對話方塊中，按一下**確定**。 產生的清單看起來應該如下所示。  
  
     ![發佈](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a> 案例 4:封鎖所有對外部存取 Office 365 除了指定的 Active Directory 群組  
 下列範例會啟用從內部 IP 位址為基礎的用戶端存取。 它會封鎖從位於公司網路外部的用戶端具有外部用戶端 IP 位址存取，在指定的 Active Directory Group.Use 內部人員除了下列步驟來新增正確的發行授權規則加入至**Microsoft Office 365 識別平台**信賴憑證者信任使用宣告規則精靈:  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>若要建立規則來封鎖所有對 Office 365 的外部存取，但指定 Active Directory 群組  
  
1.  從**伺服器管理員**，按一下**工具**，然後按一下**AD FS 管理**。  
  
2.  在主控台樹狀目錄中，在**AD fs\ 信任關係**，按一下**信賴憑證者信任**，以滑鼠右鍵按一下**Microsoft Office 365 識別平台**信任，然後按一下**編輯宣告規則**。  
  
3.  在**編輯宣告規則**對話方塊中，選取**發佈授權規則**索引標籤，然後再按**新增規則**開始宣告規則精靈。  
  
4.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
5.  在 **設定規則**頁面的 **宣告規則名稱**，輸入這項規則的顯示名稱，如範例 」 的所需的範圍之外的任何 IP 宣告是否發出 ipoutsiderange 宣告。 」 底下**自訂規則**中，輸入或貼上以下的宣告規則語言語法 （取代 「 x-ms-轉送-用戶端-ip 」 是有效的 IP 運算式使用上述的值）：  
  
      
    `c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  按一下 **[完成]** 。 確認新的規則，會出現在**發佈授權規則**清單。  
  
7.  接下來，在**編輯宣告規則**對話方塊的 **發佈授權規則**索引標籤上，按一下 **新增規則**再次啟動 宣告規則精靈。  
  
8.  在 **選取規則範本**頁面的 **宣告規則範本，** 選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
9. 在 **設定規則**頁面的 **宣告規則名稱**中，輸入此規則的顯示名稱，例如 檢查群組 SID。 底下**自訂規則**中，輸入或貼上以下的宣告規則語言語法 (取代"groupsid 」 您使用的 AD 群組的實際 SID 取代):  
   
    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. 按一下 **[完成]** 。 確認新的規則，會出現在**發佈授權規則**清單。  
  
11. 接下來，在**編輯宣告規則**對話方塊的 **發佈授權規則**索引標籤上，按一下 **新增規則**再次啟動 宣告規則精靈。  
  
12. 在 **選取規則範本**頁面的 **宣告規則範本，** 選取**使用自訂規則傳送宣告**，然後按一下**下一步** 。  
  
13. 在 **設定規則**頁面的 **宣告規則名稱**中，輸入此規則的顯示名稱，例如 「 拒絕使用者使用 ipoutsiderange true 和 groupsid 失敗 」。 底下**自訂規則**中，輸入或貼上下列宣告規則語言語法：  
   
    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. 按一下 **[完成]** 。 確認新的規則會立即顯示在上一個規則之下，之前為預設值允許所有使用者存取規則 （雖然看起來稍早在清單中，「 拒絕 」 規則都有優先權） 的發行授權規則清單中。  </br></br>如果您沒有預設值允許的存取規則，您可以加入一個，如下所示使用宣告規則語言的程式清單的結尾：  
   
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. 若要將新的規則，儲存在**編輯宣告規則**對話方塊方塊中，按一下 [確定]。 產生的清單看起來應該如下所示。  
  
     ![發佈](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a> 建立 IP 位址範圍運算式  
 X-ms-轉送-用戶端-ip 宣告會填入目前設定只能由 Exchange Online，會將驗證要求傳遞至 AD FS 時填入標頭的 HTTP 標頭。 宣告的值可能是下列其中一項：  
  
> [!NOTE]
>  Exchange Online 目前支援僅 IPV4，而非 IPV6 位址。  
  
-   單一 IP 位址：用戶端 IP 位址直接連線到 Exchange Online  
  
> [!NOTE]
>  -   公司網路上的用戶端的 IP 位址會顯示為組織的連出 proxy 或閘道的外部介面 IP 位址。  
> -   為內部的公司用戶端或外部的用戶端的 VPN 或 DA.組態而定，可能會出現或由 Microsoft DirectAccess (DA) VPN 連線到公司網路的用戶端  
  
-   一或多個 IP 位址：當 Exchange Online 無法判斷連線的用戶端的 IP 位址時，它會根據設定值 x 轉送的標頭的值，非標準的標頭可以包含在 HTTP 為基礎的要求，並支援許多用戶端，負載平衡器與市場上的 proxy。  
  
> [!NOTE]
>  1.  多個 IP 位址，表示用戶端 IP 位址並傳遞要求中，每個 proxy 的位址會以逗號分隔。  
> 2.  不在清單上，將 Exchange Online 的基礎結構相關的 IP 位址。  
  
### <a name="regular-expressions"></a>規則運算式  
 當您有相符的 IP 位址範圍時，而又需要建構規則運算式來執行比較。 在下一系列的步驟中，我們將提供如何建構這類運算式，以符合下列位址範圍 （請注意，您必須變更這些範例，以符合您的公用 IP 範圍） 的範例：  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 首先，會比對單一 IP 位址的基本模式如下所示： \b###\\。 # # #\\。 # # #\\。 # # # \b  
  
 擴充這種情況，我們可以比使用 OR 運算式的兩個不同 IP 位址，如下所示： \b###\\。 # # #\\。 # # #\\。 # # # \b&#124;\b###\\。 # # #\\。 # # #\\。 # # # \b  
  
 因此，要比對兩個位址 （例如 192.168.1.1 或 10.0.0.1） 範例： \b192\\.168\\.1\\。 1\b&#124;\b10\\.0\\.0\\。 1\b  
  
 這可讓您用中，您可以輸入任意數目的地址的技巧。 比對的位址範圍需要允許，例如 192.168.1.1-192.168.1.25，其中字元的字元必須以： \b192\\.168\\.1\\。 ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b  
  
 請注意下列事項：  
  
-   IP 位址會被視為字串並不是數字。  
  
-   此規則細分，如下所示： \b192\\.168\\.1\\。  
  
-   這會比對任何以 192.168.1 值開頭的。  
  
-   下列比對的最後一個小數點後面所需的位址部分的範圍：  
  
    -   （[1-9] 相符項目地址結尾為 1-9  
  
    -   &#124;1 [0-9] 比對結尾為 10-19 的位址  
  
    -   &#124;結尾為 20-25 2[0-5]) 相符項目位址  
  
-   請注意，必須正確置於括號，以便您不要啟動比對 IP 位址的其他部分也一樣。  
  
-   比對 192 區塊時，我們可以撰寫類似的運算式，針對 10 個區塊： \b10\\.0\\.0\\。 ([1-9]&#124;1 [0-4]) \b  
  
-   並將它們放在一起，下列運算式應符合 「 192.168.1.1~25"和"10.0.0.1~14"的所有地址： \b192\\.168\\.1\\。 ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\.0\\.0\\。 ([1-9]&#124;1 [0-4]) \b  
  
### <a name="testing-the-expression"></a>測試運算式  
 Regex 運算式可能會變得相當棘手，因此強烈建議使用 regex 驗證工具。 如果您在網際網路上搜尋 「 線上的 regex 運算式產生器 」，您會發現幾個良好的線上公用程式，可讓您試用您針對取樣資料的運算式。  
  
 測試運算式時，您必須了解要預期一定要相符的項目。 Exchange online 系統可能會傳送多個 IP 位址，並以逗號分隔。 在上面提供的運算式都適用於此。 不過，務必測試您的 regex 運算式時，想想這個問題。 比方說，其中可能會使用下列範例來驗證上述的範例輸入：  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>宣告類型  
 Windows Server 2012 R2 中的 AD FS 提供使用下列宣告類型的要求內容資訊：  
  
### <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP  
 宣告類型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 此 AD FS 宣告表示 「 地嘗試 「 查明提出要求的使用者 （例如 Outlook 用戶端） 的 IP 位址。 此宣告可以包含多個 IP 位址，包括將要求轉送給每個 proxy 的位址。  此宣告會從 HTTP 填入。 宣告的值可以是下列其中一項：  
  
-   單一 IP 位址直接連線到 Exchange Online 的用戶端的 IP 位址  
  
> [!NOTE]
>  公司網路上的用戶端的 IP 位址會顯示為組織的連出 proxy 或閘道的外部介面 IP 位址。  
  
-   一或多個 IP 位址  
  
    -   如果 Exchange Online 無法判斷連線的用戶端的 IP 位址，它會根據設定值 x 轉送的標頭值，可以包含在 HTTP 型的非標準標頭要求，並支援許多用戶端，負載平衡器和市面上的 proxy。  
  
    -   指出用戶端 IP 位址和每個傳遞要求的 proxy 位址的多個 IP 位址會以逗號分隔。  
  
> [!NOTE]
>  Exchange Online 的基礎結構相關的 IP 位址將不會出現在清單中。  
  
> [!WARNING]
>  Exchange Online 目前支援的只有 IPV4 位址;它不支援 IPV6 位址。  
  
### <a name="x-ms-client-application"></a>X-MS-Client-Application  
 宣告類型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 此 AD FS 宣告表示結束用戶端，鬆散對應至所使用的應用程式所使用的通訊協定。  此宣告會從目前的 HTTP 標頭填入只能設定 Exchange online，會將驗證要求傳遞至 AD FS 時填入標頭。 根據應用程式，此宣告的值會是下列其中一項：  
  
-   在使用 Exchange Active Sync 的裝置中，這個值會是 Microsoft.Exchange.ActiveSync。  
  
-   使用 Microsoft Outlook 用戶端可能會導致下列值之一：  
  
    -   Microsoft.Exchange.Autodiscover  
  
    -   Microsoft.Exchange.OfflineAddressBook  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
-   其他可能的值，此標頭包括下列各項：  
  
    -   Microsoft.Exchange.Powershell  
  
    -   Microsoft.Exchange.SMTP  
  
    -   Microsoft.Exchange.Pop  
  
    -   Microsoft.Exchange.Imap  
  
### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 宣告類型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 此 AD FS 宣告提供一個字串，代表用戶端用來存取服務的裝置類型。 這可以在客戶想要防止存取某些裝置 （例如智慧型手機的特殊類型） 時使用。  此宣告的範例值包括 （但不限於） 以下的值。  
  
 以下是範例的 x-ms-使用者-代理程式值可能會包含其 x-ms-用戶端-應用程式是 「 Microsoft.Exchange.ActiveSync"的用戶端  
  
-   旋風/1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android/0.3  
  
 此外，也可以是空的這個值。  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 宣告類型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 此 AD FS 宣告表示要求已通過 Web 應用程式 proxy。  將驗證要求傳遞至後端 Federation Service 時，會填入標頭的 Web 應用程式 proxy 會填入此宣告。 AD FS 然後將它轉換成宣告。  
  
 宣告的值是傳遞要求的 Web 應用程式 proxy 的 DNS 名稱。  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 宣告類型： `http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 類似於上述 x ms-proxy 宣告類型，這種宣告類型會指出要求是否已通過 web 應用程式 proxy。 不同於 x ms proxy，insidecorporatenetwork 是布林值，則為 True，指出直接從公司網路內部 federation service 的要求。  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-端點-絕對-路徑 (使用中 vs 被動)  
 宣告類型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 這種宣告類型可以用於判斷發出的 「 作用中 」 或 「 被動 」 （web 瀏覽器型） 的用戶端的 （豐富） 用戶端要求。 這可讓來自瀏覽器型應用程式，例如 Outlook Web Access、 SharePoint Online 或 Office 365 入口網站會封鎖來自 如 Microsoft Outlook 的豐富型用戶端的要求時允許的外部要求。  
  
 宣告的值是所接收到要求的 AD FS 服務名稱。  
  
## <a name="see-also"></a>另請參閱  
 [AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
