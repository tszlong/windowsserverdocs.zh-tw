---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: 建立規則來傳遞或篩選傳入宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860269"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>建立規則來傳遞或篩選傳入宣告

>適用於：Windows Server 2016, Windows Server 2012 R2

使用 「 傳遞或篩選傳入宣告 」 規則範本在 Active Directory Federation Services \(AD FS\)，您可以通過所有的連入宣告與所選的宣告類型。 您也可以篩選連入宣告的值與所選的宣告類型。 例如，您可以使用此規則範本來建立規則，它會傳送所有連入群組宣告。 您也可以使用這項規則，將使用者主體名稱\(UPN\)結尾的宣告@fabrikam。  
  
您可以使用下列程序來建立與 AD FS 管理嵌入式管理單元的 宣告規則\-中。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則來傳遞或篩選傳入宣告在信賴憑證者信任中 Windows Server 2016 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳遞或篩選傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在上**設定規則**頁面**宣告規則名稱**中輸入此規則的顯示名稱**傳入宣告類型**在清單中，選取的宣告類型，然後再選取其中一個下列選項，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **僅特定宣告值傳遞**  
  
    -   **傳遞符合特定電子郵件尾碼值的宣告值**  
  
    -   **傳遞特定值開頭的宣告值**  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立為通過或篩選傳入宣告在 Windows Server 2016 中的提供者信任宣告規則 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳遞或篩選傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在上**設定規則**頁面**宣告規則名稱**中輸入此規則的顯示名稱**傳入宣告類型**在清單中，選取的宣告類型，然後再選取其中一個下列選項，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **僅特定宣告值傳遞**  
  
    -   **傳遞符合特定電子郵件尾碼值的宣告值**  
  
    -   **傳遞特定值開頭的宣告值**  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>若要建立規則來傳遞或篩選傳入宣告在 Windows Server 2012 R2

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄中，在**AD FSAD FS\\信任關係**，按一下**宣告提供者信任**或**信賴憑證者信任**，然後按一下在您要建立此規則的清單中的特定信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  在 [**編輯宣告規則**] 對話方塊中，選取其中一個下列索引標籤，根據信任您要編輯，以及哪一個規則可設定您要建立，此規則，然後按一下**加入規則**啟動規則精靈這就是該規則集相關聯：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳遞或篩選傳入宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  在上**設定規則**頁面**宣告規則名稱**中輸入此規則的顯示名稱**傳入宣告類型**在清單中，選取的宣告類型，然後再選取其中一個下列選項，視您組織的需求而定：  
  
    -   **通過所有宣告值**  
  
    -   **僅特定宣告值傳遞**  
  
    -   **傳遞符合特定電子郵件尾碼值的宣告值**  
  
    -   **傳遞特定值開頭的宣告值**  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  



  
## <a name="additional-references"></a>其他參考資料  
[設定宣告規則](Configure-Claim-Rules.md)  
  
[使用傳遞或篩選宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
