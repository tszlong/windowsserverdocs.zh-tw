---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: 建立規則來以宣告方式傳送 LDAP 屬性
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6280ee1fb761b154cadb948d18c7b67cb6b8e784
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816651"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>建立規則來以宣告方式傳送 LDAP 屬性


在 Active Directory 同盟服務 \(AD FS\)中使用 [以宣告方式傳送 LDAP 屬性] 規則範本，您可以建立規則，從輕量型目錄存取協定中選取屬性，\(LDAP\) 屬性存放區（例如 Active Directory），以宣告方式傳送給信賴憑證者。 例如，您可以使用此規則範本來建立傳送 LDAP 屬性作為宣告規則，以從**displayName**和**telephoneNumber** Active Directory 屬性中，將已驗證使用者的屬性值解壓縮，然後將這些值當做兩個不同的傳出宣告來傳送。  
  
您也可以使用此規則來傳送所有使用者的群組成員資格。 如果您只想要傳送個別的群組成員資格，請使用「以宣告方式傳送群組成員資格」規則範本。 您可以使用下列程式，利用中的 [AD FS 管理] 嵌入式\-管理單元來建立宣告規則。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>建立規則以傳送 LDAP 屬性，做為 Windows Server 2016 中信賴憑證者信任的宣告 

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者**信任**]。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告發布原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 [**編輯宣告發布原則**] 對話方塊的 [**發行轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [從清單**傳送 LDAP 屬性做為宣告**]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，選取 [**屬性存放區**]，然後選取 LDAP 屬性並將它對應至 [傳出宣告類型]。 
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  按一下 [**完成]** 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>建立規則以傳送 LDAP 屬性，做為 Windows Server 2016 中的宣告提供者信任的宣告 
  
1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在主控台樹的  **AD FS**底下，按一下 **宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 [**編輯宣告規則**] 對話方塊的 [**接受轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [從清單**傳送 LDAP 屬性做為宣告**]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，選取 [**屬性存放區**]，然後選取 LDAP 屬性並將它對應至 [傳出宣告類型]。 
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  按一下 [**完成]** 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>建立規則以傳送 LDAP 屬性做為 Windows Server 2012 R2 的宣告  
  
1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  
  
2.  在主控台樹的 [ **AD FSAD FS\\信任關係**] 底下，按一下 [**宣告提供者信任**] 或 [信賴憑證者**信任**]，然後在清單中按一下您想要建立此規則的特定信任。  
  
3.  以滑鼠\-按右鍵選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在 [**編輯宣告規則**] 對話方塊中，根據您要編輯的信任以及要在其中建立此規則的規則集，選取下列其中一個索引標籤，然後按一下 [**新增規則**]，啟動與該規則集相關聯的規則嚮導：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [從清單**傳送 LDAP 屬性做為宣告**]，然後按 **[下一步]** 。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱，然後在 [**屬性存放區**] 下選取 [ **Active Directory**]，然後在 [ **LDAP 屬性與傳出宣告類型**的對應] 下，從\-下拉式清單中選取所需的**ldap 屬性**和對應的**輸出宣告類型**類型。  
  
    您必須針對要發出宣告的每個 Active Directory 屬性，在不同的資料列上選取新的 LDAP 屬性和傳出宣告類型組，做為此規則的一部分。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  按一下 [**完成]** 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：建立信賴憑證者信任的宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：建立宣告提供者信任的宣告規則](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
