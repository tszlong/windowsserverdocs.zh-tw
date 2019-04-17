---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: "建立為宣告傳送 LDAP 屬性規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>建立為宣告傳送 LDAP 屬性規則

>適用於：Windows Server 2016、Windows Server 2012 R2

在 Active Directory 同盟服務 \(AD FS\) 宣告規則範本為使用傳送 LDAP 屬性，您可以建立將會從輕量型 Directory 存取通訊協定 \(LDAP\) 屬性市集中，例如為宣告信賴來傳送給 Active Directory 中選取屬性規則。 例如，您可以使用此規則範本建立傳送 LDAP 屬性宣告規則，將會解壓縮驗證使用者屬性的值為**顯示名稱**和**telephoneNumber** Active Directory 屬性，並為有兩個不同的傳出宣告將這些值。  
  
您也可以使用此規則傳送給所有使用者的群組成員資格。 如果您想要傳送僅限個人群組成員資格，作為理賠要求規則範本傳送群組成員資格。 您可以使用下列程序，以建立 AD FS 管理 snap\ 中理賠要求規則。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>建立為索賠可以方信任 Windows Server 2016 傳送 LDAP 屬性規則 

1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FS**，按一下 [**做為基礎的派對信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯宣告發行原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**編輯宣告發行原則**對話方塊中，在**發行轉換規則**按**新增規則**以開始規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，請選取**以宣告傳送 LDAP 屬性**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  在**設定規則**在頁面上**理賠要求規則名稱**輸入顯示名稱此規則，選取**屬性市集**，然後選取 LDAP 屬性，並將它對應至傳出宣告類型。 
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>若要建立為適用於 Windows Server 2016 宣告提供者信任宣告傳送 LDAP 屬性規則 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在主控台在**AD FS**，按一下 [**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**編輯理賠要求規則**對話方塊中，在**接受轉換規則**按**[新增規則**開始規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，請選取**以宣告傳送 LDAP 屬性**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  在**設定規則**在頁面上**理賠要求規則名稱**輸入顯示名稱此規則，選取**屬性市集**，然後選取 LDAP 屬性，並將它對應至傳出宣告類型。 
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>建立與 Windows Server 2012 R2 的宣告傳送 LDAP 屬性規則  
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  主控台中在**AD FSAD FS\\Trust 關係**，按一下**宣告提供者信任**或**可以廠商信任**，，然後按一下 [特定信任在清單中您想要用來建立本規則。  
  
3.  Right\ 按一下信任選取，然後再按一下**編輯理賠要求規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在**編輯理賠要求規則**對話方塊中，選取其中一個下列索引標籤，根據您正在編輯，並設定您的規則信任想要建立單元，此規則，然後按一下**新增規則**以開始規則該組相關聯的規則精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發行授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  在**選取 [規則範本**頁面上，在**理賠要求規則範本**，請選取**以宣告傳送 LDAP 屬性**從清單中，然後按一下**下一步**。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  在**設定規則**在頁面上**理賠要求規則名稱**底下輸入顯示名稱，則本規則**屬性網上商店**選取**Active Directory**，並在**對應的 LDAP 屬性，傳出宣告類型**選取想要的**LDAP 屬性**和對應**傳出宣告輸入**drop\ 下拉式清單的類型。  
  
    您必須選取新 LDAP 屬性和傳出宣告輸入配對不同的資料列每個您想要這個規則的一部分發行的理賠要求的 Active Directory 屬性。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  按一下**完成**按鈕。  
  
8.  在**編輯理賠要求規則**對話方塊中，按**[確定]**來儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定理賠要求規則](Configure-Claim-Rules.md)  
 
[檢查清單︰ 建立信賴的派對信任理賠要求規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單︰ 建立理賠要求規則宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權理賠要求規則](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
