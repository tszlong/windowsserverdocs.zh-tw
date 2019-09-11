---
title: AD FS 疑難排解-審核事件和記錄
description: 本檔說明如何使用各種不同的 AD FS 記錄來疑難排解問題
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: da3214b715b415eed2cbce351cae93eff14a88c7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865530"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>AD FS 疑難排解-事件和記錄
AD FS 提供兩個可用於疑難排解的主要記錄檔。  其中包括：

- 管理員記錄檔
- 追蹤記錄檔  
 
以下將說明這些記錄檔的每一個。

## <a name="admin-log"></a>管理員記錄檔
系統管理記錄檔會針對發生的問題提供高階資訊, 並依預設啟用。

### <a name="to-view-the-admin-log"></a>若要查看系統管理員記錄
1.  開啟事件檢視器
2.  展開 [**應用程式及服務記錄**檔]。
3.  展開 [ **AD FS**]。
4.  按一下 [系統**管理員**]。

![audit 增強功能](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>追蹤記錄檔
追蹤記錄檔是記錄詳細訊息的位置, 而且在進行疑難排解時, 會是最有用的記錄。 由於許多追蹤記錄資訊可能會在短時間內產生 (可能會影響系統效能), 因此預設會停用追蹤記錄。 

### <a name="to-enable-and-view-the-trace-log"></a>啟用和查看追蹤記錄檔
1.  開啟事件檢視器
2.  以滑鼠右鍵按一下 [**應用程式及服務記錄**檔], 然後選取 [view], 再按一下 [**顯示分析和 Debug 記錄**]。  這會在左側顯示其他節點。
![audit 增強功能](media/ad-fs-tshoot-logging/event2.PNG)  
3.  展開 AD FS 追蹤
4.  以滑鼠右鍵按一下 [Debug], 然後選取 [**啟用記錄**]。
![audit 增強功能](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Windows Server 2016 上 AD FS 的事件審核資訊  
根據預設, Windows Server 2016 中的 AD FS 已啟用基本層級的審核。  在基本的審核中, 系統管理員會針對單一要求看到5個或更少的事件。  這會導致系統管理員必須查看的事件數目大幅降低, 以便看到單一要求。   您可以使用 PowerShell cmdlt 來提高或降低審核層級:  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

下表說明可用的審核層級。  

|稽核層級|PowerShell 語法|描述|  
|----- | ----- | ----- |
|無|Set-adfsproperties-AuditLevel None|已停用審核, 而且不會記錄任何事件。|  
|基本 (預設值)|Set-adfsproperties-AuditLevel Basic|單一要求不會記錄超過5個事件|  
|詳細資訊|Set-adfsproperties-AuditLevel Verbose|將記錄所有事件。  這會針對每個要求記錄大量的資訊。|  
  
若要查看目前的審核層級, 您可以使用 PowerShell cmdlt:Set-adfsproperties。  
  
![audit 增強功能](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
您可以使用 PowerShell cmdlt 來提高或降低審核層級:Set-adfsproperties-AuditLevel。  
  
![audit 增強功能](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>事件種類  
AD FS 事件可以是不同類型, 根據 AD FS 處理的不同要求類型而定。 每種類型的事件都有相關聯的特定資料。  事件種類可以在登入要求 (例如權杖要求) 與系統要求 (包括提取設定資訊的伺服器-伺服器呼叫) 之間進行區分。    

下表描述事件的基本類型。  
  
|事件類型|事件識別碼|描述| 
|----- | ----- | ----- | 
|全新認證驗證成功|1202|同盟服務成功驗證新認證的要求。 這包括 WS-TRUST、WS-同盟、SAML-P (產生 SSO 的第一個階段) 和 OAuth 授權端點。|  
|新的認證驗證錯誤|1203|同盟服務上的新認證驗證失敗的要求。 這包括 WS-TRUST、WS-送出、SAML-P (產生 SSO 的第一個階段) 和 OAuth 授權端點。|  
|應用程式權杖成功|1200|同盟服務成功發出安全性權杖的要求。 針對 WS-同盟, 當使用 SSO 成品處理要求時, 會記錄此專案。 (例如 SSO cookie)。|  
|應用程式權杖失敗|1201|同盟服務上安全性權杖發行失敗的要求。 針對 WS-同盟, 在使用 SSO 成品處理要求時, 會記錄此 SAML-P。 (例如 SSO cookie)。|  
|密碼變更要求成功|1204|同盟服務已成功處理密碼變更要求的交易。|  
|密碼變更要求錯誤|1205|同盟服務無法處理密碼變更要求的交易。| 
|登出成功|1206|說明成功的登出要求。|  
|登出失敗|1207|描述失敗的登出要求。|  

## <a name="security-auditing"></a>安全性稽核
AD FS 服務帳戶的安全性審核有時可以協助追蹤密碼更新、要求/回應記錄、要求內容標頭和裝置註冊結果的問題。  預設會停用 AD FS 服務帳戶的審核。

### <a name="to-enable-security-auditing"></a>啟用安全性審核
1. 按一下 [開始], 依序指向 [**程式**] 和 [系統**管理工具**], 然後按一下 [**本機安全性原則**]。
2. 瀏覽到 [安全性設定\本機原則\使用者權限管理] 資料夾，然後按兩下 [產生安全性稽核]。
3. 在 [**本機安全性設定**] 索引標籤上, 確認已列出 [AD FS 服務帳戶]。 如果不存在, 請按一下 [新增使用者或群組] 並將它新增至清單中, 然後按一下 [確定]。
4. 以較高的許可權開啟命令提示字元, 並執行下列命令以啟用 [/subcategory]: [應用程式產生的]/failure: [啟用/success: 啟用]
5. 關閉 [**本機安全性原則**], 然後開啟 [AD FS 管理] 嵌入式管理單元。
 
若要開啟 [AD FS 管理] 嵌入式管理單元, 請按一下 [開始], 指向 [程式], 指向 [系統管理工具], 然後按一下 [AD FS 管理]。
 
6. 在 [動作] 窗格中, 按一下 [編輯] 同盟服務屬性
7. 在 [同盟服務屬性] 對話方塊中, 按一下 [事件] 索引標籤。
8. 選取 [**成功審核**和**失敗審核**] 核取方塊。
9. 按一下 [確定]。

![audit 增強功能](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>只有當 AD FS 在獨立成員伺服器上時, 才會使用上述指示。  如果 AD FS 是在網域控制站上執行, 而不是本機安全性原則, 請使用位於**群組原則管理/樹系/網域/網域控制站**中的**預設網域控制站原則**。  按一下 編輯, 然後流覽至 **電腦設定 \ \windows**  設置 \ 本機原則 \ Rights Management

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Windows Communication Foundation 和 Windows Identity Foundation 訊息
除了追蹤記錄之外, 有時候您可能需要查看 Windows Communication Foundation (WCF) 和 Windows Identity Foundation (WIF) 訊息, 才能針對問題進行疑難排解。 藉由修改 AD FS 伺服器上的**IdentityServer** , 即可完成這項作業。 

此檔案位於 **<% system root% > \Windows\ADFS**中，且格式為 XML。 檔案的相關部分如下所示: 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


套用這些變更之後, 請儲存設定, 然後重新開機 AD FS 服務。 藉由設定適當的參數來啟用這些追蹤之後, 它們就會出現在 Windows 事件檢視器的 AD FS 追蹤記錄中。

## <a name="correlating-events"></a>相互關聯事件
最難排解的問題之一, 就是產生大量錯誤或 debug 事件的存取問題。

為協助達成此目標, AD FS 在管理員和 debug 記錄檔中, 將所有記錄到事件檢視器的事件相互關聯, 這兩者都是使用唯一的全域唯一識別碼 (GUID) (稱為活動識別碼) 對應到特定的要求。 當令牌發佈要求最初呈現給 web 應用程式 (適用于使用被動要求者設定檔的應用程式) 或直接傳送至宣告提供者 (適用于使用 WS-TRUST 的應用程式) 的要求時, 就會產生此識別碼。 

![activityid](media/ad-fs-tshoot-logging/activityid1.png)

這個活動識別碼在要求的整個持續期間都維持不變, 而且會記錄為該要求的事件檢視器中記錄的每個事件的一部分。 這表示：
 - 使用此活動識別碼篩選或搜尋事件檢視器有助於追蹤對應至權杖要求的所有相關事件
 - 相同的活動識別碼會記錄在不同的電腦上, 讓您能夠針對多部電腦 (例如同盟伺服器 proxy (FSP)) 進行使用者要求的疑難排解
 - 如果 AD FS 要求會以任何方式失敗，則活動識別碼也會出現在使用者的瀏覽器中，因此可讓使用者將此識別碼傳達給服務台或 IT 支援人員。

![activityid](media/ad-fs-tshoot-logging/activityid2.png)

為了協助進行疑難排解程式, AD FS 也會在 AD FS 伺服器上的權杖發佈程式失敗時, 記錄呼叫者識別碼事件。 此事件包含下列其中一種宣告類型的宣告類型和值, 假設這項資訊已傳遞至同盟服務做為權杖要求的一部分:
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

呼叫者識別碼事件也會記錄活動識別碼, 讓您可以使用該活動識別碼來篩選或搜尋特定要求的事件記錄檔。




## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
