---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: 建立規則轉換傳入宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3d082e64cca70866da695582a1ac6a7b46d4fef8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965620"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>建立規則轉換傳入宣告


藉由使用 Active Directory 同盟服務 AD FS 中的 [**轉換傳入**宣告規則] 範本 \( \) ，您可以選取傳入宣告、變更其宣告類型，以及變更其宣告值。 例如，您可以使用此規則範本來建立規則，以使用傳入群組宣告的相同宣告值來傳送角色宣告。 當具有 [系統管理員] 值的傳入群組宣告，或者您只能傳送以結尾的使用者主要名稱 UPN 宣告時，您也可以使用此規則來傳送宣告值為 [購買者] 的群組宣告 \( \) @fabrikam 。  
  
您可以使用下列程式，利用 AD FS 管理嵌入式管理單元來建立宣告規則 \- 。  
  
若要完成此程式，至少需要本機電腦上之**Administrators**的成員資格或同等許可權。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>建立規則以在 Windows Server 2016 中轉換信賴憑證者信任上的傳入宣告 

1.  在 [伺服器管理員]**** 中按一下 [工具]****，然後選取 [AD FS 管理]。  
  
2.  在主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者**信任**]。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告發布原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 [**編輯宣告發布原則**] 對話方塊的 [**發行轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**轉換傳入**宣告]，然後按 **[下一步]**。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱。 在 [**傳入宣告類型**] 中，選取清單中的宣告類型。 在 [**傳出宣告類型**] 中，選取清單中的宣告類型，然後選取下列其中一個選項，這取決於您組織的需求：  
  
    -   **傳遞所有宣告值**  
  
    -   **以不同的傳出宣告值取代傳入宣告值**  
  
    -   **\-以新的電子郵件尾碼取代傳入的電子郵件尾碼宣告 \-**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  按一下 [完成]**** 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。
  
> [!NOTE]  
> 如果您要設定使用 AD FS 發行之宣告的動態存取控制案例 \- ，請先在宣告提供者信任上建立轉換規則，然後在 [**傳入宣告類型**] 中輸入連入宣告的名稱，或者，如果先前已建立宣告描述，請從清單中選取它。 其次，在 [**傳出宣告類型**] 中，選取您想要的宣告 URL，然後在信賴憑證者信任上建立轉換規則，以發出裝置宣告。  
>   
> 如需動態存取控制案例的詳細資訊，請參閱[動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或搭配[使用 AD DS 宣告與 AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831504(v=ws.11))。 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要在 Windows Server 2016 中建立規則以轉換宣告提供者信任上的傳入宣告 
  
1.  在 [伺服器管理員]**** 中按一下 [工具]****，然後選取 [AD FS 管理]。  
  
2.  在主控台樹的 [ **AD FS**底下，按一下 [**宣告提供者信任**]。 
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 [**編輯宣告規則**] 對話方塊的 [**接受轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**轉換傳入**宣告]，然後按 **[下一步]**。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱。 在 [**傳入宣告類型**] 中，選取清單中的宣告類型。 在 [**傳出宣告類型**] 中，選取清單中的宣告類型，然後選取下列其中一個選項，這取決於您組織的需求：  
  
    -   **傳遞所有宣告值**  
  
    -   **以不同的傳出宣告值取代傳入宣告值**  
  
    -   **\-以新的電子郵件尾碼取代傳入的電子郵件尾碼宣告 \-**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  按一下 [完成]**** 按鈕。  
  
8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。  

> [!NOTE]  
> 如果您要設定使用 AD FS 發行之宣告的動態存取控制案例 \- ，請先在宣告提供者信任上建立轉換規則，然後在 [**傳入宣告類型**] 中輸入連入宣告的名稱，或者，如果先前已建立宣告描述，請從清單中選取它。 其次，在 [**傳出宣告類型**] 中，選取您想要的宣告 URL，然後在信賴憑證者信任上建立轉換規則，以發出裝置宣告。  
>   
> 如需動態存取控制案例的詳細資訊，請參閱[動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或搭配[使用 AD DS 宣告與 AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831504(v=ws.11))。   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>建立規則以轉換 Windows Server 2012 R2 中的連入宣告 
  
1.  在伺服器管理員中，按一下 [**工具**]，然後按一下 [ **AD FS 管理**]。  
  
2.  在主控台樹的 [ **AD FS \\ 信任關聯**性] 底下，按一下 [**宣告提供者信任**] 或 [信賴憑證者**信任**]，然後在清單中按一下您想要建立此規則的特定信任。  
  
3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告規則**]。  
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  在 [**編輯宣告規則**] 對話方塊中，選取下列其中一個索引標籤，這取決於您正在編輯的信任，以及您要建立此規則的規則集，然後按一下 [**新增規則**] 來啟動與該規則集相關聯的規則嚮導：  
  
    -   **接受轉換規則**  
  
    -   **發行轉換規則**  
  
    -   **發佈授權規則**  
  
    -   **委派授權規則**  
![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**轉換傳入**宣告]，然後按 **[下一步]**。  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱。 在 [**傳入宣告類型**] 中，選取清單中的宣告類型。 在 [**傳出宣告類型**] 中，選取清單中的宣告類型，然後選取下列其中一個選項，這取決於您組織的需求：  
  
    -   **傳遞所有宣告值**  
  
    -   **以不同的傳出宣告值取代傳入宣告值**  
  
    -   **\-以新的電子郵件尾碼取代傳入的電子郵件尾碼宣告 \-**  
![建立規則](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> 如果您要設定使用 AD FS 發行之宣告的動態存取控制案例 \- ，請先在宣告提供者信任上建立轉換規則，然後在 [**傳入宣告類型**] 中輸入連入宣告的名稱，或者，如果先前已建立宣告描述，請從清單中選取它。 其次，在 [**傳出宣告類型**] 中，選取您想要的宣告 URL，然後在信賴憑證者信任上建立轉換規則，以發出裝置宣告。  
>   
> 如需動態存取控制案例的詳細資訊，請參閱[動態存取控制內容藍圖](../../solution-guides/dynamic-access-control--scenario-overview.md)或搭配[使用 AD DS 宣告與 AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831504(v=ws.11))。  
  
7. 按一下 [完成] 。  
  
8. 在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。  

## <a name="additional-references"></a>其他參考 
[設定宣告規則](Configure-Claim-Rules.md)  
 
[檢查清單：為信賴憑證者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))  

[檢查清單：為宣告提供者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))  
  
[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
