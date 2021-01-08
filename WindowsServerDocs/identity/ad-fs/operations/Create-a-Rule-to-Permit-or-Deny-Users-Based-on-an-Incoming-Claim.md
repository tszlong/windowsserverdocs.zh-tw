---
description: 深入瞭解：建立規則以根據連入宣告允許或拒絕使用者
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: 建立規則依據傳入宣告允許或拒絕使用者
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5bcfa3f73e3aede9917e7f5cb7412cca37728550
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039178"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>建立規則依據傳入宣告允許或拒絕使用者


在 Windows Server 2016 中，您可以使用 **存取控制原則** 來建立規則，以根據連入宣告允許或拒絕使用者。  在 Windows Server 2012 R2 中，使用 Active Directory 同盟服務 AD FS 中的 [根據連入宣告規則範本的 **允許或拒絕使用者** ] \( \) ，您可以建立授權規則，以根據連入宣告的類型和值，授與或拒絕使用者存取信賴憑證者。

例如，您可以使用此規則來建立規則，以允許只有具有網域管理員值的群組宣告，才能存取信賴憑證者。 如果您想要允許所有使用者存取信賴憑證者，請使用 [ **允許每個人** 存取控制原則] 或 [ **允許所有使用者** ] 規則範本（視您的 Windows Server 版本而定）。 從同盟服務獲准存取信賴憑證者的使用者，仍可能遭信賴憑證者拒絕服務。

您可以使用下列程式，利用 AD FS 管理] 嵌入式管理單元來建立宣告規則 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>建立規則以允許以 Windows Server 2016 上的連入宣告為基礎的使用者

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [ **存取控制原則**]。
![顯示主控台樹中存取控制原則的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 以滑鼠右鍵按一下並選取 [ **新增存取控制原則**]。
![反白顯示 [新增存取控制原則] 功能表選項的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中，輸入原則的名稱、描述，然後按一下 [ **新增**]。
![螢幕擷取畫面，顯示當您建立規則以允許以 Windows Server 2016 上的連入宣告為基礎的使用者時，要在哪裡新增存取控制原則。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. 在 [ **規則編輯器**] 的 [使用者] 底下，將 [簽入] 中的 **特定宣告放入要求中** ，然後按一下底部有底線的 **特定** 宣告。
![醒目顯示要在哪裡選取加底線之特定的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. 在 [**選取宣告**] 畫面上，按一下 [宣告] 選項按鈕，選取 **宣告****類型**、**操作員** 和宣告 **值**，然後按一下 **[確定]**。
![顯示要在哪裡選取 [宣告] 選項的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  在 [ **規則編輯器** ] 上，按一下 **[確定]**。  在 [ **新增存取控制原則** ] 畫面上，按一下 **[確定]**。

8. 在 **AD FS 管理** 主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者 **信任**]。
![顯示要在哪裡選取 [信賴憑證者信任] 的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下您想要允許存取的信賴憑證者 **信任** ，然後選取 [ **編輯存取控制原則**]。
![顯示要在哪裡選取 [編輯存取控制原則] 功能表選項的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在存取控制原則上選取您的 **原則，然後按一下** **[套用] 和 [確定]**。
![顯示要在哪裡選取 [套用] 和 [確定] 的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>建立規則以根據 Windows Server 2016 上的連入宣告來拒絕使用者

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS**] 下，按一下 [ **存取控制原則**]。
![顯示要在哪裡選取存取控制原則的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 以滑鼠右鍵按一下並選取 [ **新增存取控制原則**]。
![顯示新增存取控制原則之位置的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在 [名稱] 方塊中，輸入原則的名稱、描述，然後按一下 [ **新增**]。
![顯示要在哪裡輸入原則名稱的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. 在 [**規則編輯器**] 中，確定已選取 [所有人]，並在 **[在要求中使用特定宣告** 來簽入] 底下按一下 [**僅** 限]，然後按一下底部有底線的 **特定** 宣告。
![顯示在哪裡確定已選取 [everyone] 選項的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. 在 [**選取宣告**] 畫面上，按一下 [宣告] 選項按鈕，選取 **宣告****類型**、**操作員** 和宣告 **值**，然後按一下 **[確定]**。
![顯示 [選取宣告] 畫面的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  在 [ **規則編輯器** ] 上，按一下 **[確定]**。  在 [ **新增存取控制原則** ] 畫面上，按一下 **[確定]**。

8. 在 **AD FS 管理** 主控台樹的 [ **AD FS**] 底下，按一下 [信賴憑證者 **信任**]。
![建立規則](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  以滑鼠右鍵按一下您想要允許存取的信賴憑證者 **信任** ，然後選取 [ **編輯存取控制原則**]。
![顯示以滑鼠右鍵按一下 [信賴憑證者信任] 以存取 [編輯存取控制原則] 功能表選項的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在存取控制原則上選取您的 **原則，然後按一下** **[套用] 和 [確定]**。
![顯示如何將變更套用至存取控制原則的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)


## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>建立規則以根據 Windows Server 2012 R2 上的連入宣告來允許或拒絕使用者

1.  在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2.  在主控台樹的 [ **AD FS \\ 信任關係] \\ 信賴** 憑證者信任底下，按一下清單中您要在其中建立此規則的特定信任。

3.  以滑鼠右鍵 \- 按一下選取的信任，然後按一下 [ **編輯宣告規則**]。
![顯示 [編輯宣告規則] 功能表選項的螢幕擷取畫面。](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 [ **發行授權規則** ] 索引標籤或 [ **委派授權規則** ] 索引標籤（ \( 根據您所需的授權規則類型 \) ），然後按一下 [ **新增規則** ] 以啟動 [ **新增授權宣告規則]**。
![顯示如何啟動 [新增授權宣告規則] 的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 [ **選取規則範本** ] 頁面的 [宣告 **規則範本**] 下，選取 [根據清單中的連 **入宣告允許或拒絕使用者** ]，然後按 **[下一步]**。
![螢幕擷取畫面，顯示要在哪裡選取 [根據傳入宣告] 範本來允許或拒絕使用者。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  在 [ **設定規則** ] 頁面的 [宣告 **規則名稱** ] 下，輸入此規則的顯示名稱，在 [連入宣告 **類型** ] 中選取清單中的 [宣告類型]，在 [連 **入宣告值** ] 下輸入值，或按一下 [流覽] （如果有的話），然後 \( \) 根據您的組織需求，選取下列其中一個選項：

    -   **允許具有這個傳入宣告的使用者存取**

    -   **拒絕存取具有此傳入** 
 ![ 宣告的使用者顯示要在哪裡選取連入宣告類型的螢幕擷取畫面。](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)
7.  按一下 [完成] 。

8.  在 [ **編輯宣告規則** ] 對話方塊中，按一下 **[確定]** 以儲存規則。

## <a name="additional-references"></a>其他參考資料
[設定宣告規則](Configure-Claim-Rules.md)

[檢查清單：為信賴憑證者信任建立宣告規則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[使用授權宣告規則的時機](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[宣告的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[宣告規則的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
