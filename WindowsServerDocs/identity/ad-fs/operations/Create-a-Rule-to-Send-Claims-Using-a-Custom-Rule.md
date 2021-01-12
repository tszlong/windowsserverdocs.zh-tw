---
description: 深入瞭解：使用自訂規則建立用來傳送宣告的規則
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: 建立規則使用自訂規則傳送宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5f1bb307565ce29defd918f8da606ee058113baf
ms.sourcegitcommit: 6a62d736e4d9989515c6df85e2577662deb042b6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98103860"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>建立規則使用自訂規則傳送宣告


藉由在 Active Directory 同盟服務 (AD FS) 中使用 **自訂規則範本的傳送宣告** ，您可以針對標準規則範本不符合組織需求的情況建立自訂宣告規則。 自訂宣告規則會以宣告規則語言撰寫，然後必須複製到 [ **自訂規則** ] 文字方塊中，才能在規則集中使用。 如需有關建立 advanced rule 語法的詳細資訊，請參閱宣告 [規則語言的角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)。

您可以使用下列程式，利用 AD FS 管理] 嵌入式管理單元來建立宣告規則 \- 。

若要完成此程式，至少需要本機電腦上 **Administrators** 的成員資格或同等許可權。  請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則以在 Windows Server 2016 的信賴憑證者信任中通過或篩選傳入宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [信賴憑證者 **信任**]。
![當您在 Windows Server 2016 的信賴憑證者信任上建立規則以通過或篩選傳入宣告時，顯示在主控台樹中選取 [信賴憑證者信任] 的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告發行原則**]。
![螢幕擷取畫面，顯示當您建立規則以在 Windows Server 2016 中通過或篩選信賴憑證者信任的傳入宣告時，[編輯宣告發行原則] 功能表選項的選取位置。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  在 [ **編輯宣告發行原則** ] 對話方塊的 [ **發佈轉換規則** ] 下，按一下 [ **新增規則** ] 以啟動規則嚮導。
![螢幕擷取畫面，顯示當您在 Windows Server 2016 的信賴憑證者信任上建立規則以傳遞或篩選傳入宣告時，要在哪裡選取 [新增規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **使用自訂規則傳送宣告** ]，然後按 **[下一步]**。
![螢幕擷取畫面，顯示當您建立規則以在 Windows Server 2016 的信賴憑證者信任中通過或篩選傳入宣告時，使用自訂規則來選取傳送宣告的位置。](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱**] 底下，輸入此規則的顯示名稱。 在 [ **自訂規則**] 下，輸入或貼上您想要用於此規則的宣告規則語言語法。
![顯示宣告規則名稱之類型的螢幕擷取畫面。](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)

7.  按一下 [完成] 。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立規則，以在 Windows Server 2016 中的宣告提供者信任上傳遞或篩選傳入宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [ **宣告提供者信任**]。
![螢幕擷取畫面，顯示當您建立規則以在 Windows Server 2016 中通過或篩選宣告提供者信任的傳入宣告時，要在哪裡選取宣告提供者信任](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![螢幕擷取畫面，顯示當您建立規則以在 Windows Server 2016 中的宣告提供者信任上傳遞或篩選傳入宣告時，要在哪裡選取 [編輯宣告規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 [ **接受轉換規則** ] 下的 [ **新增規則** ]，以啟動規則嚮導。
![螢幕擷取畫面，顯示當您建立規則以在 Windows Server 2016 中的宣告提供者信任上傳遞或篩選傳入宣告時，要在哪裡選取 [新增規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **使用自訂規則傳送宣告** ]，然後按 **[下一步]**。
![螢幕擷取畫面，其中顯示當您建立規則以在 Windows Server 2016 中的宣告提供者信任上傳遞或篩選傳入宣告時，使用自訂規則範本來選取傳送宣告。](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱**] 底下，輸入此規則的顯示名稱。 在 [ **自訂規則**] 下，輸入或貼上您想要用於此規則的宣告規則語言語法。
![螢幕擷取畫面，顯示當您在 Windows Server 2016 的宣告提供者信任上建立規則以傳遞或篩選傳入宣告時，要在哪裡輸入宣告規則名稱。](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)

7.  按一下 [完成] 。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。



















## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>若要建立規則以在 Windows Server 2012 R2 中使用自訂宣告來傳送宣告

1.  在伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **AD FS 管理**]。

2.  在主控台樹的 **AD FS \\ 信任關係**] 下，按一下 [ **宣告提供者信任** ] 或 [信賴憑證者 **信任**]，然後在清單中按一下您要在其中建立此規則的特定信任。

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![螢幕擷取畫面，顯示當您在 Windows Server 2012 R2 中使用自訂宣告建立規則以傳送宣告時，要在哪裡選取 [編輯宣告規則]。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，選取下列其中一個索引標籤，這取決於您正在編輯的信任，以及您想要建立此規則的規則集，然後按一下 [ **新增規則** ] 以啟動與該規則集相關聯的規則 wizard：

    -   **接受轉換規則**

    -   **發行轉換規則**

    -   **發佈授權規則**

    -   **委派授權規則** 
 ![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **使用自訂規則傳送宣告** ]，然後按 **[下一步]**。
![螢幕擷取畫面，顯示當您在 Windows Server 2012 R2 中使用自訂宣告來建立規則以傳送宣告時，使用自訂規則範本來選取傳送宣告的位置。](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱**] 底下，輸入此規則的顯示名稱。 在 [ **自訂規則**] 下，輸入或貼上您想要用於此規則的宣告規則語言語法。
![螢幕擷取畫面，顯示當您在 Windows Server 2012 R2 中使用自訂宣告建立規則時，要在哪裡輸入宣告規則名稱。](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)

7.  按一下 [完成] 。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="additional-references"></a>其他參考資料
[設定宣告規則](Configure-Claim-Rules.md)

[檢查清單：為信賴憑證者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[檢查清單：為宣告提供者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
