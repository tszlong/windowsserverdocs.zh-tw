---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: "附錄我-建立管理帳號受保護的帳號和 Active Directory 中的群組"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 666180adca691d6c9783a43063df76877115fc40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附錄 i：建立管理帳號受保護的帳號和 Active Directory 中的群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

  
## <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附錄 i：建立管理帳號受保護的帳號和 Active Directory 中的群組  

挑戰實作並不仰賴永久高特殊權限群組成員資格 Active Directory 模型是必須需要暫時群組成員資格時填入這些群組的機制。 某些權限的身分管理方案需要軟體的服務帳號會授與永久群組，例如 DA 或森林中的每個網域中的系統管理員的資格。 不過，它不技術所需的特殊權限的身分管理 (PIM) 方案在這種高特殊權限的環境中執行他們的服務。  
  
此附錄建立帳號有限的權限但可以嚴格控制，可用於填入 Active Directory 中有特殊權限的群組需要暫時提高權限時，您可以使用的原生實作或第三方 PIM 方案的資訊。 如果實作 PIM 成原生方案時，可能會這些帳號，系統管理員的員工用來執行暫時群組擴展，如果您透過協力廠商軟體實作 PIM，您可以調整下列帳號作為服務帳號，  
  
> [!NOTE]  
> 這個附錄中所述的程序提供管理高特殊權限的群組 Active Directory 中的其中一個方法。 您可以調整下列程序，以符合您需求、 新增額外的限制或省略部分限制，如下所示。  
  
### <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>建立管理帳號受保護的帳號和 Active Directory 中的群組  
建立帳號，可用來管理的特殊權限群組成員資格，而不需要太多權限授與管理帳號，並包含所述逐步指示，請依照下列四個一般活動權限：  
  
1.  首先，您應該會建立群組，以管理帳號，因為這些帳號應該由一組有限的受信任的使用者。 如果不已經有可容納將權限和受保護帳號，並一般擴展網域中的組織單位結構，您應該會建立一個。 特定的指示執行並不提供這個附錄，雖然螢幕擷取畫面會顯示此類組織單位階層的範例。  
  
2.  建立管理帳號。 這些帳號應該建立 「 一般 「 帳號，並不預設已經授權給使用者以外的使用者權限授與。  
  
3.  實作限制的管理帳號，並讓他們使用只能目的特殊的所建立，除了控制誰可以讓及使用帳號 （您建立的第一個步驟的群組） 上。  
  
4.  設定 AdminSDHolder 網域中的物件每個允許管理帳號，若要變更的網域中的特殊權限群組成員資格權限。  
  
完全應該測試所有的這些程序，視您的環境 production 環境中執行之前進行修改。 您也應該確認 [所有設定]，如預期般都運作 （部分測試提供程序本附錄），您應該先測試損壞復原案例中管理帳號並不適用於用於填入進行修復受保護的群組。 如需有關備份及還原 Active Directory 的詳細資訊，請查看[AD DS 備份和復原逐步](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx)。  
  
> [!NOTE]  
> 利用這個附錄中所述，您將會建立帳號將能管理每個網域中的所有受保護的群組、 不僅的最高權限 Active Directory 群組等 Ea、 DAs 及 BAs 成員資格。 如需受保護的群組 Active Directory 中相關資訊，請查看[附錄 c： 保護帳號，並 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
#### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>建立管理帳號受保護的群組逐步指示  
  
##### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>建立來讓及停用管理帳號群組  
管理帳號應該已經在每次使用重設密碼和時活動需要是完整應該停用。 您也可以考慮實作智慧卡登入需求這些帳號，雖然這是選擇性的設定，這些指示假設管理帳號將設定的使用者名稱和時間，複雜密碼，以最小的控制項。 在此步驟，您將會建立重設密碼的管理帳號，可以讓或停用帳號權限的群組。  
  
若要建立來讓及停用管理帳號群組，請執行下列步驟：  
  
1.  在組織單位結構中，您將會與容納管理帳號，以滑鼠右鍵按一下組織的單位您想要用來建立群組中，按一下 [**新**，按一下 [**群組**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  在**新物件-群組**對話方塊中，輸入該群組的名稱。 如果您打算使用此群組 」 啟動 「 您森林中的所有管理帳號，讓它萬用安全性群組。 如果您有樹系單一網域，或如果您想在每個網域中建立群組，您可以建立安全性的全域群組。 按一下**[確定]**以建立群組。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  以滑鼠右鍵按一下您剛建立的群組中，按一下**屬性**，按一下 [**物件**索引標籤。在群組中的**物件的屬性**對話方塊中，選取**以防止誤刪除保護物件**，這將會只使用者在無法否則授權刪除群組，但也將它移到另一個，除非第一次取消選取屬性。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  

    > 如果您已經有群組的父系 Ou 限制的管理一組有限的使用者設定的權限，您可能不需要執行下列步驟。 他們提供以下，即使尚未實作已建立此群組的組織單位結構系統有限的控制，您可以安全對修改群組未經授權的使用者。  
  
4.  按一下**成員**索引標籤，然後將帳號新增的人員將會讓管理帳號或填入負責小組的成員保護時所需的群組。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  如果您有未做，請在**Active Directory 使用者和電腦**主機，請按一下 [**檢視**，然後選取**進階功能**。 以滑鼠右鍵按一下您剛建立的群組中，按一下**屬性**，按一下 [**的安全性**索引標籤。在**安全性**索引標籤上，按**進階]**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  在**群組] 的進階安全性設定**對話方塊中，按**停用繼承**。 出現提示時，按一下 [**轉換繼承到明確此物件的權限的權限**，按一下 [ **[確定]**以返回群組的**安全性**] 對話方塊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  在**安全性**索引標籤上，這應該不會存取此群組允許群組中移除。 例如，如果您不想 Authenticated Users 以讀取群組的名稱及一般屬性，您可以移除該 A。 您也可以移除 a，例如這些 account 電信業者和 windows 2000 Server 相容存取。 不過，，您應該就地退出物件的權限的最低的設定。 保留下列 a:  
  
    -   自我  
  
    -   系統  
  
    -   網域系統管理員 」  
  
    -   企業系統管理員  
  
    -   系統管理員  
  
    -   Windows 的授權的存取群組 （如果適用）  
  
    -   企業網域控制站  
  
    雖然它看起來直覺式允許的最高有特殊權限的群組 Active Directory 管理此群組中，您在執行這些設定的目標是不防止授權的變更的這些群組成員。 而是，目標是確保當您需要更高等級權限的用場時，會在授權的變更會成功。 它是針對這個原因，變更預設的權限群組巢，權限，且本文件建議的權限。 離開結構預設關聯性，以及清空 directory 中的最高的權限群組成員資格，您可以建立更安全的環境仍會運作如預期般運作。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > 如果您無法在此群組位置建立組織單位結構已經設定物件的稽核原則，您應該設定稽核來登入變更此群組。  
  
8.  您已完成用於 「 請查看 「 群組的組態管理帳號，而且時，就需要 「 簽入 「 帳號完成他們的活動。  
  
##### <a name="creating-the-management-accounts"></a>建立管理帳號  
您應該建立至少一個 account 用於管理的安裝程式 Active Directory 中有特殊權限群組成員資格，最好是第二個 account 做為備份。 您是否選擇單一樹系網域中建立管理帳號，並授權管理功能所有網域保護群組，或您選擇管理帳號實作森林中的每個網域中，程序是否有效相同。  
  
> [!NOTE]  
> 本文件中的步驟假設，您有尚未實作以角色為基礎存取控制和身分特殊權限的管理的 Active Directory。 因此，使用者其 account 是有問題的網域網域管理群組成員必須執行某些程序。  
>   
> 當您使用 account DA 權限時，您可以登入執行活動設定的網域控制站。 登入以管理工作站的權限較低帳號，可以執行步驟不需要 DA 權限。 螢幕擷取畫面，以顯示藍色淺色對話方塊起代表網域控制站可執行的活動。 藍色深色中顯示的對話方塊的螢幕擷取畫面代表系統工作站帳號，所受到的權限才能執行的活動。  
  
若要建立管理帳號，請執行下列步驟：  
  
1.  登入網域控制站的為網域的 DA 群組成員。  
  
2.  上市**Active Directory 使用者和電腦**，瀏覽到您將會建立管理 account 組織單位。  
  
3.  以滑鼠右鍵按一下組織單位，然後按一下**新增]** ，按一下 [**使用者**。  
  
4.  在**新物件-使用者**對話方塊中，輸入您想要的命名資訊帳號，按一下 [**下一步**。  
  
5.  登入網域控制站的為網域的 DA 群組成員。  
  
6.  上市**Active Directory 使用者和電腦**，瀏覽到您將會建立管理 account 組織單位。  
  
7.  以滑鼠右鍵按一下組織單位，然後按一下**新增]** ，按一下 [**使用者**。  
  
8.  在**新物件-使用者**對話方塊中，輸入您想要的命名資訊帳號，按一下 [**下一步**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
9. 初始密碼提供使用者帳號，清除**使用者必須在變更密碼登入下一步**，請選取**使用者無法變更密碼**和**Account 已停用**，然後按一下**下一步**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  
  
10. 確認 account 詳細資料的正確，然後按一下 [**完成**。  
  
11. 以滑鼠右鍵按一下您剛建立的使用者物件，然後按一下**屬性**。  
  
12. 按一下**Account**索引標籤。  
  
13. 在**Account 選項**欄位中，選取**機密帳號，無法委派**旗標，選取**此 account 支援 Kerberos 好一段 128 元加密**和 （或)**此 account 支援 Kerberos 好一段 256 加密**旗標，然後按一下**[確定]**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  
  
    > [!NOTE]  
    > 此帳號，例如其他帳號，將會有有限，但強大功能，因為 account 只能在安全的系統管理主機上。 適用於所有安全管理主機環境中，您應該考慮實作的群組原則設定**網路安全性： 設定加密類型允許 Kerberos 的**允許的最安全加密類型可以實作的安全主機。  
    >   
    > 雖然實作更安全加密類型的主機不未減少認證竊取攻擊，就能的適當地使用與安全主機設定。 設定僅供特殊權限帳號主機較加密類型是只會整體攻擊 surface 的電腦。  
    >   
    > 適用於系統和帳號設定加密類型的相關詳細資訊，請查看[Windows 設定 Kerberos 支援加密類型的](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)。  
    >   
    > **注意**執行 Windows Server 2012、 Windows Server 2008 R2、 Windows 8 或 Windows 7 的電腦上只支援這些設定。  
  
14. 在**物件**索引標籤，選取**以防止誤刪除保護物件**。 這將會不只防止物件會被 （即使是會在授權的使用者），但會防止正被移動至不同的組織單位，在您 AD DS 階層，除非核取方塊來變更屬性的權限的使用者第一次未。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  
  
15. 按一下**遠端控制**索引標籤。  
  
16. 清除**可讓遠端控制**旗標。 不應所需的支援人員連接到此帳號的工作階段進行修正。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  
  
    > [!NOTE]  
    > Active Directory 中的每個物件應有的指定的 IT 擁有者以及指定的公司擁有者中所述[規劃危害的](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)。 如果您追蹤 Active Directory （而不是外部資料庫） AD DS 物件的擁有權，您應該輸入適當的擁有權物件的屬性。  
    >   
    > 此時，請公司擁有者最有可能是 IT 部門，andthere 不禁止在公司擁有者也正在 IT 擁有者。 建立物件的擁有權點是讓您以找出連絡人時，會對物件，也許年從他們的初始建立需要做的變更。  
  
17. 按一下**組織**索引標籤。  
  
18. 在您 AD DS 物件標準輸入所需的任何資訊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  
  
19. 按一下**-在**索引標籤。  
  
20. 在**的網路存取權限**欄位中，選取**拒絕**。完全不需要此 account 應該遠端連接到連接。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  
  
    > [!NOTE]  
    > 不太此帳號，可用來登入您的環境中唯讀網域控制站 (Rodc)。 不過，應該情況得更容易需要 account 登入 RODC，您應該將這個 account 的拒絕 RODC 密碼複寫群組，使其密碼 RODC 不快取。  
    >   
    > 雖然應之後每次使用密碼重設應該停用帳號，實作此設定會不帳號，造成不利的影響，它可能會在情形中的系統管理員的身分忘記重設密碼，來停用它來協助。  
  
21. 按一下**成員的**索引標籤。  
  
22. 按一下**新增**。  
  
23. 輸入**拒絕 RODC 密碼複寫群組**在**選取使用者] 連絡人的電腦**對話方塊中，按一下 [**檢查名稱]**。 當群組的名稱底線物件器中時，按一下**[確定]** ，並確認 account 現在會顯示在螢幕擷取畫面下列兩個群組成員。 Account 新增至受保護的任何群組。  
  
24. 按一下**[確定]**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  
  
25. 按一下**安全性**索引標籤，然後按**進階]**。  
  
26. 在**進階安全性設定]**對話方塊中，按一下 [**繼承停用**和繼承的權限以明確的權限，然後按一下 [**新增**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  
  
27. 在**權限的項目的 [Account]**對話方塊中，按一下 [**選取主體**，並加入您建立先前的程序群組。 捲動到底部的 [] 對話方塊中，按一下**[全部清除]**若要移除所有預設的權限。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  
  
28. 捲動至頂端**的權限的項目**對話方塊。 確保**輸入**下拉式清單為**允許**，在**適用於**下拉式清單中，選取**只有這個物件**。  
  
29. 在**權限**欄位中，選取**朗讀所有屬性**，**都朗讀權限]**，和**重設密碼**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  
  
30. 在**屬性**欄位中，選取**朗讀 userAccountControl**並**撰寫 userAccountControl**。  
  
31. 按一下**[確定]**， **[確定]**再試一次在**進階安全性設定]** ] 對話方塊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  
  
    > [!NOTE]  
    > **UserAccountControl**屬性控制項多個 account 設定選項。 您無法將變更的設定選項僅限部分時屬性寫入權限授與您的權限授與。  
  
32. 在**群組或使用者名稱**欄位的**的安全性**索引標籤上，這應該不會存取或管理 account 允許任何群組中移除。 例如 Everyone 群組和自我計算帳號，並移除拒絕 a，已經設定的任何群組 (該 a 設定時**使用者無法變更密碼]**旗標期間帳號建立的支援。 也不要移除您剛加入該群組，系統帳號或群組，例如 EA、 DA、 BA 或 Windows 授權的存取群組。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  
  
33. 按一下**進階**，並確認該進階安全性設定] 對話方塊類似下列螢幕擷取畫面。  
  
34. 按一下**[確定]**，以及**[確定]**一次以關閉 account 的屬性對話方塊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  
  
35. 第一次管理帳號的安裝程式現在已完成。 您將會在稍後程序測試 account。  
  
###### <a name="creating-additional-management-accounts"></a>建立帳號其他管理  
重複上一個步驟、 複製帳號，您剛建立，或建立指令碼以您想要的設定的設定建立帳號，您可以建立其他管理帳號。 注意，是否您要複製您剛建立的帳號，數個自訂的設定和 Acl 將不會將複製到新的帳號，且您將必須重複的設定步驟來執行大多數。  
  
您可以改為建立的群組您代理人的權限填入和 unpopulate 受保護的群組，但您將需要安全群組和帳號，您將其置於。 應該會在您 directory 授權管理群組成員資格的受保護的能力很少帳號，因為建立個人帳號可能是最簡單的方法。  
  
無論您如何選擇建立的群組，您可以在其中放置管理帳號，您應該確定之前所述的保護每個 account。 您也應該考慮實作 GPO 限制類似中所述[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
###### <a name="auditing-management-accounts"></a>稽核帳號管理  
您應會設定稽核至少登所有寫入 account account。 這可讓您不只可以找出成功的 account 讓和期間授權使用，但也找出未經授權的使用者嘗試操作 account 的密碼重設。 失敗的寫入帳號應該會拍下您的安全性資訊與事件監視 (SIEM) 系統 （如果有的話），以及應該觸發程序提供給負責調查潛在折衷人員通知的警示。  
  
SIEM 方案需要事件資訊 （例如，事件登、 應用程式資料、 網路串流、 你反惡意程式碼和入侵偵測來源） 的安全性相關的來源、 分頁資料，並嘗試智慧檢視和預防措施。 有許多 commercial SIEM 方案，以及許多企業建立私人實作。 安全性監視和回應事件功能，也設計和實作適當 SIEM 大幅可以美化。 不過的功能和準確度而有所不同大幅方案。 SIEMs 超出範圍此紙上一樣，但任何 SIEM 實作器所包含的特定的事件建議視為。  
  
如建議的稽核設定的網域控制站的相關詳細資訊，請查看[的符號的危害監視 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。 網域控制站的特定設定以提供[適用於符號的危害監視 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。  
  
##### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>讓管理帳號修改的受保護的群組成員資格  
在這個程序，您將會設定允許修改群組成員資格的受保護網域中的新建立的管理帳號網域的 AdminSDHolder 物件的權限。 透過圖形使用者介面 (GUI) 無法執行此程序。  
  
中所述[附錄 c： 保護帳號，並 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)，在 SDProp 工作執行時的物件有效 「 即可 」 的網域 AdminSDHolder ACL 受保護物件。 受保護的群組和帳號未繼承他們的權限 AdminSDHolder 物件。他們的權限明確為符合 AdminSDHolder 物件。 因此，當您修改 AdminSDHolder 物件的權限時，您必須修改它們適用於類型的受保護您的目標的物件的屬性。  
  
若是如此，您將會授與新建的管理帳號，讓它們讀取和寫入成員屬性物件群組。 不過，AdminSDHolder 物件群組物件並不群組屬性不會顯示在圖形 ACL 編輯器。 它是針對這個原因，您將會執行透過 Dsacls 命令列的公用程式的權限的變更。 若要 （停用） 管理帳號權限授與修改受保護的群組成員資格，執行下列步驟：  
  
1.  網域控制站，最好是角色網域控制站 PDC 模擬器 (PDCE)、 已 DA 群組成員網域中的使用者 account 的認證登入。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  打開提升權限的命令提示字元中，以滑鼠右鍵按一下**命令提示字元**，按一下 [**以系統管理員身分執行**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  核准提高權限提示，請按一下**[是]**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > 如需提高權限和使用者 account 控制 Windows 中，查看[UAC 處理程序與互動](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx)TechNet 網站上。  
  
4.  在命令提示字元中，輸入 （替代您的網域特定資訊） **Dsacls 分辨姓名 AdminSDHolder 物件網域中的 [管理 account UPN] /G: RPWP; 成員**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    （這是不區分大小寫） 前一個命令的運作方式如下：  
  
    -   Dsacls 設定或 a 顯示 directory 物件  
  
    -   DATA-CN = AdminSDHolder，DATA-CN = 系統特區 = TailSpinToys 特區 = msft 辨識修改物件  
  
    -   /G 顯示 [正在設定授與 a  
  
    -   PIM001@tailspintoys.msft是的使用者主體名稱 (UPN) a 被授與的安全性主體  
  
    -   RPWP 授權讀取和寫入屬性權限  
  
    -   成員位於屬性名稱的使用權限設定  
  
    如需有關使用**Dsacls**，而不需要任何參數，在命令提示字元中輸入 Dsacls。  
  
    如果您已建立網域中的多個管理帳號，您應該會執行每個帳號 Dsacls 命令。 當您完成 AdminSDHolder 物件 ACL 設定時，您應該強制 SDProp 執行，或等待其排程的執行完成。 有關強迫 SDProp 執行，查看 [手動執行 SDProp 」[附錄 c： 保護帳號，並 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
    SDProp 已執行時，您就可以驗證您 AdminSDHolder 物件所做的變更，已套用至網域中受保護的群組。 您無法驗證此 ACL 檢視 AdminSDHolder 物件，如之前所述的原因，但您可以透過受保護的群組上檢視 Acl 已套用權限來確認。  
  
5.  在**Active Directory 使用者和電腦**，確認您有支援**進階功能**。 若要這樣做，請按一下**檢視**，找出**網域系統管理員 」**群組中，以滑鼠右鍵按一下該群組，按一下 [**屬性**。  
  
6.  按一下**安全性**索引標籤，然後按一下 [**進階]**打開**進階安全性設定針對網域系統管理員**] 對話方塊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  選取 [**允許 a 管理 account 的**，按一下 [**編輯**。 確認 account 授權，只**朗讀成員**和**寫入成員**上 DA 群組中，按一下 [權限**[確定]**。  
  
8.  按一下**[確定]**在**進階安全性設定]**對話方塊中，然後按一下 [ **[確定]**再試一次以關閉 [DA 群組] 屬性對話方塊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. 您可以在其他受保護的群組; 網域中的重複上一個步驟權限應該相同的所有受保護的群組。 您現在已完成建立及管理帳號，在這個網域中群組受保護的設定。  
  
    > [!NOTE]  
    > 任何 account 的權限群組成員資格寫入 Active Directory 中也可以新增自己群組。 這是設計，也無法停用。 基於這個原因，您應該會一直在非使用中，停用管理帳號，以他們正在停用，以及使用中時密切應該監視帳號。  
  
##### <a name="verifying-group-and-account-configuration-settings"></a>確認群組和 Account 設定  
現在，當您建立並設定，可以修改的受保護 （包括的最高特殊權限的 EA、 DA 及 BA 群組） 網域中的群組成員資格管理帳號，您應該確認帳號，並其管理群組有已建立正常運作。 這些一般工作驗證包含：  
  
1.  測試組可讓和驗證群組可的成員，讓和停用帳號和重設密碼，但無法執行其他管理活動管理帳號管理帳號停用。  
  
2.  測試管理帳號，驗證，可以將它們新增與移除成員受的網域中的群組，但無法變更其他任何屬性受保護的帳號及群組。  
  
###### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>測試，將會讓停用管理帳號群組  
  
1.  若要測試讓管理帳號，並其密碼重設，登入安全管理工作站為群組成員與您建立[附錄 i： 建立管理帳號 Active Directory 中的群組保護帳號，](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  開放**Active Directory 使用者和電腦**，以滑鼠右鍵按一下 [管理帳號，然後按一下**可讓 Account**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  應該會顯示對話方塊中，請確認帳號，已支援。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  接下來，重設管理 account 的密碼。 若要這樣做，請 account 上按一下滑鼠右鍵，然後按一下**重設密碼**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  輸入新密碼 account 中的**新密碼**和**確認密碼**欄位，然後按一下 [ **[確定]**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  確認已重設密碼帳號，應該會出現一個對話方塊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  現在，嘗試修改管理 account 的其他屬性。 Account 上按一下滑鼠右鍵，然後按一下**屬性**，按一下 [**遠端控制**索引標籤。  
  
8.  選取 [**可讓遠端控制**，按一下 [**套用]**。 操作應該會失敗並**存取**應該會顯示錯誤訊息。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. 按一下**Account**索引標籤帳號，並嘗試變更 account 的名稱、 登入小時的時間或登入工作站。 所有應該失敗，並考慮選項，不受**userAccountControl**屬性出並不適用於修改應該灰色。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. 嘗試將管理群組新增至受保護的 DA 群組例如群組。 當您按下**[確定]**，訊息應該會通知您，您不需要修改群組權限。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. 在確認您無法在以外管理 account 上設定的任何項目，才能執行其他測試**userAccountControl**設定和密碼重設。  
  
    > [!NOTE]  
    > **UserAccountControl**屬性控制項多個 account 設定選項。 您無法將變更的設定選項僅限部分時屬性寫入權限授與您的權限授與。  
  
###### <a name="test-the-management-accounts"></a>測試管理帳號  
既然您有支援，可變更的受保護的群組成員資格一或多個帳號，您可以測試帳號，以確保他們可以修改受保護的群組成員資格，但無法執行其他修改受保護的帳號，並群組。  
  
1.  第一次管理 account 登入安全管理主機。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  上市**Active Directory 使用者和電腦**，並找出**群組網域系統管理員 」**。  
  
3.  以滑鼠右鍵按一下**網域系統管理員**群組中，按一下 [**屬性**。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  在**網域管理員屬性**，按一下 [**成員**索引標籤和**按一下 [**新增。 輸入名稱帳號，將會提供暫時網域系統管理員權限，然後按一下**檢查名稱]**。 當底線 account 的名稱時，按一下**[確定]**以返回**成員**索引標籤。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  在**成員**索引標籤上的**網域管理員屬性**對話方塊中，按一下 [**套用**。 按一下 [後**套用]**，account 應該就會保持 DA 群組成員且應該會出現任何錯誤訊息。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  按一下**，受管理的**索引標籤中**網域管理員屬性**對話方塊方塊，然後確認您不能在任何欄位中輸入文字的所有按鈕呈現都灰色。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  按一下**一般**索引標籤中**網域管理員屬性**對話方塊中，並確認您無法修改任何該] 索引標籤的相關資訊。  
  
    ![建立管理帳號](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  視需要其他受保護的群組重複這些步驟。 當您完成時，請登入的群組成員 account 的安全的系統管理主機建立讓和管理帳號停用。 然後重設上管理您的帳號只測試 account 停用的密碼。 您已完成設定的管理帳號及負責讓和停用帳號群組。  
  


