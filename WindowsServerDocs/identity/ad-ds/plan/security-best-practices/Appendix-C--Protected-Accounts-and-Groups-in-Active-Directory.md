---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: 附錄 C-受保護的帳戶和 Active Directory 中的群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70e29ad42b57cf315c7179d6eea8220ad2bfab48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834699"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附錄 C：Active Directory 中受保護的帳戶和群組

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附錄 C：Active Directory 中受保護的帳戶和群組

Active Directory，內一組預設的高特殊權限的帳戶和群組會被視為受保護的帳戶和群組。 使用 Active Directory 中的大部分物件，委派的系統管理員 （使用者已被委派權限來管理 Active Directory 物件） 可以變更權限的物件，其中包括變更權限以允許本身變更的成員資格群組，例如。  

不過，與受保護的帳戶和群組，以物件的權限會設定，並強制執行透過自動化的程序，可確保您的權限的物件維持一致，即使物件是移動目錄。 即使有人手動變更受保護的物件權限，可確保此程序的權限會設為預設值中快速地傳回。  

### <a name="protected-groups"></a>受保護的群組

下表列出網域控制站作業系統的 Active Directory 中受保護的群組。  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>作業系統的 Active Directory 中受保護的帳戶和群組

| Windows Server 2003 RTM | Windows Server 2003 SP1+ | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Account Operators|Account Operators|Account Operators|Account Operators|
|Administrator|Administrator|Administrator|Administrator|
|Administrators|Administrators|Administrators|Administrators|
|Backup Operators|Backup Operators|Backup Operators|Backup Operators|
|Cert Publishers|||
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|
|網域控制站|網域控制站|網域控制站|網域控制站|
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|
||||企業重要的系統管理員|
||||索引鍵的系統管理員|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Print Operators|Print Operators|Print Operators|Print Operators|
|||Read-only Domain Controllers|Read-only Domain Controllers|
|Replicator|Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|
|Server Operators|Server Operators|Server Operators|Server Operators|

#### <a name="adminsdholder"></a>AdminSDHolder

AdminSDHolder 物件的目的是要提供受保護的帳戶和網域中的群組的 「 範本 」 權限。 AdminSDHolder 會自動建立為每個 Active Directory 網域的系統容器中的物件。 其路徑為：**CN=AdminSDHolder,CN=System,DC=<domain_component>,DC=<domain_component>?.**  

不同於大部分物件在 Active Directory 網域中，這系統管理員群組所擁有，AdminSDHolder 屬於 Domain Admins 群組。 根據預設，EAs 可以變更任何網域的 AdminSDHolder 物件，如同可以網域的 Domain Admins 和系統管理員群組。 此外，雖然預設的擁有者 AdminSDHolder 是網域的 Domain Admins 群組，Administrators 或 Enterprise Admins 的成員可能需要物件的擁有權。  

#### <a name="sdprop"></a>SDProp

SDProp 是持有網域的 PDC 模擬器 (PDCE) 的網域控制站執行 （依預設） 每隔 60 分鐘的處理序。 SDProp 比較網域的 AdminSDHolder 物件上的受保護的帳戶和網域中的群組的權限的權限。 如果任何受保護的帳戶和群組的權限不相符的 AdminSDHolder 物件的權限，受保護的帳戶和群組的權限會重設以符合那些網域的 AdminSDHolder 物件。  

此外，權限繼承已停用受保護的群組和帳戶，這表示，即使您的帳戶和群組會移到不同位置的目錄中，它們不會繼承權限自其新的父物件。 使父物件的權限變更不會變更 AdminSDHolder 的權限的 AdminSDHolder 物件已停用繼承。  

##### <a name="changing-sdprop-interval"></a>變更 SDProp 間隔

一般來說，您應該不需要變更的間隔時間 SDProp 執行，除了測試之用。 如果您需要變更網域，會在 PDCE 上的 SDProp 間隔使用 regedit 來新增或修改在 HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters AdminSDProtectFrequency DWORD 值。  

值範圍是以 60 秒 7200 （2 小時 1 分鐘）。 若要反轉所做的變更，請刪除 AdminSDProtectFrequency 索引鍵，這會導致 SDProp 還原回在 60 分鐘的間隔。 您通常應該減少此間隔在實際執行網域，它可能會增加額外負荷處理網域控制站上的 LSASS。 這種增加的影響是在網域中受保護的物件數目而定。  

##### <a name="running-sdprop-manually"></a>手動執行 SDProp

更好的方法，以測試 AdminSDHolder 變更為 SDProp 手動執行，這會導致立即執行的工作，但不會影響排定的執行。 以手動方式執行 SDProp 是執行 Windows Server 2008 的網域控制站上的方式稍有不同的執行和先前版本，比執行 Windows Server 2012 或 Windows Server 2008 R2 的網域控制站上。  

提供程序，以手動方式執行舊版作業系統上的 SDProp [Microsoft 支援服務文章 251343](https://support.microsoft.com/kb/251343)，以下是適用於舊版和新版作業系統的逐步指示和。 在任一情況下，您必須連接到 rootDSE 物件在 Active Directory 中，然後執行修改操作與 null 的 DN rootDSE 物件，指定作業的名稱做為要修改的屬性。 如需在 rootDSE 物件上的可修改作業的詳細資訊，請參閱[rootDSE 修改作業](https://msdn.microsoft.com/library/cc223297.aspx)MSDN 網站上。  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>在 Windows Server 2008 或更早版本中手動執行 SDProp

您可以強制執行使用 Ldp.exe 或執行的 LDAP 修改指令碼的 SDProp。 您已在網域中的 AdminSDHolder 物件的變更之後，若要執行 SDProp 使用 Ldp.exe，執行下列步驟：  

1. 啟動**Ldp.exe**。  
2. 按一下 **連接**Ldp 對話方塊中，然後按一下  **Connect**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. 在  **Connect**對話方塊中，輸入網域持有 PDC 模擬器 (PDCE) 角色的網域控制站的名稱，然後按一下**確定**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. 請確認您已連接成功，如所示**Dn:(RootDSE)** 在下列螢幕擷取畫面中，按一下**連接**然後按一下**繫結**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. 在 **繫結**對話方塊方塊中，輸入具有修改 rootDSE 物件的權限的使用者帳戶的認證。 (如果您以該使用者身分登入，您可以選取**繫結為**目前登入的使用者。)按一下 [確定] 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. 您已完成繫結作業之後，請按一下**瀏覽**，然後按一下**修改**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. 在 [**修改**] 對話方塊中，保持**DN**欄位保留空白。 在 **編輯項目屬性**欄位中，輸入**FixUpInheritance**，然後在**值**欄位中，輸入**是**。 按一下  **Enter**填入**項目清單**如下列螢幕擷取畫面所示。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. 在填入的 [修改] 對話方塊中，按一下 [執行]，並確認您對 AdminSDHolder 物件所做的變更會出現在該物件。  

> [!NOTE]  
> 如需修改 AdminSDHolder 以允許指定無特殊權限的帳戶，以修改受保護群組的成員資格的詳細資訊，請參閱[附錄 i:建立管理帳戶，受保護的帳戶和 Active Directory 中群組](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  

如果您想要透過 LDIFDE 或指令碼手動執行 SDProp，您可以建立的修改項目，如下所示：  

![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>在 Windows Server 2012 或 Windows Server 2008 R2 中手動執行 SDProp

您也可以強制執行使用 Ldp.exe 或執行的 LDAP 修改指令碼的 SDProp。 您已在網域中的 AdminSDHolder 物件的變更之後，若要執行 SDProp 使用 Ldp.exe，執行下列步驟：  

1. 啟動**Ldp.exe**。  

2. 在**Ldp**  對話方塊中，按一下**連線**，然後按一下**Connect**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. 在  **Connect**對話方塊中，輸入網域持有 PDC 模擬器 (PDCE) 角色的網域控制站的名稱，然後按一下**確定**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. 請確認您已連接成功，如所示**Dn:(RootDSE)** 在下列螢幕擷取畫面中，按一下**連接**然後按一下**繫結**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. 在 **繫結**對話方塊方塊中，輸入具有修改 rootDSE 物件的權限的使用者帳戶的認證。 (如果您以該使用者身分登入，您可以選取**繫結以目前登入的使用者**。)按一下 [確定] 。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. 您已完成繫結作業之後，請按一下**瀏覽**，然後按一下**修改**。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. 在 [**修改**] 對話方塊中，保持**DN**欄位保留空白。 在 **編輯項目屬性**欄位中，輸入**RunProtectAdminGroupsTask**，然後在**值**欄位中，輸入**1**。 按一下  **Enter**來填入項目清單，如下所示。  

   ![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. 中已填入**修改** 對話方塊中，按一下**執行**，並確認您對 AdminSDHolder 物件所做的變更會出現在該物件。  

如果您想要透過 LDIFDE 或指令碼手動執行 SDProp，您可以建立的修改項目，如下所示：  

![受保護的帳戶和群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
