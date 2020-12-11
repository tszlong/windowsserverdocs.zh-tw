---
description: 深入瞭解：建立規則以傳遞或篩選傳入宣告
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: 建立規則以傳遞或篩選傳入宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 235d2bf0572e2b84536d09a1bf9784d635db3ad5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048216"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>建立規則以傳遞或篩選傳入宣告

使用 Active Directory 同盟服務 AD FS 中的 [傳遞或篩選內送宣告規則] 範本 \( \) ，您可以傳遞所有具有所選宣告類型的傳入宣告。 您也可以篩選包含所選宣告類型的內送宣告值。 例如，您可以使用此規則範本來建立將傳送所有內送群組宣告的規則。 您也可以使用此規則，只傳送以結尾的使用者主體名稱 \( UPN \) 宣告 @fabrikam 。

您可以使用下列程式，利用 AD FS 管理] 嵌入式管理單元來建立宣告規則 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則以在 Windows Server 2016 的信賴憑證者信任中通過或篩選傳入宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [信賴憑證者 **信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告發行原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  在 [ **編輯宣告發行原則** ] 對話方塊的 [ **發佈轉換規則** ] 下，按一下 [ **新增規則** ] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **傳遞或篩選傳入** 宣告]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱** ] 下，輸入此規則的顯示名稱，在 [連 **入宣告類型** ] 中，選取清單中的 [宣告類型]，然後根據您的組織需求，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **僅傳遞特定宣告值**

    -   **只傳遞符合特定電子郵件尾碼值的宣告值**

    -   **只傳遞以特定值** 
 ![ 開頭的宣告值建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)

7.  按一下 [完成] 按鈕。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要建立規則，以在 Windows Server 2016 中的宣告提供者信任上傳遞或篩選傳入宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [ **宣告提供者信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 [ **接受轉換規則** ] 下的 [ **新增規則** ]，以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **傳遞或篩選傳入** 宣告]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱** ] 下，輸入此規則的顯示名稱，在 [連 **入宣告類型** ] 中，選取清單中的 [宣告類型]，然後根據您的組織需求，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **僅傳遞特定宣告值**

    -   **只傳遞符合特定電子郵件尾碼值的宣告值**

    -   **只傳遞以特定值** 
 ![ 開頭的宣告值建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)

7.  按一下 [完成] 按鈕。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>建立規則以在 Windows Server 2012 R2 中通過或篩選傳入宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FSAD FS \\ 信任關係**] 底下，按一下 [ **宣告提供者信任** ] 或 [信賴憑證者 **信任**]，然後在清單中按一下您要在其中建立此規則的特定信任。

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，根據您要編輯的信任和您要在其中建立此規則的規則集，選取下列其中一個索引標籤，然後按一下 [ **新增規則** ]，以啟動與該規則集相關聯的規則 wizard：

    -   **接受轉換規則**

    -   **發行轉換規則**

    -   **發佈授權規則**

    -   **委派授權規則** 
 ![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **傳遞或篩選傳入** 宣告]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱** ] 下，輸入此規則的顯示名稱，在 [連 **入宣告類型** ] 中，選取清單中的 [宣告類型]，然後根據您的組織需求，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **僅傳遞特定宣告值**

    -   **只傳遞符合特定電子郵件尾碼值的宣告值**

    -   **只傳遞以特定值** 
 ![ 開頭的宣告值建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)

7.  按一下 [完成] 按鈕。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。




## <a name="additional-references"></a>其他參考資料
[設定宣告規則](Configure-Claim-Rules.md)

[使用傳遞或篩選宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)

