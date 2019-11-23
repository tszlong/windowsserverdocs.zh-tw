---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: 附錄 C-Active Directory 中受保護的帳戶和群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 606b3a42d70ee5c2a3479f9c9df2f95a495d6afd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408722"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附錄 C︰Active Directory 中受保護的帳戶和群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附錄 C︰Active Directory 中受保護的帳戶和群組

在 Active Directory 內，會將一組預設的高許可權帳戶和群組視為受保護的帳戶和群組。 在 Active Directory 中的大部分物件，委派的系統管理員（已委派管理 Active Directory 物件的許可權）可以變更物件的許可權，包括變更許可權以允許自己變更的成員資格例如，群組。  

不過，透過受保護的帳戶和群組，物件的許可權會透過自動程式來設定和強制執行，確保物件上的許可權會保持一致，即使物件已移至目錄也一樣。 即使有人手動變更受保護物件的許可權，此程式仍可確保許可權會快速地傳回給其預設值。  

### <a name="protected-groups"></a>受保護的群組

下表包含網域控制站作業系統所列出 Active Directory 中受保護的群組。  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Active Directory 中受保護的帳戶和群組（依作業系統）

| Windows Server 2003 RTM | Windows Server 2003 SP1 + | Windows Server 2012、 <br> Windows Server 2008 R2、 <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Account Operators|Account Operators|Account Operators|Account Operators|
|Administrator|Administrator|Administrator|Administrator|
|Administrators|Administrators|Administrators|Administrators|
|Backup Operators|Backup Operators|Backup Operators|Backup Operators|
|Cert Publishers|||
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|
|網域控制站|網域控制站|網域控制站|網域控制站|
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|
||||企業金鑰管理員|
||||金鑰管理員|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Print Operators|Print Operators|Print Operators|Print Operators|
|||Read-only Domain Controllers|Read-only Domain Controllers|
|Replicator|Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|
|Server Operators|Server Operators|Server Operators|Server Operators|

#### <a name="adminsdholder"></a>AdminSDHolder

AdminSDHolder 物件的目的是要為網域中受保護的帳戶和群組提供「範本」許可權。 AdminSDHolder 會自動建立為每個 Active Directory 網域之系統容器中的物件。 其路徑為： **cn = AdminSDHolder，cn = System，dc = < domain_component >，dc = < domain_component >？。**  

不同于系統管理員群組所擁有之 Active Directory 網域中的大部分物件，AdminSDHolder 是由 Domain Admins 群組所擁有。 根據預設，EAs 可以對任何網域的 AdminSDHolder 物件進行變更，就像網域的 Domain Admins 和 Administrators 群組一樣。 此外，雖然 AdminSDHolder 的預設擁有者是網域的 Domain Admins 群組，但系統管理員或企業系統管理員的成員可以取得物件的擁有權。  

#### <a name="sdprop"></a>SDProp

SDProp 是在保存網域 PDC 模擬器（PDCE）的網域控制站上每60分鐘執行一次（根據預設）的程式。 SDProp 會比較網域的 AdminSDHolder 物件使用權限與網域中受保護帳戶和群組的許可權。 如果任何受保護帳戶和群組的許可權不符合 AdminSDHolder 物件上的許可權，則會重設受保護帳戶和群組的許可權，以符合網域的 AdminSDHolder 物件。  

此外，受保護的群組和帳戶會停用許可權繼承，這表示即使帳戶和群組已移至目錄中的不同位置，它們也不會繼承其新父物件的許可權。 AdminSDHolder 物件上的繼承已停用，因此父物件的許可權變更不會變更 AdminSDHolder 的許可權。  

##### <a name="changing-sdprop-interval"></a>變更 SDProp 間隔

一般來說，您應該不需要變更 SDProp 執行的間隔，但測試用途除外。 如果您需要變更網域的 PDCE 上的 SDProp 間隔，請使用 regedit 來新增或修改 Hklm\system\currentcontrolset\services\ntds\parameters 中的 AdminSDProtectFrequency DWORD 值  

值的範圍以秒為單位，從60到7200（一分鐘到兩小時）。 若要反轉變更，請刪除 AdminSDProtectFrequency key，這會導致 SDProp 還原回60分鐘的間隔。 一般來說，您不應該在生產網域中減少此間隔，因為它可能會增加網域控制站上的 LSASS 處理負荷。 這項增加的影響取決於網域中受保護的物件數目。  

##### <a name="running-sdprop-manually"></a>手動執行 SDProp

測試 AdminSDHolder 變更的更好方法是手動執行 SDProp，這會導致工作立即執行，但不會影響排程的執行。 在執行 Windows Server 2008 和更早版本的網域控制站上，會以手動方式執行 SDProp，而不是在執行 Windows Server 2012 或 Windows Server 2008 R2 的網域控制站上執行。  

[Microsoft 支援服務文章 251343](https://support.microsoft.com/kb/251343)中提供了在舊版作業系統上手動執行 SDProp 的程式，以下是舊版和較新作業系統的逐步指示。 不論是哪一種情況，您都必須連接到 Active Directory 中的 rootDSE 物件，並對 rootDSE 物件執行含有 null DN 的修改作業，將作業的名稱指定為要修改的屬性。 如需 rootDSE 物件之可修改作業的詳細資訊，請參閱 MSDN 網站上的[RootDSE 修改作業](https://msdn.microsoft.com/library/cc223297.aspx)。  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>在 Windows Server 2008 或更早版本中手動執行 SDProp

您可以使用 Ldp.exe 或藉由執行 LDAP 修改腳本，強制執行 SDProp。 若要使用 Ldp.exe 執行 SDProp，請在對網域中的 AdminSDHolder 物件進行變更之後，執行下列步驟：  

1. 啟動**ldp.exe**。  
2. 按一下 [Ldp] 對話方塊上的 [**連接**]，然後按一下 **[連接]** 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. 在 [**連接**] 對話方塊中，輸入保留 PDC 模擬器（PDCE）角色之網域的網域控制站名稱，然後按一下 **[確定]** 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. 請確認您已成功連線，如下列螢幕擷取畫面中的**Dn：（RootDSE）** 所指示 **，按一下 [** **連接**]，然後按一下 [系結]。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. **在 [系**結] 對話方塊中，輸入具有修改 rootDSE 物件使用權限之使用者帳戶的認證。 （如果您是以該使用者的身分登入，則可以選取 [系結**為**目前登入的使用者]）。按一下 **[確定]** 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. 完成系結操作之後，請按一下 **[流覽]** ，然後按一下 [**修改**]。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. 在 [**修改**] 對話方塊中，將 [ **DN** ] 欄位保留空白。 在 [**編輯專案屬性**] 欄位中，輸入**FixUpInheritance**，然後在 [**值**] 欄位中輸入**Yes**。 按一下**Enter**以填入**專案清單**，如下列螢幕擷取畫面所示。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. 在 [填入的修改] 對話方塊中，按一下 [執行]，並確認您對 AdminSDHolder 物件所做的變更已出現在該物件上。  

> [!NOTE]  
> 如需修改 AdminSDHolder 以允許指定的無許可權帳戶修改受保護群組成員資格的相關資訊，請參閱[附錄 I：在 Active Directory 中建立受保護帳戶和群組的管理帳戶](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  

如果您想要透過 LDIFDE 或腳本手動執行 SDProp，您可以建立修改專案，如下所示：  

![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>在 Windows Server 2012 或 Windows Server 2008 R2 中手動執行 SDProp

您也可以使用 Ldp.exe 或藉由執行 LDAP 修改腳本，強制執行 SDProp。 若要使用 Ldp.exe 執行 SDProp，請在對網域中的 AdminSDHolder 物件進行變更之後，執行下列步驟：  

1. 啟動**ldp.exe**。  

2. 在 [ **Ldp** ] 對話方塊中，按一下 [**連接**]，然後按一下 **[連接]** 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. 在 [**連接**] 對話方塊中，輸入保留 PDC 模擬器（PDCE）角色之網域的網域控制站名稱，然後按一下 **[確定]** 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. 請確認您已成功連線，如下列螢幕擷取畫面中的**Dn：（RootDSE）** 所指示 **，按一下 [** **連接**]，然後按一下 [系結]。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. **在 [系**結] 對話方塊中，輸入具有修改 rootDSE 物件使用權限之使用者帳戶的認證。 （如果您是以該使用者的身分登入，則可以選取 [系結**為目前登入的使用者**]）。按一下 **[確定]** 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. 完成系結操作之後，請按一下 **[流覽]** ，然後按一下 [**修改**]。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. 在 [**修改**] 對話方塊中，將 [ **DN** ] 欄位保留空白。 在 [**編輯專案屬性**] 欄位中，輸入**RunProtectAdminGroupsTask**，然後在 [**值**] 欄位中輸入**1**。 按一下**Enter**以填入專案清單，如下所示。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. 在 [填入的**修改**] 對話方塊中，按一下 [**執行**]，並確認您對 AdminSDHolder 物件所做的變更已出現在該物件上。  

如果您想要透過 LDIFDE 或腳本手動執行 SDProp，您可以建立修改專案，如下所示：  

![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
