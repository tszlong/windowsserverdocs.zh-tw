---
description: 深入瞭解：建立規則以將 LDAP 屬性作為宣告傳送
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: 建立規則以傳送 LDAP 屬性作為宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6b6cddaa5df89e16f77022b41400be75c2bd67d5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048076"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>建立規則以傳送 LDAP 屬性作為宣告


使用 Active Directory 同盟服務 AD FS 中的 [傳送 LDAP 屬性作為宣告規則] 範本 \( \) ，您可以建立規則來選取輕量目錄存取通訊協定 \( LDAP \) 屬性存放區（例如 Active Directory）中的屬性，以將宣告傳送給信賴憑證者。 例如，您可以使用此規則範本建立傳送 LDAP 屬性作為宣告規則，以從 **displayName** 和 **telephoneNumber** Active Directory 屬性解壓縮已驗證使用者的屬性值，然後將這些值以兩個不同的傳出宣告來傳送。

您也可以使用此規則來傳送所有使用者的群組成員資格。 如果您只想要傳送個別的群組成員資格，請使用「以宣告方式傳送群組成員資格」規則範本。 您可以使用下列程式，利用 AD FS 管理] 嵌入式管理單元來建立宣告規則 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>若要建立規則，以在 Windows Server 2016 中將 LDAP 屬性傳送為信賴憑證者信任的宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [信賴憑證者 **信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告發行原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  在 [ **編輯宣告發行原則** ] 對話方塊的 [ **發佈轉換規則** ] 下，按一下 [ **新增規則** ] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **以宣告方式傳送 LDAP 屬性** ]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱** ] 下，輸入此規則的顯示名稱，選取 [ **屬性存放區**]，然後選取 [LDAP] 屬性，並將其對應至傳出宣告類型。
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)

7.  按一下 [完成] 按鈕。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>若要建立規則以傳送 LDAP 屬性作為 Windows Server 2016 中宣告提供者信任的宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [ **宣告提供者信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 [ **接受轉換規則** ] 下的 [ **新增規則** ]，以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **以宣告方式傳送 LDAP 屬性** ]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱** ] 下，輸入此規則的顯示名稱，選取 [ **屬性存放區**]，然後選取 [LDAP] 屬性，並將其對應至傳出宣告類型。
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)

7.  按一下 [完成] 按鈕。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。



## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>建立規則以傳送 LDAP 屬性作為 Windows Server 2012 R2 的宣告

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FSAD FS \\ 信任關係**] 底下，按一下 [ **宣告提供者信任** ] 或 [信賴憑證者 **信任**]，然後在清單中按一下您要在其中建立此規則的特定信任。

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，選取下列其中一個索引標籤，根據您要編輯的信任以及您要在其中建立此規則的規則集，然後按一下 [ **新增規則** ]，以啟動與該規則集相關聯的規則 wizard：

    -   **接受轉換規則**

    -   **發行轉換規則**

    -   **發佈授權規則**

    -   **委派授權規則** 
 ![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，從清單中選取 [ **以宣告方式傳送 LDAP 屬性** ]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱** ] 下，輸入此規則的顯示名稱，在 [ **屬性存放區** ] 下選取 [ **Active Directory**，然後在 [ **ldap 屬性與傳出宣告類型** 的對應] 下，從下拉式清單中選取所需的 **LDAP 屬性** 和對應的 **輸出宣告類型** 類型 \- 。

    您必須針對每個要發出宣告的 Active Directory 屬性，在不同的資料列上選取新的 LDAP 屬性和連出宣告類型組，作為此規則的一部分。
![建立規則](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)
7.  按一下 [完成] 按鈕。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="additional-references"></a>其他參考資料
[設定宣告規則](Configure-Claim-Rules.md)

[檢查清單：為信賴憑證者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[檢查清單：為宣告提供者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
