---
description: 深入瞭解：附錄 C： Active Directory 中受保護的帳戶和群組
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: 附錄 C-Active Directory 中受保護的帳戶和群組
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a968ddd70aa74d571e939c3d2c21cbc24e572f5e
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711763"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附錄 C︰Active Directory 中受保護的帳戶和群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附錄 C︰Active Directory 中受保護的帳戶和群組

在 Active Directory 中，會將一組預設的高許可權帳戶和群組視為受保護的帳戶和群組。 在 Active Directory 的大部分物件中，委派的系統管理員 (已委派許可權來管理 Active Directory 物件的使用者) 可以變更物件的許可權，例如，變更許可權以允許自己變更群組的成員資格。

不過，使用受保護的帳戶和群組時，會透過自動程式來設定和強制物件的許可權，以確保物件的許可權會保持一致，即使物件已移動目錄也一樣。 即使有人手動變更受保護物件的許可權，此程式仍可確保將許可權快速地傳回給其預設值。

### <a name="protected-groups"></a>受保護的群組

下表包含網域控制站作業系統所列出 Active Directory 中受保護的群組。

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>作業系統 Active Directory 中受保護的帳戶和群組

| Windows Server 2003 RTM | Windows Server 2003 SP1 + | Windows Server 2012、 <br> Windows Server 2008 R2、 <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Account Operators|Account Operators|Account Operators|Account Operators|
|系統管理員|系統管理員|系統管理員|系統管理員|
|系統管理員|系統管理員|系統管理員|系統管理員|
|Backup Operators|Backup Operators|Backup Operators|Backup Operators|
|Cert Publishers|||
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|
|網域控制站|網域控制站|網域控制站|網域控制站|
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Print Operators|Print Operators|Print Operators|Print Operators|
|||Read-only Domain Controllers|Read-only Domain Controllers|
|Replicator|Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|
|Server Operators|Server Operators|Server Operators|Server Operators|

#### <a name="adminsdholder"></a>AdminSDHolder

AdminSDHolder 物件的目的是要為網域中受保護的帳戶和群組提供「範本」許可權。 AdminSDHolder 會自動建立為每個 Active Directory 網域的系統容器中的物件。 其路徑為： **CN = AdminSDHolder，cn = System，dc =<domain_component>，dc =<domain_component>？。**

不同于系統管理員群組所擁有的 Active Directory 網域中的大部分物件，AdminSDHolder 是由 Domain Admins 群組所擁有。 根據預設，EAs 可以對任何網域的 AdminSDHolder 物件進行變更，就像網域的 Domain Admins 和 Administrators 群組一樣。 此外，雖然 AdminSDHolder 的預設擁有者是網域的 Domain Admins 群組，但是系統管理員或企業系統管理員的成員可以取得物件的擁有權。

#### <a name="sdprop"></a>SDProp

SDProp 是一種處理常式，預設會在保存網域 PDC 模擬器 (PDCE) 的網域控制站上，每60分鐘執行一次 () 。 SDProp 會比較網域的 AdminSDHolder 物件使用權限與網域中受保護帳戶和群組的許可權。 如果任何受保護帳戶和群組的許可權不符合 AdminSDHolder 物件的許可權，則會重設受保護帳戶和群組的許可權，以符合網域 AdminSDHolder 物件的許可權。

此外，受保護的群組和帳戶已停用許可權繼承，這表示即使將帳戶和群組移至目錄中的不同位置，它們也不會繼承其新父物件的許可權。 AdminSDHolder 物件上已停用繼承，因此父物件的許可權變更不會變更 AdminSDHolder 的許可權。

##### <a name="changing-sdprop-interval"></a>變更 SDProp 間隔

一般來說，您應該不需要變更 SDProp 執行的間隔，但測試用途除外。 如果您需要變更 SDProp 間隔，請在網域的 PDCE 上，使用 regedit 在 HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters. 中新增或修改 AdminSDProtectFrequency DWORD 值

值的範圍是從60到7200的秒數， (一分鐘到兩小時) 。 若要反轉變更，請刪除 AdminSDProtectFrequency 索引鍵，這會導致 SDProp 還原回60分鐘的間隔。 您通常不應該減少生產網域中的此間隔，因為它會增加網域控制站上的 LSASS 處理負荷。 此增加的影響取決於網域中受保護的物件數目。

##### <a name="running-sdprop-manually"></a>手動執行 SDProp

測試 AdminSDHolder 變更的較佳方法是手動執行 SDProp，這會導致工作立即執行，但不會影響排程的執行。 在執行 windows Server 2008 及更早版本的網域控制站上執行 SDProp 時，會在執行 Windows Server 2012 或 Windows Server 2008 R2 的網域控制站上以稍微不同的方式執行。

在舊版作業系統上手動執行 SDProp 的程式會在 [Microsoft 支援服務文章 251343](https://support.microsoft.com/kb/251343)中提供，而下列是較舊和較新的作業系統的逐步指示。 無論是哪一種情況，您都必須連接到 Active Directory 中的 rootDSE 物件，並針對 rootDSE 物件執行具有 null DN 的修改作業，並將作業的名稱指定為要修改的屬性。 如需 rootDSE 物件上可修改作業的詳細資訊，請參閱 MSDN 網站上的 [Rootdse 修改作業](/openspecs/windows_protocols/ms-adts/fc74972f-b267-4c1a-8716-0f5b48cf52b9) 。

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>在 Windows Server 2008 或更早版本中手動執行 SDProp

您可以使用 Ldp.exe 或執行 LDAP 修改腳本來強制執行 SDProp。 若要使用 Ldp.exe 執行 SDProp，請在對網域中的 AdminSDHolder 物件進行變更之後，執行下列步驟：

1. 啟動 **Ldp.exe**。
2. 按一下 [Ldp] 對話方塊上的 [ **連接** ]，然後按一下 **[連接]**。

   ![顯示 [連線] 功能表選項的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)

3. 在 [ **連線] 對話方塊** 中，輸入保存 PDC 模擬器 (PDCE) 角色之網域的網域控制站名稱，然後按一下 **[確定]**。

   ![螢幕擷取畫面，顯示要在哪裡輸入網域控制站的名稱，以保存 PDC 模擬器 (PDCE) 角色。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)

4. 確認您已成功連接，如下列螢幕擷取畫面中的 **Dn： (RootDSE)** 所示 **，按一下 [** 連線] **，然後按一下**[系結]。

   ![螢幕擷取畫面：顯示要在哪裡選取連接，然後選取 [系結] 以確認您已成功連線。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)

5. **在 [系** 結] 對話方塊中，輸入具有修改 rootDSE 物件使用權限之使用者帳戶的認證。  (如果您以該使用者的身分登入，您可以選取 [系結 **為** 目前登入的使用者]。 ) 按一下 **[確定]**。

   ![顯示 [系結] 對話方塊的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)

6. 完成系結作業之後，請按一下 **[流覽]**，然後按一下 [ **修改**]。

   ![在 [流覽] 功能表中顯示 [修改] 功能表選項的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)

7. 在 [ **修改** ] 對話方塊中，將 [ **DN** ] 欄位保留空白。 在 [ **編輯專案屬性** ] 欄位中，輸入 **FixUpInheritance**，然後在 [ **值** ] 欄位中輸入 **Yes**。 按一下 [ **輸入** ] 以填入 **專案清單** ，如下列螢幕擷取畫面所示。

   ![顯示 [修改] 對話方塊的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)

8. 在 [已填入的修改] 對話方塊中，按一下 [執行]，並確認您對 AdminSDHolder 物件所做的變更已出現在該物件上。

> [!NOTE]
> 如需修改 AdminSDHolder 以允許指定的無特殊許可權帳戶修改受保護群組成員資格的相關資訊，請參閱 [附錄 I：在 Active Directory 中建立受保護帳戶和群組的管理帳戶](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。

如果您想要透過 LDIFDE 或腳本手動執行 SDProp，您可以建立如下所示的修改專案：

![顯示如何建立修改專案的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>在 Windows Server 2012 或 Windows Server 2008 R2 中手動執行 SDProp

您也可以使用 Ldp.exe 或執行 LDAP 修改腳本來強制執行 SDProp。 若要使用 Ldp.exe 執行 SDProp，請在對網域中的 AdminSDHolder 物件進行變更之後，執行下列步驟：

1. 啟動 **Ldp.exe**。

2. 在 [ **Ldp** ] 對話方塊中，按一下 [ **連接**]，然後按一下 **[連接]**。

   ![顯示 [Ldp] 對話方塊的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)

3. 在 [ **連線] 對話方塊** 中，輸入保存 PDC 模擬器 (PDCE) 角色之網域的網域控制站名稱，然後按一下 **[確定]**。

   ![顯示 [連線] 對話方塊的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)

4. 確認您已成功連接，如下列螢幕擷取畫面中的 **Dn： (RootDSE)** 所示 **，按一下 [** 連線] **，然後按一下**[系結]。

   ![顯示 [連接] 功能表上 [系結] 功能表選項的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)

5. **在 [系** 結] 對話方塊中，輸入具有修改 rootDSE 物件使用權限之使用者帳戶的認證。  (如果您以該使用者的身分登入，您可以選取 [系結 **為目前登入的使用者**]。 ) 按一下 **[確定]**。

   ![螢幕擷取畫面，顯示在哪裡輸入具有修改 rootDSE 物件使用權限之使用者帳戶的認證。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)

6. 完成系結作業之後，請按一下 **[流覽]**，然後按一下 [ **修改**]。

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)

7. 在 [ **修改** ] 對話方塊中，將 [ **DN** ] 欄位保留空白。 在 [ **編輯專案屬性** ] 欄位中，輸入 **RunProtectAdminGroupsTask**，然後在 [ **值** ] 欄位中輸入 **1**。 按一下 [ **輸入** ] 以填入專案清單，如下所示。

   ![顯示 [編輯專案屬性] 欄位的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)

8. 在 [已填入的 **修改** ] 對話方塊中，按一下 [ **執行**]，並確認您對 AdminSDHolder 物件所做的變更已出現在該物件上。

如果您想要透過 LDIFDE 或腳本手動執行 SDProp，您可以建立如下所示的修改專案：

![如果您想要透過 LDIFDE 或腳本手動執行 SDProp，顯示該怎麼辦的螢幕擷取畫面。](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)
