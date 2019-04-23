---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: 建立規則，以宣告形式傳送 LDAP 屬性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887609"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>建立規則，以宣告形式傳送 LDAP 屬性

>適用於：Windows Server 2016, Windows Server 2012 R2

使用傳送 LDAP 屬性做為 Active Directory Federation Services 中的宣告規則範本\(AD FS\)，您可以建立會選取從輕量型目錄存取通訊協定的屬性規則\(LDAP\)屬性存放區，例如 Active Directory，以傳送宣告給信賴憑證者的合作對象。 比方說，您可以使用此規則範本來建立宣告規則，就會擷取屬性值已驗證的使用者，從 a Send LDAP Attributes **displayName**並**telephoneNumber** Active目錄屬性，並接著將這些值傳送為兩個不同的連出宣告。  
  
您也可以使用此規則來傳送使用者的所有群組成員資格。 如果您只想要傳送個別的群組成員資格，請使用「以宣告方式傳送群組成員資格」規則範本。 您可以使用下列程序來建立與 AD FS 管理嵌入式管理單元的 宣告規則\-中。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則以針對信賴憑證者信任 Windows Server 2016 中的宣告形式傳送 LDAP 屬性 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**以宣告方式傳送 LDAP 屬性**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  在上**設定規則**頁面**宣告規則名稱**輸入顯示名稱，此規則選取**屬性存放區**，然後選取 LDAP 屬性，並將它對應至傳出宣告類型。 
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>若要建立規則以針對宣告提供者信任 Windows Server 2016 中的宣告形式傳送 LDAP 屬性 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**以宣告方式傳送 LDAP 屬性**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  在上**設定規則**頁面**宣告規則名稱**輸入顯示名稱，此規則選取**屬性存放區**，然後選取 LDAP 屬性，並將它對應至傳出宣告類型。 
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>若要建立以 Windows Server 2012 r2 的宣告形式傳送 LDAP 屬性的規則  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄中，在**AD FSAD FS\\信任關係**，按一下**宣告提供者信任**或**信賴憑證者信任**，然後按一下在您要建立此規則的清單中的特定信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在 [**編輯宣告規則**] 對話方塊中，選取其中一個下列索引標籤，根據信任您要編輯，以及哪一個規則可設定您要建立，此規則，然後按一下**加入規則**啟動規則該規則集相關聯的精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**以宣告方式傳送 LDAP 屬性**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  在上**設定規則**頁面**宣告規則名稱**下輸入這項規則的顯示名稱**屬性存放區**選取**Active Directory**，然後在**對應的 LDAP 屬性與傳出宣告類型**選取想要**LDAP 屬性**對應**傳出宣告類型**從下拉式清單的型別\-清單。  
  
    您必須選取新的 LDAP 屬性與傳出宣告類型組不同的資料列，每個您想要發出的宣告，此規則的一部分的 Active Directory 屬性。  
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：建立信賴憑證者信任宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：建立宣告規則的宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
