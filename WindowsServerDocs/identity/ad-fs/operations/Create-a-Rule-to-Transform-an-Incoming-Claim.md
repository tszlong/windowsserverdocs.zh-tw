---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: 建立規則轉換傳入宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6bd107aca6c6f33cdf5f88e5b48a52fdea8d2086
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189339"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>建立規則轉換傳入宣告


藉由使用**轉換傳入宣告**規則範本，在 Active Directory Federation Services \(AD FS\)，您可以選取 傳入宣告、 變更其宣告類型，並變更其宣告值。 例如，您可以使用此規則範本來建立會傳送具有相同的宣告值，連入群組宣告的角色宣告的規則。 您也可以使用這項規則將連入群組宣告值是系統管理員，或您可以傳送使用者主體名稱時，群組宣告宣告值是以下\(UPN\)結尾的宣告@fabrikam。  
  
您可以使用下列程序來建立與 AD FS 管理嵌入式管理單元的 宣告規則\-中。  
  
中的成員資格**系統管理員**，或同等權限，在本機電腦上完成此程序的最低需求。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則來轉換傳入宣告在信賴憑證者信任中 Windows Server 2016 

1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**信賴憑證者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告發佈原則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 **編輯宣告發佈原則**對話方塊的 **發佈轉換規則**按一下 **新增規則**啟動規則精靈。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步** .  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 **設定規則**頁面的 **宣告規則名稱**，輸入此規則的顯示名稱。 在 **傳入宣告類型**，在清單中選取的宣告類型。 在 **連出的宣告型別**在清單中，選取 宣告類型，然後選取下列選項，取決於您的組織需求的其中一個：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **將內送電子\-郵件後置詞宣告新的電子\-郵件尾碼**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。
  
> [!NOTE]  
> 如果您要設定動態存取控制案例中，使用 AD FS\-發出宣告，在宣告提供者信任，並在第一次建立轉換規則**傳入宣告類型**，輸入的名稱連入宣告，或者，如果宣告描述先前已建立，請從清單中選取。 第二個，在**連出的宣告型別**選取您想要宣告 URL，然後建立信賴憑證者信任上發出的裝置宣告的轉換規則。  
>   
> 如需有關動態存取控制案例的詳細資訊，請參閱 <<c0> [ 動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或是[搭配 AD FS 使用 AD DS 宣告](https://technet.microsoft.com/library/hh831504.aspx)。 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立來轉換傳入宣告在 Windows Server 2016 中的提供者信任宣告規則 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在主控台樹狀目錄之下**AD FS**，按一下**宣告提供者信任**。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-按一下 選取的信任，然後按一下**編輯宣告規則**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 **編輯宣告規則**對話方塊的 **接受轉換規則**按一下 **新增規則**啟動規則精靈。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步** .  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 **設定規則**頁面的 **宣告規則名稱**，輸入此規則的顯示名稱。 在 **傳入宣告類型**，在清單中選取的宣告類型。 在 **連出的宣告型別**在清單中，選取 宣告類型，然後選取下列選項，取決於您的組織需求的其中一個：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **將內送電子\-郵件後置詞宣告新的電子\-郵件尾碼**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  按一下 [**完成**] 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

> [!NOTE]  
> 如果您要設定動態存取控制案例中，使用 AD FS\-發出宣告，在宣告提供者信任，並在第一次建立轉換規則**傳入宣告類型**，輸入的名稱連入宣告，或者，如果宣告描述先前已建立，請從清單中選取。 第二個，在**連出的宣告型別**選取您想要宣告 URL，然後建立信賴憑證者信任上發出的裝置宣告的轉換規則。  
>   
> 如需有關動態存取控制案例的詳細資訊，請參閱 <<c0> [ 動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或是[搭配 AD FS 使用 AD DS 宣告](https://technet.microsoft.com/library/hh831504.aspx)。   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>若要建立規則來轉換傳入宣告在 Windows Server 2012 R2 
  
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
  
5.  在 **選取規則範本**頁面的 **宣告規則範本**，選取**傳輸傳入宣告**從清單中，然後按一下 **下一步** .  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  在 **設定規則**頁面的 **宣告規則名稱**，輸入此規則的顯示名稱。 在 **傳入宣告類型**，在清單中選取的宣告類型。 在 **連出的宣告型別**在清單中，選取 宣告類型，然後選取下列選項，取決於您的組織需求的其中一個：  
  
    -   **通過所有宣告值**  
  
    -   **使用不同的傳出宣告值取代傳入宣告值**  
  
    -   **將內送電子\-郵件後置詞宣告新的電子\-郵件尾碼**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> 如果您要設定動態存取控制案例中，使用 AD FS\-發出宣告，在宣告提供者信任，並在第一次建立轉換規則**傳入宣告類型**，輸入的名稱連入宣告，或者，如果宣告描述先前已建立，請從清單中選取。 第二個，在**連出的宣告型別**選取您想要宣告 URL，然後建立信賴憑證者信任上發出的裝置宣告的轉換規則。  
>   
> 如需有關動態存取控制案例的詳細資訊，請參閱 <<c0> [ 動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或是[搭配 AD FS 使用 AD DS 宣告](https://technet.microsoft.com/library/hh831504.aspx)。  
  
7.  按一下 **[完成]** 。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下**確定**儲存規則。  

## <a name="additional-references"></a>其他參考資料 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：為信賴憑證者信任建立宣告規則](https://technet.microsoft.com/library/ee913578.aspx)  

[檢查清單：為宣告提供者信任建立宣告規則](https://technet.microsoft.com/library/ee913564.aspx)  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
