---
title: AD FS 疑難排解-稽核事件和記錄
description: 本文件說明如何使用各種不同的 AD FS 記錄檔對問題進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1acc00ca376c48f7fb34214cef3a92961d355ae4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444018"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>AD FS 疑難排解-事件和記錄
AD FS 提供兩個可用於疑難排解的主要記錄檔。  其中包括：

- 系統管理記錄檔
- 追蹤記錄檔  
 
將下面說明每個這些記錄檔。

## <a name="admin-log"></a>系統管理記錄檔
系統管理記錄檔提供問題發生，且預設會啟用高的層級資訊。

### <a name="to-view-the-admin-log"></a>若要檢視系統管理記錄檔
1.  開啟事件檢視器
2.  依序展開**應用程式及服務記錄檔**。
3.  依序展開**AD FS**。
4.  按一下  **Admin**。

![稽核增強功能](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>追蹤記錄檔
追蹤記錄是詳細的訊息會記錄和疑難排解時，將會是最有用的記錄檔的位置。 因為短暫的時間，這可能會影響系統效能，可產生大量追蹤記錄檔資訊預設會停用追蹤記錄檔。 

### <a name="to-enable-and-view-the-trace-log"></a>若要啟用和檢視追蹤記錄檔
1.  開啟事件檢視器
2.  以滑鼠右鍵按一下**應用程式及服務記錄檔**並選取 [檢視]，然後按一下**顯示分析與偵錯記錄檔**。  這會在左邊顯示的其他節點。
![稽核增強功能](media/ad-fs-tshoot-logging/event2.PNG)  
3.  依序展開 AD FS 追蹤
4.  以滑鼠右鍵按一下 偵錯，然後選取**啟用記錄**。
![稽核增強功能](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>事件稽核資訊適用於 AD FS Windows Server 2016 上  
根據預設，Windows Server 2016 中的 AD FS 會具有基本的層級的稽核功能。  基本稽核時，系統管理員會看到針對單一要求的 5 個以下的事件。  這會將標示大幅降低系統管理員可以查看，若要查看單一要求的事件數目。   稽核層級可能會引發，或降低使用 PowerShell cmdlet:  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

下表說明可用的稽核層級。  

|稽核層級|PowerShell 語法|描述|  
|----- | ----- | ----- |
|None|Set-adfsproperties-AuditLevel None|已停用稽核，而且會記錄任何事件。|  
|基本 （預設值）|Set-AdfsProperties - AuditLevel Basic|單一要求就會記錄 5 個以上的事件|  
|Verbose|Set-adfsproperties-AuditLevel 詳細資訊|會記錄所有事件。  它會記錄大量的每個要求的資訊。|  
  
若要檢視目前的稽核層級，您可以使用 PowerShell cmdlet:Get-AdfsProperties.  
  
![稽核增強功能](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
稽核層級可能會引發，或降低使用 PowerShell cmdlet:Set-AdfsProperties -AuditLevel.  
  
![稽核增強功能](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>類型的事件  
AD FS 事件可以屬於不同的類型，根據不同的型別，AD FS 所處理的要求。 事件的每個類型都有與其相關聯的特定資料。  事件的類型可以區別登入要求 （也就是權杖要求），與系統要求 （包括提取組態資訊的伺服器呼叫）。    

下表說明基本的事件類型。  
  
|事件類型|事件識別碼|描述| 
|----- | ----- | ----- | 
|全新的認證驗證成功|1202|其中最新的認證會驗證 Federation service 的成功要求。 這包括 Ws-trust、 WS-同盟、 SAML-P （第一個階段，以產生 SSO） 和 OAuth 授權端點。|  
|全新的認證驗證錯誤|1203|要求，Federation Service 全新的認證驗證失敗的位置。 這包括 Ws-trust、 Ws-fed、 SAML-P （第一個階段，以產生 SSO） 和 OAuth 授權端點。|  
|應用程式權杖成功|1200|安全性權杖已成功發出 Federation Service 所要求。 WS-同盟，SAML-P SSO 成品處理要求時，這會記錄。 （例如 SSO cookie)。|  
|應用程式的權杖失敗|1201|要求，Federation Service 的安全性權杖發行失敗的位置。 WS-同盟，SAML-P SSO 成品處理要求時，這會記錄。 （例如 SSO cookie)。|  
|密碼變更要求成功|1204|密碼變更要求的其中一個交易已成功處理同盟服務。|  
|密碼變更要求時發生錯誤|1205|密碼變更要求的其中一個交易無法 Federation Service 所處理。| 
|登出成功|1206|描述成功登出要求。|  
|登出失敗|1207|描述失敗的登出要求。|  

## <a name="security-auditing"></a>安全性稽核
AD FS 服務帳戶的安全性稽核有時可協助追蹤問題的密碼更新、 要求/回應記錄、 要求 contect 標頭和裝置註冊結果。  依預設已停用稽核的 AD FS 服務帳戶。

### <a name="to-enable-security-auditing"></a>若要啟用安全性稽核
1. 按一下 [開始]，指向**程式**，指向**系統管理工具**，然後按一下**本機安全性原則**。
2. 瀏覽到 [安全性設定\本機原則\使用者權限管理]  資料夾，然後按兩下 [產生安全性稽核]  。
3. 在 **本機安全性設定**索引標籤上，確認已列出 AD FS 服務帳戶。 如果不存在，按一下 新增使用者或群組並將它新增至清單中，，然後按一下 確定。
4. 使用提高的權限開啟命令提示字元並執行下列命令，以啟用稽核 auditpol.exe /set /subcategory:"Application Generated"/failure /success:enable
5. 關閉**本機安全性原則**，然後開啟 AD FS 管理嵌入式管理單元。
 
若要開啟 AD FS 管理嵌入式管理單元，按一下 開始，指向程式、 指向 系統管理工具，然後按一下 AD FS 管理。
 
6. 在 動作 窗格中，按一下 編輯 Federation Service 屬性
7. 在 [Federation Service 屬性] 對話方塊中，按一下 [事件] 索引標籤。
8. 選取 **成功稽核**並**失敗稽核**核取方塊。
9. 按一下 [確定]。

![稽核增強功能](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>只有在 AD FS 是獨立的成員伺服器上時，才使用上述的指示。  如果執行 AD FS 的網域控制站，而不是本機安全性原則中，使用**預設網域控制站原則**位於**群組原則管理/樹系/網域/網域控制站**。  按一下 [編輯] 並瀏覽至**電腦設定 \ 原則 \windows 設定 \ 安全性 Settings\Local Policies\User Rights Management**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Windows Communication Foundation 和 Windows Identity Foundation 訊息
有時候除了追蹤記錄，您可能需要檢視 Windows Communication Foundation (WCF) 與 Windows Identity Foundation (WIF) 的訊息，以便針對問題進行疑難排解。 這可以藉由修改**Microsoft.IdentityServer.ServiceHost.Exe.Config** AD FS 伺服器上的檔案。 

此檔案位於 **< %系統根目錄 %> \Windows\ADFS**和 XML 格式。 檔案的相關部分如下所示： 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


套用這些變更之後，儲存設定，並重新啟動 AD FS 服務。 您會藉由設定適當的參數，讓這些追蹤之後，它們會出現在 Windows 事件檢視器中的 AD FS 追蹤記錄檔。

## <a name="correlating-events"></a>相互關聯事件
若要疑難排解最困難的事情之一是產生許多錯誤，或偵錯事件的存取問題。

若要協助，AD FS 相互關聯會記錄到事件檢視器中，系統管理員和偵錯記錄檔，使用唯一全域唯一識別碼 (GUID) 稱為活動識別碼。 對應到特定的要求中的所有事件 Web 應用程式 （適用於使用被動式要求者設定檔的應用程式） 或直接傳送到宣告提供者 （適用於使用 Ws-trust 的應用程式） 的要求一開始看到權杖發佈要求時，會產生此識別碼。 

![activityid](media/ad-fs-tshoot-logging/activityid1.png)

此活動識別碼的要求，在整段期間維持不變，並會記錄為一部分的每個事件記錄在事件檢視器該要求。 這表示：
 - 篩選或搜尋事件檢視器使用此識別碼可協助追蹤的所有對應至權杖要求的相關的事件的活動
 - 相同的活動識別碼會記錄在不同的電腦，可讓您進行疑難排解的使用者要求到多部電腦，例如同盟伺服器 proxy (FSP)
 - 活動識別碼也會出現在使用者的瀏覽器在 AD FS 要求失敗時以任何方式，因此可讓使用者進行通訊，此識別碼，以協助支援人員或 IT 支援人員。

![activityid](media/ad-fs-tshoot-logging/activityid2.png)

為了協助疑難排解程序中，AD FS 也會記錄每當 AD FS 伺服器上的權杖發佈程序失敗時，才會進行呼叫者識別碼事件。 本項目中包含的宣告類型和下列宣告類型，假設這項資訊做為權杖要求的一部分傳遞至 Federation Service 的其中一個值：
- http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress 
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- http://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier 

呼叫者識別碼事件也會記錄可讓您使用該活動識別碼篩選或搜尋事件日誌中的特定要求的活動識別碼。




## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
