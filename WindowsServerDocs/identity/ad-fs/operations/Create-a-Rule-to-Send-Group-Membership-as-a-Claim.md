---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: 建立規則以宣告方式傳送群組成員資格
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 96ab653393fbc5f0a4306db53f84c2d9ba6c7f5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847449"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>建立規則以宣告方式傳送群組成員資格

>適用於：Windows Server 2016, Windows Server 2012 R2

做為宣告規則範本，在 Active Directory Federation Services 使用傳送群組成員資格\(AD FS\)，您可以建立一個規則，便可讓您選取 Active Directory 安全性群組，以宣告形式傳送。 這項規則，根據您選取的群組，將會發出單一宣告。 例如，您可以使用此規則範本來建立規則，它會傳送值是系統管理員的群組宣告，如果使用者是 Domain Admins 安全性群組的成員。 此規則只應該用於本機 Active Directory 網域中的使用者。  
  
您可以使用下列程序來建立與 AD FS 管理嵌入式管理單元的 宣告規則\-中。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則，以在信賴憑證者信任 Windows Server 2016 中以宣告形式傳送群組成員資格 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在上**選取規則範本**頁面的 **宣告規則範本**，選取**宣告形式傳送群組成員資格**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   在上**設定規則**頁面**宣告規則名稱**中輸入此規則的顯示名稱**使用者的群組**按一下**瀏覽**，然後選取群組底下**傳出宣告類型**選取所需的宣告型別，然後在 **傳出宣告類型**輸入的值。
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立宣告提供者信任 Windows Server 2016 中以宣告形式傳送群組成員資格規則 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在上**選取規則範本**頁面的 **宣告規則範本**，選取**宣告形式傳送群組成員資格**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   在上**設定規則**頁面**宣告規則名稱**中輸入此規則的顯示名稱**使用者的群組**按一下**瀏覽**，然後選取群組底下**傳出宣告類型**選取所需的宣告型別，然後在 **傳出宣告類型**輸入的值。 
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>若要建立 Windows Server 2012 R2 中以宣告形式傳送群組成員資格規則 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄中下, **AD FS\\信任關係**，按一下**宣告提供者信任**或是**信賴憑證者信任**，然後按一下 特定您要建立此規則的清單中的信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在 [**編輯宣告規則**] 對話方塊中，選取其中一個下列索引標籤，根據信任您要編輯，以及哪一個規則可設定您要建立，此規則，然後按一下**加入規則**啟動規則該規則集相關聯的精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  在上**選取規則範本**頁面的 **宣告規則範本**，選取**以宣告形式傳送群組成員資格**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  在上**設定規則**頁面**宣告規則名稱**中輸入此規則的顯示名稱**使用者的群組**按一下**瀏覽**，然後選取群組底下**傳出宣告類型**選取所需的宣告型別，然後在 **傳出宣告類型**輸入的值。  
![建立規則](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  按一下 **[完成]**。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  



## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：建立信賴憑證者信任宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：建立宣告規則的宣告提供者信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
