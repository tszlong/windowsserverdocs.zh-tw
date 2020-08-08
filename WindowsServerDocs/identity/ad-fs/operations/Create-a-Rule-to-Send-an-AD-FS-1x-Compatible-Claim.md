---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: 建立規則以傳送 AD FS 1.x 相容的宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4f46130129617113b8877b764bf081d0fb585e4a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967225"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>建立規則以傳送 AD FS 1.x 相容的宣告

當您使用 Active Directory 同盟服務 \( AD FS \) 來發行由執行 AD FS 1.0 \( Windows server 2003 R2 \) 或 AD FS 1.1 \( Windows server 2008 或 Windows server 2008 r2 的同盟伺服器所接收的宣告 \) 時，您必須執行下列動作：

-   建立規則，以使用 UPN、電子郵件或一般名稱的格式來傳送名稱識別碼宣告類型。

-   所有其他已傳送的宣告都必須具有下列其中一種類型：

    -   AD FS 1.*x* 電子郵件地址

    -   AD FS 1。*x* UPN

    -   一般名稱

    -   群組

    -   以為開頭的任何其他宣告類型 https://schemas.xmlsoap.org/claims/ ，例如https://schemas.xmlsoap.org/claims/EmployeeID

視組織的需求而定，請使用下列其中一個程式來建立 AD FS 1。*x*相容的 NameID 宣告：

-   建立此規則，以使用 [傳遞] 或 [篩選傳入宣告]**規則範本**來發行 AD FS 1.X 名稱識別碼宣告

-   建立此規則，使用「**轉換傳入宣告」規則範本**來發行 AD FS 1.X 名稱識別碼宣告。 如果您想要將現有的宣告類型變更為新的宣告類型，以便搭配 AD FS 1，您可以使用此規則範本。 *x*宣告。

> [!NOTE]
> 若要讓此規則如預期般運作，請確定您建立此規則的信賴憑證者信任或宣告提供者信任已設定為使用**AD FS 1.0 和1.1 設定檔**。

## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>建立規則以發出 AD FS 1。在 Windows Server 2016 的信賴憑證者信任上使用 [傳遞或篩選傳入宣告] 規則範本的*x*名稱識別碼宣告

1.  在 [伺服器管理員]**** 中按一下 [工具]****，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者**信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告發布原則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  在 [**編輯宣告發布原則**] 對話方塊的 [**發行轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [從清單**傳遞或篩選傳入**宣告]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。

7.  在 [**傳入宣告類型**] 中，選取清單中的 [**名稱識別碼**]。

8.  在 [**傳入名稱識別碼格式**] 中，選取下列其中一項 AD FS 1。*x* \-清單中的相容宣告格式：

    -   **UPN**

    -   **電子 \- 郵件**

    -   **一般名稱**

9. 視組織的需求而定，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **僅傳遞特定宣告值**

    -   **只傳遞符合特定電子郵件尾碼值的宣告值**

    -   **僅傳遞以特定值** 
 ![ 開頭的宣告值建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. 按一下 **[完成]**，然後按一下 **[確定]** 以儲存規則。


## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>建立規則以發出 AD FS 1。在 Windows Server 2016 中使用宣告提供者信任上的 [傳遞或篩選傳入宣告] 規則範本的*x*名稱識別碼宣告

1.  在 [伺服器管理員]**** 中按一下 [工具]****，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**底下，按一下 [**宣告提供者信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  在 [**編輯宣告規則**] 對話方塊的 [**接受轉換規則**] 底下，按一下 [**新增規則**] 以啟動規則嚮導。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [從清單**傳遞或篩選傳入**宣告]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。

7.  在 [**傳入宣告類型**] 中，選取清單中的 [**名稱識別碼**]。

8.  在 [**傳入名稱識別碼格式**] 中，選取下列其中一項 AD FS 1。*x* \-清單中的相容宣告格式：

    -   **UPN**

    -   **電子 \- 郵件**

    -   **一般名稱**

9. 視組織的需求而定，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **僅傳遞特定宣告值**

    -   **只傳遞符合特定電子郵件尾碼值的宣告值**

    -   **僅傳遞以特定值** 
 ![ 開頭的宣告值建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. 按一下 **[完成]**，然後按一下 **[確定]** 以儲存規則。


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

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。

7.  在 [**傳入宣告類型**] 中，選取您想要在清單中轉換的傳入宣告類型。

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**名稱識別碼**]。

9. 在 [**外寄名稱識別碼格式**] 中，選取下列其中一項 AD FS 1。*x* \-清單中的相容宣告格式：

    -   **UPN**

    -   **電子 \- 郵件**

    -   **一般名稱**

10. 視組織的需求而定，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **以不同的傳出宣告值取代傳入宣告值**

    -   ** \- 以新的電子 \- 郵件尾碼建立規則取代傳入的電子郵件尾碼宣告** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. 按一下 **[完成]**，然後按一下 **[確定]** 以儲存規則。




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

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。

7.  在 [**傳入宣告類型**] 中，選取您想要在清單中轉換的傳入宣告類型。

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**名稱識別碼**]。

9. 在 [**外寄名稱識別碼格式**] 中，選取下列其中一項 AD FS 1。*x* \-清單中的相容宣告格式：

    -   **UPN**

    -   **電子 \- 郵件**

    -   **一般名稱**

10. 視組織的需求而定，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **以不同的傳出宣告值取代傳入宣告值**

    -   ** \- 以新的電子 \- 郵件尾碼建立規則取代傳入的電子郵件尾碼宣告** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. 按一下 **[完成]**，然後按一下 **[確定]** 以儲存規則。














## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>建立規則以發出 AD FS 1。在 Windows Server 2012 R2 上使用 [傳遞或篩選傳入宣告] 規則範本的*x*名稱識別碼宣告

1.  在伺服器管理員中，按一下 [**工具**]，然後按一下 [ **AD FS 管理**]。

2.  在主控台樹的 [ **AD FS \\ 信任關聯**性] 底下，按一下 [**宣告提供者信任**] 或 [信賴憑證者**信任**]，然後在清單中按一下您想要建立此規則的特定信任。

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [**編輯宣告規則**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  在 [**編輯宣告規則**] 對話方塊中，選取下列其中一個索引標籤，視您正在編輯的信任和您想要在其中建立此規則的規則集，然後按一下 [**新增規則**] 來啟動與該規則集相關聯的規則嚮導：

    -   **接受轉換規則**

    -   **發行轉換規則**

    -   **發佈授權規則**

    -   **委派授權規則** 
 ![建立規則](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [**選取規則範本**] 頁面的 [宣告**規則範本**] 底下，選取 [從清單**傳遞或篩選傳入**宣告]，然後按 **[下一步]**。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。

7.  在 [**傳入宣告類型**] 中，選取清單中的 [**名稱識別碼**]。

8.  在 [**傳入名稱識別碼格式**] 中，選取下列其中一項 AD FS 1。*x* \-清單中的相容宣告格式：

    -   **UPN**

    -   **電子 \- 郵件**

    -   **一般名稱**

9. 視組織的需求而定，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **僅傳遞特定宣告值**

    -   **只傳遞符合特定電子郵件尾碼值的宣告值**

    -   **僅傳遞以特定值** 
 ![ 開頭的宣告值建立規則](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)

10. 按一下 **[完成]**，然後按一下 **[確定]** 以儲存規則。


## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>建立規則以發出 AD FS 1。使用 Windows Server 2012 R2 中的「轉換傳入宣告」規則範本的*x*名稱識別碼宣告

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

6.  在 [**設定規則**] 頁面上，輸入宣告規則名稱。

7.  在 [**傳入宣告類型**] 中，選取您想要在清單中轉換的傳入宣告類型。

8.  在 [**傳出宣告類型**] 中，選取清單中的 [**名稱識別碼**]。

9. 在 [**外寄名稱識別碼格式**] 中，選取下列其中一項 AD FS 1。*x* \-清單中的相容宣告格式：

    -   **UPN**

    -   **電子 \- 郵件**

    -   **一般名稱**

10. 視組織的需求而定，選取下列其中一個選項：

    -   **傳遞所有宣告值**

    -   **以不同的傳出宣告值取代傳入宣告值**

    -   ** \- 以新的電子 \- 郵件尾碼建立規則取代傳入的電子郵件尾碼宣告** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)

11. 按一下 **[完成]**，然後按一下 **[確定]** 以儲存規則。

## <a name="additional-references"></a>其他參考資料
[設定宣告規則](Configure-Claim-Rules.md)

[檢查清單：為信賴憑證者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[檢查清單：為宣告提供者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
