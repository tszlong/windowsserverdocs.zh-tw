---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: 建立規則使用自訂規則傳送宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ade2a8304288d102608c81a0c29155478e5a4b7b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189428"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>建立規則使用自訂規則傳送宣告


藉由使用**使用自訂規則傳送宣告**Active Directory Federation Services (AD FS) 中的範本，您可以建立自訂宣告規則的標準規則範本不符合需求的情況下您組織。 自訂宣告規則以宣告規則語言撰寫，並必須複製到**自訂規則**才能使用規則集內的文字方塊。 如需建構進階規則的語法資訊，請參閱 < [The Role of 宣告規則語言](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)。  
  
您可以使用下列程序，若要使用 AD FS 管理嵌入式管理單元建立宣告規則\-中。  
  
中的成員資格**系統管理員**，或同等權限，在本機電腦上完成此程序的最低需求。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則來傳遞或篩選傳入宣告在信賴憑證者信任中 Windows Server 2016 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在 **設定規則**頁面的 **宣告規則名稱**，輸入此規則的顯示名稱。 底下**自訂規則**中，輸入或貼上您想為此規則的宣告規則語言語法。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  按一下 **[完成]** 。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立為通過或篩選傳入宣告在 Windows Server 2016 中的提供者信任宣告規則 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在 **設定規則**頁面的 **宣告規則名稱**，輸入此規則的顯示名稱。 底下**自訂規則**中，輸入或貼上您想為此規則的宣告規則語言語法。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  按一下 **[完成]** 。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>若要建立規則，以使用 Windows Server 2012 R2 中的自訂宣告傳送宣告 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後按一下**AD FS 管理**。  
  
2.  在主控台樹狀目錄中下, **AD FS\\信任關係**，按一下**宣告提供者信任**或是**信賴憑證者信任**，然後按一下 特定您要建立此規則的清單中的信任。  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  在 **編輯宣告規則**對話方塊中，選取其中一個下列索引標籤中，這取決於您要編輯，並在哪一個規則中設定您的信任要建立這項規則，然後按一下**新增規則**啟動規則該規則集相關聯的精靈：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**使用自訂規則傳送宣告**從清單中，然後按一下 **下一步**.  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  在 **設定規則**頁面的 **宣告規則名稱**，輸入此規則的顯示名稱。 底下**自訂規則**中，輸入或貼上您想為此規則的宣告規則語言語法。  
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  按一下 **[完成]** 。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：為信賴憑證者信任建立宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：為宣告提供者信任建立宣告規則](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
