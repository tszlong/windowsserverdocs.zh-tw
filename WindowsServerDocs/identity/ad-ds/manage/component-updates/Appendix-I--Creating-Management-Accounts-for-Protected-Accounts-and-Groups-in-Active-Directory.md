---
description: 深入瞭解：附錄 I：在 Active Directory 中建立受保護帳戶和群組的管理帳戶
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 附錄 I-在 Active Directory 中建立受保護帳戶和群組的管理帳戶
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 905fd038223c11a012abf30f82d6b743ed04e511
ms.sourcegitcommit: e57536e28902ae52d3040141bbd2aa00e91bbdd3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644708"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附錄 I︰為 Active Directory 中的受保護帳戶和群組建立管理帳戶

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

執行不依賴高許可權群組永久成員資格之 Active Directory 模型的其中一項挑戰，就是在需要群組的暫存成員資格時，必須有一種機制來填入這些群組。 某些特殊許可權身分識別管理解決方案要求軟體的服務帳戶必須被授與樹系中每個網域的永久成員資格（例如 DA 或系統管理員）。 不過，在技術上，Privileged Identity Management (PIM) 解決方案並不需要在這類高度許可權的環境中執行其服務。

本附錄提供的資訊可讓您用於原生實行或協力廠商 PIM 解決方案來建立具有有限許可權的帳戶，而且可以 stringently 控制，但在需要暫時提高許可權時，可以用來填入 Active Directory 中的特殊許可權群組。 如果您要將 PIM 實作為原生解決方案，系統管理人員可以使用這些帳戶來執行暫存群組擴展，如果您是透過協力廠商軟體來實行 PIM，您可能可以調整這些帳戶以作為服務帳戶。

> [!NOTE]
> 本附錄中所述的程式提供一種方法來管理 Active Directory 中的高特殊許可權群組。 您可以調整這些程式以符合您的需求、新增額外的限制，或省略此處所述的一些限制。

## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>為 Active Directory 中的受保護帳戶和群組建立管理帳戶

建立可用來管理特殊許可權群組成員資格的帳戶，而不需要授與管理帳戶過多的許可權，而許可權則包含四個一般活動，這些活動會在後續的逐步指示中說明：

1.  首先，您應該建立將會管理帳戶的群組，因為這些帳戶應該由一組有限的受信任使用者來管理。 如果您還沒有 OU 結構，可容納從網域中的一般人口擴展具有特殊許可權和受保護的帳戶和系統，您應該建立一個。 雖然本附錄未提供特定的指示，但螢幕擷取畫面會顯示這類 OU 階層的範例。

2.  建立管理帳戶。 這些帳戶應該建立為「一般」使用者帳戶，而且除了預設已授與使用者的許可權以外，不會授與使用者權利。

3.  除了控制誰可以啟用和使用您在第一個步驟) 中所建立之群組 (的帳戶之外，還能對管理帳戶執行限制，使其僅適用于建立它們的特殊用途。

4.  設定每個網域中 AdminSDHolder 物件的許可權，以允許管理帳戶變更網域中特殊許可權群組的成員資格。

在生產環境中執行這些程式之前，您應該徹底測試這些程式，並視您的環境需要加以修改。 您也應該確認所有設定都能如預期般運作 (本附錄) 提供某些測試程式，而且您應該測試嚴重損壞修復案例，在此情況下，管理帳戶無法用於填入受保護的群組以進行復原。 如需備份和還原 Active Directory 的詳細資訊，請參閱 [AD DS 備份和復原的逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771290(v=ws.10))。

> [!NOTE]
> 藉由執行本附錄所述的步驟，您將會建立帳戶，以便在每個網域中管理所有受保護群組的成員資格，而不只是最高許可權的 Active Directory 群組，例如 EAs、DAs 和 BAs。 如需 Active Directory 中受保護群組的詳細資訊，請參閱 [附錄 C： Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。

### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>針對受保護群組建立管理帳戶的逐步指示

#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>建立群組以啟用和停用管理帳戶

管理帳戶應該在每次使用時重設其密碼，且應在需要它們的活動完成時停用。 雖然您也可以考慮針對這些帳戶執行智慧卡登入要求，但這是選擇性的設定，而這些指示會假設管理帳戶會設定為使用使用者名稱和長密碼的複雜密碼做為最小控制。 在此步驟中，您將建立具有在管理帳戶上重設密碼的許可權，以及啟用和停用帳戶的群組。

若要建立群組來啟用和停用管理帳戶，請執行下列步驟：

1.  在您要用來存放管理帳戶的 OU 結構中，以滑鼠右鍵按一下您要建立群組的 OU，按一下 [ **新增** ]，然後按一下 [ **群組**]。

    ![顯示如何選取 [群組] 功能表選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)

2.  在 [ **新增物件-群組** ] 對話方塊中，輸入群組的名稱。 如果您打算使用此群組來「啟動」樹系中的所有管理帳戶，請將它設為通用安全性群組。 如果您有單一網域樹系，或您打算在每個網域中建立群組，您可以建立一個全域安全性群組。 按一下 [確定] 以建立群組。

    ![顯示在 [新增物件-群組] 對話方塊中輸入組名之位置的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)

3.  以滑鼠右鍵按一下您剛才建立的群組，按一下 [ **屬性**]，然後按一下 [ **物件** ] 索引標籤。在群組的 [ **物件** 內容] 對話方塊中，選取 [ **保護物件不被意外刪除**]，這不僅會防止以其他授權的使用者刪除群組，還會將它移到另一個 OU，除非先取消選取該屬性。

    ![顯示 [物件] 索引標籤的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)

    > [!NOTE]
    > 如果您已經在群組的父 Ou 上設定許可權，以將系統管理限制在一組有限的使用者，您可能不需要執行下列步驟。 在這裡提供它們，即使您尚未對建立此群組的 OU 結構執行有限的系統管理控制，也可以保護群組免于未經授權的使用者修改。

4.  按一下 [ **成員** ] 索引標籤，然後為您的小組成員新增帳戶，這些成員將負責啟用管理帳戶，或在必要時填入受保護的群組。

    ![顯示 [成員] 索引標籤上之帳戶的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)

5.  如果您尚未這麼做，請在 **Active Directory 消費者和電腦** 主控台中，按一下 [ **View** ]，然後選取 [ **Advanced Features （Advanced Features**）]。 以滑鼠右鍵按一下您剛才建立的群組，按一下 [ **屬性**]，然後按一下 [ **安全性** ] 索引標籤。在 [ **安全性** ] 索引標籤上，按一下 [ **Advanced**]。

    ![顯示 [安全性] 索引標籤上 [Advanced] 按鈕的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)

6.  在 **[群組] 對話方塊的 [安全性設定]** 對話方塊中，按一下 [ **停用繼承**]。 出現提示時，按一下 [ **將繼承的許可權轉換成這個物件的明確許可權**]，然後按一下 **[確定** ] 返回群組的 [ **安全性** ] 對話方塊。

    ![顯示 [將繼承的許可權轉換成這個物件的明確許可權] 選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)

7.  在 [ **安全性** ] 索引標籤上，移除不允許存取此群組的群組。 例如，如果您不想讓經過驗證的使用者能夠讀取群組的名稱和一般屬性，您可以移除該 ACE。 您也可以移除 Ace，例如帳戶操作員的和 Windows Server 相容的 2000 Windows Server 相容存取。 不過，您應該保留最小的物件使用權限集。 將下列 Ace 保持不變：

    -   SELF

    -   系統

    -   Domain Admins

    -   Enterprise Admins

    -   系統管理員

    -   Windows 授權存取群組 (（如果適用）) 

    -   企業網域控制站

    雖然在 Active Directory 中允許最高許可權的群組來管理此群組似乎違反直覺，但您在實施這些設定時的目標不是要防止這些群組的成員進行授權的變更。 相反地，目標是要確保當您需要非常高等級的許可權時，授權的變更將會成功。 基於這個原因，我們不建議您在這份檔中變更預設的特殊許可權群組的嵌套、許可權和許可權。 藉由讓預設結構保持不變，並在目錄中清空最高許可權群組的成員資格，您可以建立更安全的環境，但仍會如預期般運作。

    ![顯示 [已驗證的使用者] 區段之許可權的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)

    > [!NOTE]
    > 如果您尚未針對您在其中建立此群組的 OU 結構中的物件設定稽核原則，您應該將 [審核] 設定為記錄變更此群組。

8.  您已完成群組的設定，此群組將用來在需要時「簽出」管理帳戶，並在其活動完成時「簽入」帳戶。

#### <a name="creating-the-management-accounts"></a>建立管理帳戶

您應該至少建立一個帳戶，用來管理 Active Directory 安裝中特殊許可權群組的成員資格，而且最好是第二個帳戶做為備份。 無論您選擇在樹系中的單一網域中建立管理帳戶，並為所有網域的受保護群組授與管理帳戶，或者您是否選擇在樹系的每個網域中執行管理帳戶，程式都是相同的。

> [!NOTE]
> 本檔中的步驟假設您尚未針對 Active Directory 執行角色型存取控制和特殊許可權身分識別管理。 因此，某些程式必須由其帳戶是有問題之網域的 Domain Admins 群組成員的使用者執行。
>
> 當您使用具有 DA 許可權的帳戶時，您可以登入網域控制站來執行設定活動。 不需要 DA 許可權的步驟可由登入系統管理工作站的較低許可權帳戶來執行。 顯示以較淺藍色表示之對話方塊的螢幕擷取畫面，代表可以在網域控制站上執行的活動。 以深藍色顯示對話方塊的螢幕擷取畫面代表可在具有有限許可權帳戶的系統管理工作站上執行的活動。

若要建立管理帳戶，請執行下列步驟：

1. 使用網域之 DA 群組成員的帳戶登入網域控制站。

2. 啟動 **Active Directory 消費者和電腦** ，然後流覽至您將在其中建立管理帳戶的 OU。

3. 以滑鼠右鍵按一下 OU，然後按一下 [ **新增** ]，再按一下 [ **使用者**]。

4. 在 [ **新增物件-使用者** ] 對話方塊中，輸入您想要的帳戶命名資訊，然後按 **[下一步]**。

   ![顯示輸入命名資訊之位置的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)

5. 提供使用者帳戶的初始密碼，清除 **[使用者必須在下次登入時變更密碼]**，選取 [ **使用者不能變更密碼** ] 和 [ **帳戶已停用**]，然後按 **[下一步]**。

   ![顯示要在哪裡提供初始密碼的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)

6. 確認帳戶詳細資料正確無誤，然後按一下 **[完成]**。

7. 以滑鼠右鍵按一下您剛才建立的使用者物件，然後按一下 [ **屬性**]。

8. 按一下 [帳戶]  索引標籤。

9. 在 [ **帳戶選項** ] 欄位中，選取 [ **帳戶是機密的，無法委派** ] 旗標，選取 [ **此帳戶支援 kerberos aes 128 位加密** ] 和/或 [ **此帳戶支援 kerberos aes 256 加密** 旗標]，然後按一下 **[確定]**。

   ![顯示您應該選取之選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)

   > [!NOTE]
   > 因為此帳戶（例如其他帳戶）將會有有限但功能強大的功能，所以帳戶只能用於安全的系統管理主機。 針對環境中的所有安全系統管理主機，您應該考慮實施群組原則設定 **網路安全性：設定 Kerberos 允許的加密類型** ，以只允許您可針對安全主機執行的最安全加密類型。
   >
   > 雖然為主機執行更安全的加密類型，並不會減輕認證遭竊的攻擊，但適當的安全主機使用和設定。 為只由特殊許可權帳戶使用的主機設定更強的加密類型，只是減少電腦的整體受攻擊面。
   >
   > 如需有關在系統與帳戶上設定加密類型的詳細資訊，請參閱 [適用于 Kerberos 支援之加密類型的 Windows](/archive/blogs/openspecification/windows-configurations-for-kerberos-supported-encryption-type)設定。
   >
   > 只有在執行 Windows Server 2012、Windows Server 2008 R2、Windows 8 或 Windows 7 的電腦上才支援這些設定。

10. 在 [ **物件** ] 索引標籤上，選取 [ **保護物件不被意外刪除**]。 這不僅會防止物件 (刪除) 的授權使用者，還會防止該物件被移至 AD DS 階層中的不同 OU，除非使用者先清除該核取方塊，且該使用者具有變更屬性的許可權。

    ![螢幕擷取畫面，顯示 [物件] 索引標籤上的 [保護物件不被意外刪除] 選項。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)

11. 按一下 [ **遠端控制** ] 索引標籤。

12. 清除 [ **啟用遠端控制** ] 旗標。 支援人員必須永遠不需要連線到此帳戶的會話，才能執行修正。

    ![顯示已清除的 [啟用遠端控制] 選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)

    > [!NOTE]
    > Active Directory 中的每個物件都應該具有指定的 IT 擁有者和指定的商務擁有者（如 [規劃入侵](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)所述）。 如果您要追蹤 Active Directory (中 AD DS 物件的擁有權，而不是外部資料庫) ，您應該在此物件的屬性中輸入適當的擁有權資訊。
    >
    > 在此情況下，業務擁有者很可能是 IT 部門，andthere 對企業擁有者也是 IT 擁有者。 建立物件擁有權的重點是，可讓您在需要對物件進行變更時識別連絡人，可能是從最初建立以來的年份。

13. 按一下 [ **組織** ] 索引標籤。

14. 輸入 AD DS 物件標準所需的任何資訊。

    ![顯示您的 AD DS 物件標準中所需資訊的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)

15. 按一下 [ **撥入** ] 索引標籤。

16. 在 [ **網路存取權限** ] 欄位中，選取 [ **拒絕存取**]。此帳戶永遠不需要透過遠端連線來連接。

    ![顯示 [拒絕存取] 選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)

    > [!NOTE]
    > 此帳戶不太可能會用來登入您環境中 (Rodc) 的唯讀網域控制站。 不過，萬一您需要登入 RODC 的帳戶，您應該將此帳戶新增至拒絕的 RODC 密碼複寫群組，如此就不會在 RODC 上快取其密碼。
    >
    > 雖然應在每次使用後重設帳戶的密碼，且應該停用帳戶，但實行此設定並不會對帳戶造成不利的影響，且在系統管理員忘記重設帳戶密碼並予以停用的情況下可能有所説明。

17. 按一下 [隸屬於] 索引標籤。

18. 按一下 [新增] 。

19. 在 [**選取使用者、連絡人、電腦**] 對話方塊中，輸入 **拒絕的 RODC 密碼複寫群組**，然後按一下 [**檢查名稱**]。 當物件選擇器中的群組名稱加上底線時，請按一下 **[確定]** ，並確認該帳戶現在是在下列螢幕擷取畫面中顯示的兩個群組的成員。 請勿將帳戶新增到任何受保護的群組。

20. 按一下 [確定]。

    ![顯示 [確定] 按鈕的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)

21. 按一下 [ **安全性** ] 索引標籤，然後按一下 [ **Advanced**]。

22. 在 [ **Advanced Security Settings** ] 對話方塊中，按一下 [ **停用繼承** ]，並將繼承的許可權複製為明確許可權，然後按一下 [ **新增**]。

    ![顯示 [封鎖繼承] 對話方塊的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)

23. 在 **[帳戶] 對話方塊的 [許可權專案]** 對話方塊中，按一下 [ **選取主體** ]，然後新增您在先前程式中建立的群組。 滾動至對話方塊底部，然後按一下 [ **全部清除** ] 以移除所有預設許可權。

    ![顯示 [全部清除] 按鈕的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)

24. 滾動至 [ **許可權專案** ] 對話方塊的頂端。 確定 [ **類型** ] 下拉式清單已設定為 [ **允許**]，然後在 [ **套用至** ] 下拉式清單中，選取 [ **僅限這個物件**]。

25. 在 [ **許可權** ] 欄位中，選取 [ **讀取所有屬性**]、[ **讀取權限**] 和 [ **重設密碼**]。

    ![顯示 [讀取所有屬性]、[讀取權限] 和 [重設密碼] 選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)

26. 在 [ **屬性** ] 欄位中，選取 [ **讀取 userAccountControl** 並 **寫入 useraccountcontrol**]。

27. 按一下 [ **Advanced Security Settings** ] 對話方塊中的 [**確定** **]，再按一下 [確定]** 。

    ![顯示 [Advanced Security Settings] 對話方塊中 [確定] 按鈕的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)

    > [!NOTE]
    > **UserAccountControl** 屬性會控制多個帳戶設定選項。 當您授與屬性的寫入權限時，無法授與變更部分設定選項的許可權。

28. 在 [**安全性**] 索引標籤的 [**群組或使用者名稱**] 欄位中，移除不允許存取或管理帳戶的任何群組。 請勿移除使用 Deny Ace 設定的任何群組，例如 Everyone 群組和自我計算帳戶 (當 **使用者無法變更密碼** 旗標在帳戶建立期間啟用時，會設定 ACE。 此外，請勿移除您剛剛新增的群組、系統帳戶，或是 EA、DA、BA 或 Windows 授權存取群組等群組。

    ![顯示 [安全性] 索引標籤上 [群組或使用者名稱] 區段的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)

29. 按一下 [ **advanced** ]，並確認 [Advanced Security Settings] 對話方塊看起來類似下列螢幕擷取畫面。

30. 按一下 **[確定**]，然後再按一次 **[確定** ]，關閉帳戶的屬性對話方塊。

    ![顯示 [Advanced Security Settings] 對話方塊的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)

31. 第一個管理帳戶的設定現已完成。 您將在稍後的程式中測試帳戶。

##### <a name="creating-additional-management-accounts"></a>建立其他管理帳戶

您可以藉由複製剛才建立的帳戶，或建立腳本來建立具有所需設定的帳戶，藉以建立其他管理帳戶。 不過，請注意，如果您複製剛才建立的帳戶，許多自訂設定和 Acl 將不會複製到新的帳戶，而您必須重複大部分的設定步驟。

您可以改為建立群組，讓您委派許可權來填入及 unpopulate 受保護的群組，但您必須保護群組以及您在其中放置的帳戶。 因為您目錄中的帳戶應該會被授與管理受保護群組成員資格的能力，所以建立個別帳戶可能是最簡單的方法。

無論您選擇如何建立群組來放置管理帳戶，您都應該確定每個帳戶的安全，如稍早所述。 您也應該考慮採用類似于 [附錄 D：保護 Built-In 系統管理員帳戶 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)中所述的 GPO 限制。

##### <a name="auditing-management-accounts"></a>審核管理帳戶

您應該在帳戶上設定審核，以記錄至少所有寫入帳戶的資料。 這可讓您不只識別成功啟用帳戶，並在授權使用期間重設其密碼，同時也會識別未經授權的使用者動作帳戶的嘗試。 在您的安全性資訊和事件監視 (SIEM) 系統 (（如果適用的) ）中，應該會產生失敗的寫入，並觸發警示，以提供通知給負責調查潛在危害的人員。

SIEM 解決方案會從相關的安全性來源取得事件資訊 (例如事件記錄檔、應用程式資料、網路串流、反惡意程式碼產品和入侵偵測來源) 、將資料排序，以及嘗試進行智慧型視圖和主動式動作。 有許多商業 SIEM 解決方案，而許多企業都建立私用。 妥善設計和適當實施的 SIEM 可大幅增強安全性監視和事件回應功能。 不過，解決方案之間的功能和精確度會有極大的差異。 Siem 已超出本檔的範圍，但所包含的特定事件建議應由任何 SIEM 實施者來考慮。

如需網域控制站建議的 audit configuration 設定的詳細資訊，請參閱 [監視 Active Directory 的入侵徵兆](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。 監視 Active Directory 中提供網域控制站專屬的設定 [，以因應入侵的徵兆](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。

#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>啟用管理帳戶以修改受保護群組的成員資格

在此程式中，您將設定網域 AdminSDHolder 物件的許可權，以允許新建立的管理帳戶修改網域中受保護群組的成員資格。 此程式無法透過圖形化使用者介面 (GUI) 來執行。

如 [附錄 C： Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)所述，在 SDProp 工作執行時，網域 AdminSDHolder 物件上的 ACL 實際上會「複製」到受保護的物件。 受保護的群組和帳戶不會繼承其 AdminSDHolder 物件的許可權;其許可權會明確設定為符合 AdminSDHolder 物件上的許可權。 因此，當您修改 AdminSDHolder 物件的許可權時，必須針對適用于您目標受保護物件類型的屬性修改它們。

在此情況下，您將授與新建立的管理帳戶，讓他們能夠讀取和寫入群組物件的成員屬性。 不過，AdminSDHolder 物件不是群組物件，而且不會在圖形化 ACL 編輯器中公開群組屬性。 基於這個原因，您將透過 Dsacls 命令列公用程式來執行許可權變更。 若要授與 (已停用的) 管理帳戶許可權，以修改受保護群組的成員資格，請執行下列步驟：

1. 使用已成為網域中 DA 群組成員之使用者帳戶的認證，來登入網域控制站，最好是持有 PDC 模擬器 (PDCE) 角色的網域控制站。

   ![螢幕擷取畫面：顯示使用者帳號憑證的輸入位置。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)

2. 以滑鼠右鍵按一下 **命令提示** 字元，然後按一下 [以 **系統管理員身分執行**]，開啟提升許可權的命令提示字元。

   ![顯示 [以系統管理員身分執行] 功能表選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)

3. 當系統提示您核准提高許可權時，請按一下 **[是]**。

   ![顯示要在哪裡選取 [是] 以核准提升許可權的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)

   > [!NOTE]
   > 如需 Windows 中的「提高許可權」和「使用者帳戶控制」 (UAC) 的詳細資訊，請參閱 TechNet 網站上的 [Uac 處理常式和互動](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd835561(v=ws.10)) 。

4. 在命令提示字元中，輸入 (替代您的網域特定資訊) **Dsacls [網域中 AdminSDHolder 物件的分辨名稱]/g [管理帳戶 UPN]： RPWP; 成員**。

   ![顯示命令提示字元的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)

   先前的命令 (不區分大小寫) 的運作方式如下：

   - Dsacls 設定或顯示目錄物件上的 Ace

   - CN = AdminSDHolder，CN = System，DC = TailSpinToys，DC = msft 識別要修改的物件

   - /G 表示正在設定 grant ACE

   - PIM001@tailspintoys.msft 這是將授與 Ace 之安全性主體 (UPN) 的使用者主體名稱

   - RPWP 授與 read 屬性和寫入屬性許可權

   - 成員是將設定許可權 (屬性) 的名稱。

   如需使用 **Dsacls** 的詳細資訊，請在命令提示字元中輸入不含任何參數的 Dsacls。

   如果您已為網域建立多個管理帳戶，則應該為每個帳戶執行 Dsacls 命令。 當您完成 AdminSDHolder 物件的 ACL 設定時，您應該強制 SDProp 執行，或等候直到其排程執行完成為止。 如需強制執行 SDProp 的相關資訊，請參閱 [附錄 C： Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)中的「手動執行 SDProp」。

   當 SDProp 執行時，您可以確認您對 AdminSDHolder 物件所做的變更已套用到網域中的受保護群組。 您無法根據先前所述的原因在 AdminSDHolder 物件上查看 ACL 來確認這一點，但您可以藉由在受保護群組上查看 Acl 來確認是否已套用許可權。

5. 在 **Active Directory 消費者和電腦** 中，確認您已啟用 [ **Advanced] 功能**。 若要這樣做，請按一下 [ **View**]，找出 **Domain Admins** 群組，以滑鼠右鍵 **按一下群組**，然後按一下 [內容]。

6. 按一下 [ **安全性** ] 索引標籤，然後按一下 [ **advanced** ] 開啟 [ **Domain Admins 的 advanced Security 設定** ] 對話方塊。

   ![顯示如何開啟 [Domain Admins 的 Advanced Security 設定] 對話方塊的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)

7. 選取 **[允許管理帳戶的 ACE** ]，然後按一下 [ **編輯**]。 確認帳戶是否已被授與 DA 群組的 [ **讀取成員** ] 和 [ **寫入成員** ] 許可權，然後按一下 **[確定]**。

8. 按一下 [ **Advanced Security Settings** ] 對話方塊中的 **[確定**]，然後再按一下 **[確定**]，關閉 DA 群組的 [內容] 對話方塊。

   ![顯示如何關閉 [屬性] 對話方塊的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)

9. 您可以針對網域中的其他受保護群組重複上述步驟;所有受保護群組的許可權都應該相同。 您現在已完成建立及設定此網域中受保護群組的管理帳戶。

    > [!NOTE]
    > 有權在 Active Directory 中寫入群組成員資格的任何帳戶，也可以將自己新增至群組。 這是設計的行為，無法停用。 基於這個理由，您應該一律讓管理帳戶在不使用時保持停用狀態，而且應該在帳戶停用和使用中時，仔細地監視這些帳戶。

#### <a name="verifying-group-and-account-configuration-settings"></a>正在驗證群組和帳戶設定

現在您已建立並設定可修改網域中受保護群組成員資格的管理帳戶 (其中包含) 的最高許可權 EA、DA 和 BA 群組，您應該確認已正確建立帳戶及其管理群組。 驗證是由下列一般工作所組成：

1.  測試可以啟用和停用管理帳戶的群組，以確認群組的成員可以啟用和停用帳戶，以及重設其密碼，但無法在管理帳戶上執行其他系統管理活動。

2.  測試管理帳戶，確認它們可以新增和移除網域中受保護群組的成員，但無法變更受保護帳戶和群組的任何其他屬性。

##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>測試將啟用和停用管理帳戶的群組

1.  若要測試啟用管理帳戶並重設其密碼，請使用您在 [附錄 I：為 Active Directory 中的受保護帳戶和群組建立管理帳戶](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)的帳戶，登入安全的系統管理工作站。

    ![顯示如何登入屬於您所建立群組成員之帳戶的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)

2.  開啟 **Active Directory 消費者和電腦**，以滑鼠右鍵按一下管理帳戶，然後按一下 [ **啟用帳戶**]。

    ![醒目顯示 [啟用帳戶] 功能表選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)

3.  對話方塊應該會顯示，確認帳戶已啟用。

    ![顯示已啟用帳戶的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)

4.  接下來，在管理帳戶上重設密碼。 若要這樣做，請再次以滑鼠右鍵按一下帳戶，然後按一下 [ **重設密碼**]。

    ![醒目顯示 [重設密碼] 功能表選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)

5.  在 [ **新密碼** ] 和 [ **確認密碼** ] 欄位中輸入帳戶的新密碼，然後按一下 **[確定]**。

    ![顯示輸入新密碼之位置的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)

6.  應該會出現對話方塊，確認帳戶的密碼已重設。

    ![顯示確認帳戶密碼已重設之訊息的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)

7.  現在嘗試修改管理帳戶的其他屬性。 以滑鼠右鍵按一下帳戶，然後按一下 [ **屬性**]，再按一下 [ **遠端控制** ] 索引標籤。

8.  選取 [ **啟用遠端控制** ]， **然後按一下 [** 套用]。 作業應該會失敗，而且應該會顯示 **拒絕存取** 錯誤訊息。

    ![顯示拒絕存取錯誤的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)

9. 按一下帳戶的 [ **帳戶** ] 索引標籤，並嘗試變更帳戶的名稱、登入時間或登入工作站。 全都應該會失敗，而且不受 **userAccountControl** 屬性控制的帳戶選項應該會呈現灰色且無法進行修改。

    ![顯示 [帳戶] 索引標籤的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)

10. 嘗試將管理群組新增至受保護的群組，例如 DA 群組。 當您按一下 **[確定]** 時，就會出現一則訊息，通知您沒有修改該群組的許可權。

    ![顯示訊息通知您沒有修改群組許可權的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)

11. 視需要執行其他測試，以確認您無法在管理帳戶上進行任何設定，但 **userAccountControl** 設定和密碼重設除外。

    > [!NOTE]
    > **UserAccountControl** 屬性會控制多個帳戶設定選項。 當您授與屬性的寫入權限時，無法授與變更部分設定選項的許可權。

##### <a name="test-the-management-accounts"></a>測試管理帳戶

現在您已啟用一或多個可以變更受保護群組成員資格的帳戶，您可以測試帳戶以確保他們可以修改受保護的群組成員資格，但無法對受保護的帳戶和群組執行其他修改。

1.  以第一個管理帳戶的形式登入安全的系統管理主機。

    ![顯示如何登入安全系統管理主機的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)

2.  啟動 **Active Directory 消費者和電腦** ，並找出 **Domain Admins 群組**。

3.  以滑鼠右鍵按一下 [ **Domain Admins** ] 群組， **然後按一下 [** 內容]。

    ![醒目顯示 [屬性] 功能表選項的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)

4.  在 [ **Domain Admins] 屬性** 中，按一下 [ **成員** ] 索引標籤，然後 **按一下** [新增]。 輸入將授與暫時性 Domain Admins 許可權的帳戶名稱，然後按一下 [ **檢查名稱**]。 當帳戶的名稱加上底線時，請按一下 **[確定** ] 返回 [ **成員** ] 索引標籤。

    ![顯示在何處加入將授與暫時性 Domain Admins 許可權之帳戶名稱的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)

5.  在 [**網域系統管理員屬性**] 對話方塊的 [**成員**] 索引標籤上 **，按一下 [** 套用]。 按一下 [套用] 之後，帳戶應該會保留在 DA 群組的 **成員，而且** 您應該不會收到任何錯誤訊息。

    ![螢幕擷取畫面，顯示 [Domain Admins 屬性] 對話方塊中的 [成員] 索引標籤。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)

6.  按一下 [ **Domain Admins 屬性**] 對話方塊中的 [**管理者**] 索引標籤，並確認您無法在任何欄位中輸入文字，而且所有按鈕都呈現灰色。

    ![顯示 [管理者] 索引標籤的螢幕擷取畫面。](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)

7.  按一下 [**網域管理員** 內容] 對話方塊中的 [**一般**] 索引標籤，並確認您無法修改該索引標籤的任何相關資訊。

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)

8.  視需要針對其他受保護的群組重複這些步驟。 當您完成時，請使用您所建立之群組的成員帳戶登入安全的系統管理主機，以啟用和停用管理帳戶。 然後在您剛剛測試的管理帳戶上重設密碼，並停用該帳戶。 您已完成管理帳戶的設定，以及將負責啟用和停用帳戶的群組。
