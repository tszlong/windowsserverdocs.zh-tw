---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 附錄 I-在 Active Directory 中建立受保護帳戶和群組的管理帳戶
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c2141e4fad564579fd687b2dfc7e4a12e1634acb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823481"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附錄 I︰為 Active Directory 中的受保護帳戶和群組建立管理帳戶

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若不依賴高許可權群組中的永久成員資格，其中一項 Active Directory 挑戰就是在需要群組的暫時成員資格時，必須有一種機制來填入這些群組。 某些特殊許可權身分識別管理解決方案要求軟體的服務帳戶會被授與群組中的永久成員資格，例如在樹系中的每個網域中的 DA 或系統管理員。 不過，在技術上，Privileged Identity Management （PIM）解決方案不需要在這類高許可權的內容中執行其服務。  
  
本附錄提供的資訊可供您用來執行原生或協力廠商 PIM 解決方案，以建立具有有限許可權且可由而更加嚴格控制的帳戶，但可在需要暫時提高許可權時用來填入 Active Directory 中的特殊許可權群組。 如果您要將 PIM 實作為原生解決方案，系統管理人員可能會使用這些帳戶來執行臨時組擴展，如果您是透過協力廠商軟體來實行 PIM，則可以將這些帳戶調整成當做服務帳戶來運作。  
  
> [!NOTE]  
> 本附錄中所述的程式提供一種方法來管理 Active Directory 中的高許可權群組。 您可以調整這些程式以符合您的需求、新增其他限制，或省略這裡所述的一些限制。  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>在 Active Directory 中建立受保護帳戶和群組的管理帳戶

建立可用來管理許可權群組成員資格的帳戶，而不需要授與管理帳戶過多的許可權和許可權，包括下列的逐步指示所述的四個一般活動：  
  
1.  首先，您應該建立一個將管理帳戶的群組，因為這些帳戶應該由一組有限的受信任使用者來管理。 如果您還沒有 OU 結構，可以將特殊許可權和受保護的帳戶和系統從網域中的一般擴展中分離，您應該建立一個。 雖然本附錄並未提供特定指示，但螢幕擷取畫面會顯示這類 OU 階層的範例。  
  
2.  建立管理帳戶。 這些帳戶應建立為「一般」使用者帳戶，並授與已授與使用者的使用者權限（預設除外）。  
  
3.  除了控制可以啟用和使用帳戶的人員（您在第一個步驟中建立的群組）之外，也會對管理帳戶實行限制，使其僅適用于其所建立的特殊用途。  
  
4.  在每個網域中的 AdminSDHolder 物件上設定許可權，以允許管理帳戶變更網域中特殊許可權群組的成員資格。  
  
在生產環境中執行這些程式之前，您應該徹底測試所有這些程式，並根據環境的需要加以修改。 您也應該驗證所有設定是否如預期般運作（本附錄中提供一些測試程式），而且您應該測試嚴重損壞修復案例，其中管理帳戶無法用來填入受保護的群組以供復原之用。 如需備份和還原 Active Directory 的詳細資訊，請參閱[AD DS 備份和復原逐步指南](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx)。  
  
> [!NOTE]  
> 藉由執行本附錄所述的步驟，您將會建立帳戶，以管理每個網域中所有受保護群組的成員資格，而不只是最高許可權的 Active Directory 群組，例如 EAs、DAs 和 BAs。 如需 Active Directory 中受保護群組的詳細資訊，請參閱[附錄 C： Active Directory 中的受保護帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>為受保護的群組建立管理帳戶的逐步指示  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>建立群組以啟用和停用管理帳戶

管理帳戶應該會在每次使用時重設其密碼，而且應該在需要它們的活動完成時停用。 雖然您也可以考慮針對這些帳戶執行智慧卡登入需求，但這是選擇性的設定，而這些指示會假設管理帳戶會以使用者名稱和長的複雜密碼做為最小控制項來設定。 在此步驟中，您將建立一個群組，其具有在管理帳戶上重設密碼以及啟用和停用帳戶的許可權。  
  
若要建立群組來啟用及停用管理帳戶，請執行下列步驟：  
  
1.  在您要用來存放管理帳戶的 OU 結構中，以滑鼠右鍵按一下您要建立群組的 OU，按一下 [**新增**]，然後按一下 [**群組**]。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  在 [**新增物件-群組**] 對話方塊中，輸入群組的名稱。 如果您打算使用此群組來「啟用」樹系中的所有管理帳戶，請將它設為通用安全性群組。 如果您有單一網域樹系，或您打算在每個網域中建立群組，您可以建立全域安全性群組。 按一下 [確定] 以建立群組。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  以滑鼠右鍵按一下您剛才建立的群組，按一下 [**屬性**]，然後按一下 [**物件**] 索引標籤。在群組的 [**物件屬性**] 對話方塊中，選取 [**保護物件不被意外刪除**]，這不僅可以防止其他授權使用者刪除群組，還可以將它移至另一個 OU，除非第一次取消選取該屬性。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > 如果您已經設定群組父系 Ou 的許可權，將系統管理限制為一組有限的使用者，則可能不需要執行下列步驟。 這裡提供這些功能，即使您尚未對已建立此群組的 OU 結構執行有限的系統管理控制，您也可以保護群組免于未經授權的使用者修改。  
  
4.  按一下 [**成員**] 索引標籤，然後新增小組成員的帳戶，負責啟用管理帳戶或在必要時填入受保護的群組。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  如果您尚未這麼做，請在 [ **Active Directory 使用者和電腦**] 主控台中，按一下 [ **View** ]，然後選取 [ **Advanced Features**]。 在您剛才建立的群組上按一下滑鼠右鍵 **，按一下 [** 內容]，然後按一下 [**安全性**] 索引標籤。在 [**安全性**] 索引標籤上，按一下 [ **Advanced**]。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  在 **[[群組] 的 Advanced Security 設定]** 對話方塊中，按一下 [**停用繼承**]。 出現提示時，按一下 [**將繼承的許可權轉換成此物件的明確許可權**]，然後按一下 **[確定]** 以返回群組的 [**安全性**] 對話方塊。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  在 [**安全性**] 索引標籤上，移除不允許存取此群組的群組。 例如，如果您不想讓已驗證的使用者能夠讀取群組的名稱和一般屬性，您可以移除該 ACE。 您也可以移除 Ace，例如適用于帳戶操作員的和 Windows 2000 Server 相容的存取。 不過，您應該保留一組最小的物件使用權限。 讓下列 Ace 保持不變：  
  
    -   SELF  
  
    -   系統  
  
    -   Domain Admins  
  
    -   Enterprise Admins  
  
    -   Administrators  
  
    -   Windows 授權存取群組（如果適用）  
  
    -   企業網域控制站  
  
    雖然在 Active Directory 中允許最高許可權的群組來管理此群組，但您在執行這些設定時的目標並不是要防止這些群組的成員進行授權的變更。 相反地，目標是要確保當您需要非常高層級的許可權時，授權的變更將會成功。 這是因為這份檔不鼓勵您變更預設的特殊許可權群組嵌套、許可權和許可權。 藉由讓預設結構保持不變並清空目錄中最高許可權群組的成員資格，您就可以建立更安全的環境，但仍能如預期般運作。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > 如果您尚未在您建立此群組所在的 OU 結構中設定物件的稽核原則，您應該設定要記錄變更此群組的審核。  
  
8.  您已完成群組的設定，其會在需要時用來「簽出」管理帳戶，並在其活動完成時「簽入」帳戶。  
  
#### <a name="creating-the-management-accounts"></a>建立管理帳戶

您應該至少建立一個帳戶，用來管理 Active Directory 安裝中特殊許可權群組的成員資格，最好是使用第二個帳戶做為備份。 無論您選擇在樹系的單一網域中建立管理帳戶，並為所有網域的受保護群組授與管理功能，或者您選擇在樹系的每個網域中執行管理帳戶，這些程式實際上是相同的。  
  
> [!NOTE]  
> 本檔中的步驟假設您尚未針對 Active Directory 執行角色型存取控制和特殊許可權身分識別管理。 因此，某些程式必須由其帳戶為有問題之網域的 Domain Admins 群組成員的使用者執行。  
>   
> 當您使用具有 DA 許可權的帳戶時，您可以登入網域控制站來執行設定活動。 不需要 DA 許可權的步驟可由登入系統管理工作站的較低許可權帳戶執行。 顯示以較淺藍色色彩界定之對話方塊的螢幕擷取畫面，代表可在網域控制站上執行的活動。 以較暗藍色色彩顯示對話方塊的螢幕擷取畫面，代表可在具有有限許可權之帳戶的系統管理工作站上執行的活動。  
  
若要建立管理帳戶，請執行下列步驟：  
  
1. 使用網域的 DA 群組成員的帳戶登入網域控制站。  

2. 啟動**Active Directory 使用者和電腦**，然後流覽至您將在其中建立管理帳戶的 OU。  

3. 以滑鼠右鍵按一下 OU，然後按一下 [**新增**]，再按一下 [**使用者**]。  

4. 在 [**新增物件-使用者**] 對話方塊中，輸入您想要的帳戶命名資訊，然後按 **[下一步]** 。  

   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. 提供使用者帳戶的初始密碼、 **[清除使用者必須在下次登入時變更密碼]、[** 選取**使用者無法變更密碼**] 和 [**帳戶已停用**]，然後按 **[下一步]** 。  

   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. 確認帳戶詳細資料正確無誤，然後按一下 **[完成]** 。  

7. 以滑鼠右鍵按一下您剛建立的使用者物件，然後按一下 [**屬性**]。  

8. 按一下 [**帳戶**] 索引標籤。  

9. 在 [**帳戶選項**] 欄位中，選取 [此**帳戶為機密，無法委派**] 旗標，選取 [**此帳戶支援 kerberos aes 128 位加密**] 及/或 [**此帳戶支援 kerberos aes 256 加密**] 旗標，然後按一下 **[確定]** 。  

   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > 因為此帳戶與其他帳戶一樣，將具有有限但功能強大的功能，所以該帳戶只能用於安全的系統管理主機。 對於您環境中所有安全的系統管理主機，您應該考慮執行群組原則設定 [**網路安全性：設定 Kerberos 允許的加密類型**]，只允許您可以為安全主機執行的最安全加密類型。  
   >
   > 雖然為主機執行更安全的加密類型並不會減輕認證竊取攻擊，但安全主機的適當使用和設定也會受到保護。 為僅供特殊許可權帳戶使用的主機設定更強的加密類型，只會減少電腦的整體攻擊面。  
   >
   > 如需有關在系統和帳戶上設定加密類型的詳細資訊，請參閱[適用于 Kerberos 支援的加密類型的 Windows](https://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)設定。  
   >
   > 只有執行 Windows Server 2012、Windows Server 2008 R2、Windows 8 或 Windows 7 的電腦才支援這些設定。  
  
10. 在 [**物件**] 索引標籤上，選取 [**保護物件不被意外刪除**]。 這不只會防止刪除物件（即使是由授權的使用者），但會防止它移到 AD DS 階層中的不同 OU，除非有許可權變更屬性的使用者先清除此核取方塊。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. 按一下 [**遠端控制**] 索引標籤。  

12. 清除 [**啟用遠端控制**] 旗標。 支援人員不一定需要連線到此帳戶的會話來執行修正程式。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Active Directory 中的每個物件都應具有指定的 IT 擁有者和指定的商務擁有者，如[規劃危害](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)中所述。 如果您要追蹤 Active Directory 中 AD DS 物件的擁有權（而不是外部資料庫），您應該在此物件的屬性中輸入適當的擁有權資訊。  
    >
    > 在此情況下，商務擁有者很可能是 IT 部門，andthere 也不會禁止企業擁有者也是 IT 擁有者。 建立物件擁有權的重點是，可讓您在需要對物件進行變更時（可能是從初始建立的年份）識別連絡人。  

13. 按一下 [**組織**] 索引標籤。  

14. 輸入 AD DS 物件標準所需的任何資訊。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. 按一下 [**撥入**] 索引標籤。  

16. 在 [**網路存取權限**] 欄位中，選取 [**拒絕存取**]。此帳戶應該永遠不需要透過遠端連線進行連線。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > 此帳戶不太可能會用來登入您環境中的唯讀網域控制站（Rodc）。 不過，如果您需要帳戶登入 RODC，您應該將此帳戶新增到拒絕的 RODC 密碼複寫群組，使其密碼不會在 RODC 上快取。  
    >
    > 雖然應在每次使用後重設帳戶的密碼，且應停用帳戶，但執行此設定並不會對帳戶造成不利影響，而且在系統管理員忘記重設帳戶密碼並加以停用的情況下，可能會有所説明。  

17. 按一下 [隸屬於] 索引標籤。  

18. 按一下 [加入]。  

19. 在 [**選取使用者、連絡人、電腦**] 對話方塊中，輸入**拒絕的 RODC 密碼複寫群組**，然後按一下 [**檢查名稱**]。 當物件選擇器中的群組名稱加上底線時，按一下 **[確定]** ，並確認該帳戶現在是下列螢幕擷取畫面中所顯示的兩個群組的成員。 請勿將帳戶新增至任何受保護的群組。  

20. 按一下 [確定]。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. 按一下 [**安全性**] 索引標籤，然後按一下 [ **Advanced**]。  

22. 在 [**高級安全性設定**] 對話方塊中，按一下 [**停用繼承**]，並將繼承的許可權複製為明確許可權，然後按一下 [**新增**]。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. 在 **[帳戶] 對話方塊的 [許可權專案]** 中，按一下 [**選取主體**]，然後新增您在上一個程式中建立的群組。 移至對話方塊底部，然後按一下 [**全部清除**] 以移除所有預設許可權。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. 流覽至 [**許可權專案**] 對話方塊的頂端。 確定 [**類型**] 下拉式清單設為 [**允許**]，然後在 [**套用至**] 下拉式清單中選取 [**僅限此物件**]。  

25. 在 [**許可權**] 欄位中，選取 [**讀取所有屬性**]、[**讀取權限**] 和 [**重設密碼**]。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. 在 [**屬性**] 欄位中，選取 [**讀取 useraccountcontrol** ] 和 [**寫入 useraccountcontrol**]。  

27. 在 [**高級安全性設定**] 對話方塊中，再次按一下 [**確定**]、 **[確定]** 。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > **UserAccountControl**屬性會控制多個帳戶設定選項。 當您授與屬性的寫入權限時，您無法授與變更部分設定選項的許可權。  

28. 在 [**安全性**] 索引標籤的 [**群組或使用者名稱**] 欄位中，移除不應允許存取或管理帳戶的任何群組。 請勿移除已設定拒絕 Ace 的任何群組，例如 Everyone 群組和自我計算帳戶（在建立帳戶期間啟用 [**使用者無法變更密碼**] 旗標時，會設定該 ace）。 此外，請勿移除您剛新增的群組、系統帳戶，或 EA、DA、BA 或 Windows Authorization Access 群組等群組。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. 按一下 [ **advanced** ]，並確認 [Advanced Security Settings] 對話方塊看起來類似下列螢幕擷取畫面。  

30. 按一下 **[確定]** ，然後再按一次 **[確定**] 以關閉帳戶的 [屬性] 對話方塊。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. 第一個管理帳戶的設定現在已完成。 您將在稍後的程式中測試此帳戶。  

##### <a name="creating-additional-management-accounts"></a>建立其他管理帳戶

您可以重複上述步驟來建立其他管理帳戶，方法是複製您剛建立的帳戶，或建立腳本來建立具有所需設定的帳戶。 不過，請注意，如果您複製剛才建立的帳戶，許多自訂的設定和 Acl 將不會複製到新的帳戶，而且您必須重複大部分的設定步驟。  
  
您可以改為建立群組，以委派許可權來填入及 unpopulate 受保護的群組，但您必須保護群組和您在其中放置的帳戶。 因為您目錄中的帳戶應該會被授與管理受保護群組之成員資格的能力，所以建立個別帳戶可能是最簡單的方法。  
  
無論您選擇如何建立群組來放置管理帳戶，您都應該確保每個帳戶都受到保護，如先前所述。 您也應該考慮採用類似于 Active Directory 中的內[建系統管理員帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)中所述的 GPO 限制。  
  
##### <a name="auditing-management-accounts"></a>審核管理帳戶

您應該將帳戶的審核設定為至少記錄到帳戶的所有寫入。 如此一來，您不僅可以在授權使用期間識別成功啟用帳戶及重設其密碼，還可以識別未經授權的使用者嘗試動作帳戶的嘗試。 在您的安全性資訊和事件監視（SIEM）系統（如果適用）中，應會攔截帳戶上的失敗寫入，並且應該觸發警示，以向負責調查潛在危害的員工提供通知。  
  
SIEM 解決方案會從相關的安全性來源取得事件資訊（例如，事件記錄檔、應用程式資料、網路串流、反惡意程式碼產品，以及入侵偵測來源）、將資料定序，並嘗試進行智慧型視圖和主動式動作。 有許多商業 SIEM 解決方案，而許多企業都建立了私用的實施。 妥善設計且適當的 SIEM 可大幅增強安全性監視和事件回應功能。 不過，解決方案之間的功能和精確度會有極大的差異。 Siem 已超出本文的範圍，但所包含的特定事件建議應由任何 SIEM 實施者考慮。  
  
如需網域控制站之建議的 audit configuration 設定的詳細資訊，請參閱[監視 Active Directory 以取得危害的徵兆](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。 監視 Active Directory 會提供網域控制站特定的設定[，以因應危害的徵兆](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>啟用管理帳戶以修改受保護群組的成員資格

在此程式中，您將在網域的 AdminSDHolder 物件上設定許可權，以允許新建立的管理帳戶修改網域中受保護群組的成員資格。 這個程式無法透過圖形化使用者介面（GUI）來執行。  
  
如[附錄 C： Active Directory 中的受保護帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)中所述，當 SDProp 工作執行時，網域 AdminSDHolder 物件上的 ACL 會有效地「複製」到受保護的物件。 受保護的群組和帳戶不會從 AdminSDHolder 物件繼承其許可權;系統會明確設定其許可權，以符合 AdminSDHolder 物件上的許可權。 因此，當您修改 AdminSDHolder 物件的許可權時，必須針對您設為目標之受保護物件的類型適當地修改這些屬性。  
  
在此情況下，您將授與新建立的管理帳戶，讓他們能夠讀取和寫入群組物件上的 members 屬性。 不過，AdminSDHolder 物件不是群組物件，而且不會在圖形化 ACL 編輯器中公開群組屬性。 基於這個理由，您將透過 Dsacls 命令列公用程式來執行許可權變更。 若要授與（已停用）管理帳戶許可權以修改受保護群組的成員資格，請執行下列步驟：  
  
1. 使用已成為網域中的 DA 群組成員之使用者帳戶的認證，登入網域控制站，最好是持有 PDC 模擬器（PDCE）角色的網域控制站。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. 以滑鼠右鍵按一下 [**命令提示**字元]，然後按一下 [以**系統管理員身分執行**]，開啟提升許可權的命令提示  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. 當系統提示您核准提高許可權時，按一下 **[是]** 。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > 如需 Windows 中提高許可權和使用者帳戶控制（UAC）的詳細資訊，請參閱 TechNet 網站上的[UAC 處理常式和互動](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx)。  
  
4. 在命令提示字元中，輸入（取代您的網域特定資訊） **Dsacls [網域中 AdminSDHolder 物件的辨別名稱]/g [管理帳戶 UPN]： RPWP; 成員**。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   上一個命令（不區分大小寫）的運作方式如下所示：  
  
   - Dsacls 設定或顯示目錄物件上的 Ace  
  
   - CN = AdminSDHolder，CN = System，DC = TailSpinToys，DC = msft 識別要修改的物件  
  
   - /G 表示正在設定授與 ACE  
  
   - PIM001@tailspintoys.msft 是將授與 Ace 之安全性主體的使用者主要名稱（UPN）  
  
   - RPWP 授與讀取屬性和寫入屬性許可權  
  
   - 成員是將設定許可權之屬性（attribute）的名稱。  
  
   如需使用**Dsacls**的詳細資訊，請在命令提示字元中輸入不含任何參數的 Dsacls。  
  
   如果您已為網域建立多個管理帳戶，您應該針對每個帳戶執行 Dsacls 命令。 當您完成 AdminSDHolder 物件的 ACL 設定時，您應該強制執行 SDProp，或等到其排程的執行完成。 如需強制執行 SDProp 的相關資訊，請參閱[附錄 C： Active Directory 中的受保護帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)中的「手動執行 SDProp」。  
  
   當 SDProp 執行時，您可以確認您對 AdminSDHolder 物件所做的變更已套用到網域中的受保護群組。 您無法根據先前所述的原因，在 AdminSDHolder 物件上查看 ACL 來進行驗證，但您可以藉由在受保護的群組上查看 Acl 來確認許可權是否已套用。  
  
5. 在**Active Directory 使用者和電腦** 中，確認您已啟用  **Advanced 功能**。 若要這麼做，請按一下 [ **View**]，找出 [ **Domain Admins** ] 群組，在群組上按一下滑鼠右鍵，然後按一下 [**屬性**]。  
  
6. 按一下 [**安全性**] 索引標籤，然後按一下 [ **advanced** ] 以開啟 [ **Domain Admins 的 advanced Security 設定**] 對話方塊。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. **針對管理帳戶選取 [允許 ACE** ]，然後按一下 [**編輯**]。 確認帳戶已僅授與 [**讀取成員**] 和 [**寫入成員**] 許可權，然後按一下 **[確定]** 。  
  
8. 在 [**高級安全性設定**] 對話方塊中按一下 **[確定]** ，然後再按一下 **[確定**]，關閉 [DA] 群組的 [內容] 對話方塊。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. 您可以針對網域中的其他受保護群組重複上述步驟;所有受保護的群組的許可權應該都相同。 您現在已完成建立及設定此網域中受保護群組的管理帳戶。  
  
    > [!NOTE]  
    > 有權在 Active Directory 中寫入群組成員資格的任何帳戶，也可以將其本身新增至群組。 這是設計的行為，且無法停用。 基於這個理由，當管理帳戶不在使用中時，您應該一律保持停用狀態，而且應該在帳戶已停用且已在使用中時，密切地加以監視。  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>正在驗證群組和帳戶設定

現在您已建立並設定可修改網域中受保護群組成員資格的管理帳戶（包括最具特殊許可權的 EA、DA 和 BA 群組），您應該確認已正確建立帳戶和其管理群組。 驗證封裝含下列一般工作：  
  
1.  測試可啟用和停用管理帳戶的群組，以確認群組的成員可以啟用和停用帳戶，並重設其密碼，但無法在管理帳戶上執行其他系統管理活動。  
  
2.  測試管理帳戶，確認他們可以新增和移除網域中受保護群組的成員，但無法變更受保護帳戶和群組的任何其他屬性。  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>測試將啟用和停用管理帳戶的群組
  
1.  若要測試啟用管理帳戶及重設其密碼，請使用您在[附錄 I：在 Active Directory 中建立受保護帳戶和群組的管理帳戶](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)中所建立之群組成員的帳戶登入安全的系統管理工作站。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  開啟**Active Directory 使用者和電腦**，以滑鼠右鍵按一下管理帳戶，然後按一下 **啟用帳戶**。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  對話方塊應該會顯示，確認帳戶已啟用。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  接下來，重設管理帳戶的密碼。 若要這樣做，請再次以滑鼠右鍵按一下帳戶，然後按一下 [**重設密碼**]。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  在 [**新密碼**] 和 [**確認密碼**] 欄位中輸入帳戶的新密碼，然後按一下 **[確定]** 。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  對話方塊應該會出現，確認帳戶的密碼已重設。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  現在嘗試修改管理帳戶的其他屬性。 在**帳戶上按一下滑鼠右鍵，按一下**[內容]，然後按一下 [**遠端控制**] 索引標籤。  
  
8.  選取 [**啟用遠端控制**]，**然後按一下 [** 套用]。 作業應該會失敗，而且應該會顯示「**拒絕存取**」錯誤訊息。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. 按一下帳戶的 [**帳戶**] 索引標籤，然後嘗試變更帳戶的名稱、登入時間或登入工作站。 全部都應該失敗，而且不受**userAccountControl**屬性控制的帳戶選項應該呈現灰色且無法進行修改。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. 嘗試將管理群組新增至受保護的群組，例如 DA 群組。 當您按一下 **[確定]** 時，就會出現一則訊息，通知您沒有修改該群組的許可權。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. 視需要執行額外的測試，以確認您無法在管理帳戶上設定任何專案（ **userAccountControl**設定和密碼重設除外）。  
  
    > [!NOTE]  
    > **UserAccountControl**屬性會控制多個帳戶設定選項。 當您授與屬性的寫入權限時，您無法授與變更部分設定選項的許可權。  
  
##### <a name="test-the-management-accounts"></a>測試管理帳戶

現在您已啟用一或多個可變更受保護群組之成員資格的帳戶，您可以測試帳戶以確保他們可以修改受保護的群組成員資格，但無法對受保護的帳戶和群組執行其他修改。  
  
1.  以第一個管理帳戶的身分登入安全的系統管理主機。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  啟動**Active Directory 使用者和電腦**，並找出 **網域系統管理員 群組**。  
  
3.  在 [ **Domain Admins** ] 群組上按一下滑鼠右鍵，然後按一下 [**屬性**]。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  在 [ **Domain Admins] 屬性**中，按一下 [**成員**] 索引標籤，然後**按一下**[新增]。 輸入將提供臨時網域系統管理員許可權的帳戶名稱，然後按一下 [**檢查名稱**]。 當帳戶的名稱加上底線時，按一下 **[確定**] 以返回 [**成員**] 索引標籤。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  在 [**網域系統管理員屬性**] 對話方塊的 [**成員**] 索引標籤上 **，按一下 [** 套用]。 按一下 [套用] 之後，帳戶應會保持為 [DA] 群組的**成員，而且**您應該不會收到任何錯誤訊息。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  按一下 [**網域系統管理員屬性**] 對話方塊中的 [**受管理者**] 索引標籤，並確認您不能在任何欄位中輸入文字，而且所有按鈕都呈現灰色。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  按一下 [**網域系統管理員**內容] 對話方塊中的 [**一般**] 索引標籤，並確認您無法修改該索引標籤的任何相關資訊。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  視需要針對其他受保護的群組重複這些步驟。 當您完成時，請使用您所建立之群組的成員帳戶登入安全的系統管理主機，以啟用和停用管理帳戶。 然後在您剛測試的管理帳戶上重設密碼，並停用帳戶。 您已完成管理帳戶的設定，以及將負責啟用和停用帳戶的群組。  
