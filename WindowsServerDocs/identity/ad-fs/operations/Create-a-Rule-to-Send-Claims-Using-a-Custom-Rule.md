---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: 建立規則使用自訂規則傳送宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4c9d755dbcde3bb60bc418150061d2f58cf9f19e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967255"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>建立規則使用自訂規則傳送宣告


藉由在 Active Directory 同盟服務 (AD FS) 中使用 [**使用自訂規則來傳送宣告**] 範本，您可以針對標準規則範本無法滿足貴組織需求的情況，建立自訂宣告規則。 自訂宣告規則是以宣告規則語言撰寫，接著必須複製到 [**自訂規則**] 文字方塊中，才能用於規則集。 如需有關為「高級」規則建立語法的詳細資訊，請參閱宣告[規則語言的角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)。

您可以使用下列程式，使用 [AD FS 管理] 嵌入式管理單元來建立宣告規則 \- 。

若要完成此程式，至少需要本機電腦上之**Administrators**的成員資格或同等許可權。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>建立規則以在 Windows Server 2016 中的信賴憑證者信任上傳遞或篩選傳入宣告

1.  在 [伺服器管理員]**** 中按一下 [工具]****，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者**信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告發布原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  在 [**編輯宣告發布原則**] 對話方塊的 [**發行轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱。 在 [**自訂規則**] 底下，輸入或貼上您想要用於此規則的宣告規則語言語法。
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)

7.  按一下 [完成] 。

8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立規則，以在 Windows Server 2016 中的宣告提供者信任上傳遞或篩選傳入宣告

1.  在 [伺服器管理員]**** 中按一下 [工具]****，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**底下，按一下 [**宣告提供者信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  在 [**編輯宣告規則**] 對話方塊的 [**接受轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱。 在 [**自訂規則**] 底下，輸入或貼上您想要用於此規則的宣告規則語言語法。
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)

7.  按一下 [完成] 。

8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。



















## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>若要使用 Windows Server 2012 R2 中的自訂宣告建立規則以傳送宣告

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

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，從清單中選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)

6.  在 [**設定規則**] 頁面的 [宣告**規則名稱**] 下，輸入此規則的顯示名稱。 在 [**自訂規則**] 底下，輸入或貼上您想要用於此規則的宣告規則語言語法。
![建立規則](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)

7.  按一下 [完成] 。

8.  在 [**編輯宣告規則**] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="additional-references"></a>其他參考資料
[設定宣告規則](Configure-Claim-Rules.md)

[檢查清單：為信賴憑證者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[檢查清單：為宣告提供者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
