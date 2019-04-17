---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: "C-附錄保護帳號，並在 Active Directory 中群組"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c1d29b61844428115f8b2d1f5d935f225a249659
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>C：附錄受保護的帳號及 Active Directory 中的群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>C：附錄受保護的帳號及 Active Directory 中的群組  
在 Active Directory 受保護的帳號和群組視為高度授權的帳號及群組預設設定。 大多數在 Active Directory 物件，委派系統管理員 （使用者已委派管理 Active Directory 物件的權限） 可以變更權限的物件，包括本身變更成員資格群組，例如允許權限的變更。  

不過的受保護的帳號，群組物件的權限的設定，並透過自動程序，以確保物件維持一致即使物件的權限移動 directory 執行。 即使某個人手動變更受保護的物件的權限，此程序可以確保權限的快速傳回設為預設值。  

### <a name="protected-groups"></a>受保護的群組  
下表包含可在 Active Directory 中列出的網域控制站作業系統受保護的群組。  

**受保護的帳號和作業系統 Active Directory 中的群組**  

|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4 Windows Server 2003 RTM**|**Windows Server 2003 SP1 +**|**Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008**|  
|系統管理員|Account 電信業者|Account 電信業者|Account 電信業者|  
||系統管理員|系統管理員|系統管理員|  
||系統管理員|系統管理員|系統管理員|  
||備份電信業者|備份電信業者|備份電信業者|  
||憑證的發行者|||  
|網域系統管理員 」|網域系統管理員 」|網域系統管理員 」|網域系統管理員 」|  
||網域控制站|網域控制站|網域控制站|  
|企業系統管理員|企業系統管理員|企業系統管理員|企業系統管理員|  
||Krbtgt|Krbtgt|Krbtgt|  
||列印電信業者|列印電信業者|列印電信業者|  
||||唯讀模式網域控制站|  
||複製者|複製者|複製者|  
|架構系統管理員|架構系統管理員|架構系統管理員|架構系統管理員|  
||伺服器電信業者|伺服器電信業者|伺服器電信業者|  

#### <a name="adminsdholder"></a>AdminSDHolder  
AdminSDHolder 物件的目的是提供 「 範本 」 權限的受保護的帳號及網域中的群組。 AdminSDHolder 會自動建立的每個 Active Directory domain 系統容器中的物件。 其路徑： **DATA-CN = AdminSDHolder，DATA-CN = 系統特區 = < domain_component，> 俠 = < domain_component >？。**  

不同於在 Active Directory 網域中，這由系統管理員群組所擁有，大部分物件 AdminSDHolder 屬於網域系統管理員 」 群組。 根據預設，EAs 可以做變更任何網域的 AdminSDHolder 物件，可以網域的網域系統管理員 」 及系統管理員的群組。 此外，雖然預設的擁有者 AdminSDHolder 網域的網域管理群組，系統管理員或企業系統管理員 」 的成員花費物件的擁有權。  

#### <a name="sdprop"></a>SDProp  
SDProp 是處理程序的網域控制站保留的網域 PDC 模擬器 (PDCE) 上執行 （預設） 每個 60 分鐘。 SDProp 比較網域的 AdminSDHolder 物件的權限的權限的受保護的帳號及網域中的群組。 如果任何受保護的帳號及群組的權限不符合 AdminSDHolder 物件的權限的權限的受保護的帳號和群組會重設以符合網域的 AdminSDHolder 物件。  

此外，權限繼承上已停用受保護的群組和帳號，這表示，即使帳號和群組移動到不同的位置在 directory，它們未繼承權限新父物件。 繼承已停用 AdminSDHolder 物件，以便父物件的權限的變更並不會變更 AdminSDHolder 的權限。  

##### <a name="changing-sdprop-interval"></a>變更 SDProp 長的時間間隔  
一般而言，您應該不需要變更的間隔的 SDProp 執行，除了測試目的。 如果您需要變更 SDProp 間隔 PDCE 網域中，使用 regedit 新增或修改 HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters AdminSDProtectFrequency DWORD 值。  

值的範圍是從 60 秒 7200 （分鐘到兩個小時的時間）。 若要即可反向所做的變更，delete AdminSDProtectFrequency 鍵，會導致 SDProp 還原回 60 分鐘的時間間隔。 您通常應該未減少此長的時間間隔 production 網域中它可以增加 LSASS 處理費用網域控制站。 增加的影響是受保護的物件網域中的數目而定。  

##### <a name="running-sdprop-manually"></a>手動執行 SDProp  
測試 AdminSDHolder 變更的最佳方法是執行 SDProp 以手動方式，會導致工作立即執行，但不會影響排程的執行。 手動執行 SDProp 是執行 Windows Server 2008 的網域控制站稍有不同上執行和以前是執行 Windows Server 2012 或 Windows Server 2008 R2 網域控制站在。  

適用於在較舊的作業系統上手動執行 SDProp 程序中提供[Microsoft 的支援文章 251343](https://support.microsoft.com/kb/251343)，，以下是為舊版和較新的作業系統逐步指示。 不論，您必須連接到 Active Directory 中進行 rootDSE 物件，並執行進行 rootDSE 物件，以空值 DN 修改操作，作業的名稱指定為修改屬性。 如需進行 rootDSE 物件修改作業，請查看[進行 rootDSE 修改作業](https://msdn.microsoft.com/library/cc223297.aspx)MSDN 網站上。  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Windows Server 2008，或更早版本中手動執行 SDProp  
您可以強制 SDProp 来執行利用 Ldp.exe 或執行 LDAP 修改指令碼。 您已變更網域中的 AdminSDHolder 物件之後，執行 SDProp 使用 Ldp.exe，執行下列步驟：  

1.  上市**Ldp.exe**。  

2.  按一下**連接**上 Ldp 對話方塊中，按**連接**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3.  在**連接**對話方塊中，輸入名稱的網域控制站的網域擁有角色 PDC 模擬器 (PDCE)，按一下 [ **[確定]**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4.  確認您有連接成功，如同**Dn: (進行 RootDSE)**下的螢幕擷取畫面，按**連接**，按一下 [**繫結**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5.  在**繫結**對話方塊中，輸入的認證使用者的修改進行 rootDSE 物件的權限。 (如果您的身分登入，您可以選取**繫結以**目前登入的使用者。)按一下**[確定]**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6.  繫結作業完成之後，請按一下**瀏覽]**，按一下 [**修改**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7.  在**修改]**對話方塊中，保留**DN**欄位空白。 在**編輯項目屬性**欄位中，輸入**FixUpInheritance**，並在**值**欄位，輸入**[是]**。 按一下**Enter**以填入**項目清單**下的螢幕擷取畫面中所示。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8.  在填入修改對話方塊中，按一下 [執行，並確認 AdminSDHolder 物件您所做的變更，會出現的物件。  

> [!NOTE]  
> 修改允許修改的受保護的群組成員資格指定授權的帳號 AdminSDHolder 相關資訊，請查看[附錄 i： 建立管理帳號 Active Directory 中的群組保護帳號，](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  

如果您喜歡手動透過 LDIFDE 或指令碼執行 SDProp，您可以建立修改項目，如下所示：  

![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Windows Server 2012 或 Windows Server 2008 R2 手動執行 SDProp  
您也可以強制 SDProp 来執行利用 Ldp.exe 或執行 LDAP 修改指令碼。 您已變更網域中的 AdminSDHolder 物件之後，執行 SDProp 使用 Ldp.exe，執行下列步驟：  

1.  上市**Ldp.exe**。  

2.  在**Ldp**對話方塊中，按一下 [**連接**，並按**連接**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3.  在**連接**對話方塊中，輸入名稱的網域控制站的網域擁有角色 PDC 模擬器 (PDCE)，按一下 [ **[確定]**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4.  確認您有連接成功，如同**Dn: (進行 RootDSE)**下的螢幕擷取畫面，按**連接**，按一下 [**繫結**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5.  在**繫結**對話方塊中，輸入的認證使用者的修改進行 rootDSE 物件的權限。 (如果您的身分登入，您可以選取**如目前登入的使用者繫結**。)按一下**[確定]**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6.  繫結作業完成之後，請按一下**瀏覽]**，按一下 [**修改**。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7.  在**修改]**對話方塊中，保留**DN**欄位空白。 在**編輯項目屬性**欄位中，輸入**RunProtectAdminGroupsTask**，並在**值**欄位，輸入**1**。 按一下**Enter**來填入項目清單，如此處所示。  

    ![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8.  填入在**修改**對話方塊中，按一下 [**執行**，並確認該物件，會出現 AdminSDHolder 物件您所做的變更。  

如果您喜歡手動透過 LDIFDE 或指令碼執行 SDProp，您可以建立修改項目，如下所示：  

![受保護的帳號，並群組](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
