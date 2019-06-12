---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 附錄 I-建立受保護的帳戶和 Active Directory 中的群組的管理帳戶
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e90c2c075ba2dc2b63e9a18c9eba192116265b90
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443521"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附錄 i:建立管理帳戶的受保護的帳戶和 Active Directory 中的群組

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

實作不需要高特殊權限的群組中的永久成員資格的 Active Directory 模型的挑戰之一是必須有一個機制，需要在群組中的暫時成員資格時填入這些群組。 某些特殊權限的身分識別管理解決方案都需要軟體的服務帳戶會授與群組，例如 DA 或樹系中每個網域中的系統管理員的永久成員資格。 不過，它不技術上需要的 Privileged Identity Management (PIM) 解決方案，在這類高特殊權限的內容中執行其服務。  
  
本附錄提供您可以用原生實作或第三方 PIM 解決方案來建立具有有限的權限，也能嚴格控制，但可以用來填入 Active Directory 中的特殊權限的群組的帳戶資訊時需要暫時提高權限。 如果您要實作以原生解決方案的 PIM，這些帳戶可用的系統管理人員來執行暫存群組母體擴展，以及如果您透過協力廠商軟體實作 PIM，您可以調整這些帳戶加入做為服務帳戶。  
  
> [!NOTE]  
> 本附錄中所述的程序提供的 Active Directory 中具有高度權限的群組管理的其中一個方法。 您可以調整以符合您的需求，加入額外的限制，這些程序，或省略一些此處所述的限制。  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>建立管理帳戶的受保護的帳戶和 Active Directory 中的群組

建立帳戶，可以用來管理特殊權限群組的成員資格，不需要管理帳戶過多的權利與權限授與包含下列四個一般的活動所述的逐步指示，請依照下列：  
  
1.  首先，您應該建立的群組，將管理帳戶，因為這些帳戶應由一組有限的受信任的使用者。 如果您還沒有適用於將特殊權限和受保護的帳戶和系統一般的母體擴展，在網域中的 OU 結構，您應該建立一個。 雖然本附錄中未提供特定的指示，螢幕擷取畫面會顯示這類的 OU 階層的範例。  
  
2.  建立管理帳戶。 這些帳戶應該建立為 「 一般 」 的使用者帳戶，而且沒有以外已經預設會授與給使用者的使用者權限授與。  
  
3.  實作限制，讓它們可為其所建立，除了控制誰可以啟用及使用的帳戶 （您在第一個步驟中建立的群組） 的特殊用途的僅適用於管理帳戶。  
  
4.  若要允許變更網域中的特殊權限群組的成員資格管理帳戶的每個網域中的 AdminSDHolder 物件上設定權限。  
  
您應該徹底測試所有的這些程序，並視需要修改這些為您的環境之前在生產環境中執行。 您也應該確認所有的設定會在如預期般運作 （某些測試程序本附錄中提供），您應該先測試災害復原案例中管理帳戶並不適用於用來填入受保護的群組復原用途。 如需有關備份和還原 Active Directory 的詳細資訊，請參閱 < [AD DS 備份與復原逐步指南 》](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx)。  
  
> [!NOTE]  
> 藉由實作本附錄中所述的步驟，您會建立將能夠管理所有受保護的群組，每個網域中，不只有最高特殊權限 Active Directory 群組 EAs、 DAs 等 BAs 的成員資格的帳戶。 如需有關 Active Directory 中的受保護群組的詳細資訊，請參閱[附錄 c:受保護的帳戶和 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>建立受保護群組的管理帳戶的逐步指示  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>建立群組，以啟用和停用管理帳戶

管理帳戶應該具有在每次使用重設其密碼，而且應該停用時需要這些活動都已完成。 雖然您也可以考慮實作這些帳戶的智慧卡登入需求，它是選擇性的設定與這些指示假設管理帳戶會使用使用者名稱和最小值為長而複雜密碼進行設定控制項。 在此步驟中，您將建立具有上管理帳戶密碼重設，以啟用和停用帳戶的權限的群組。  
  
若要建立群組，以啟用和停用管理帳戶，請執行下列步驟：  
  
1.  在 OU 結構中，您將會裝載 「 管理帳戶，請以滑鼠右鍵按一下的 OU，您想要用來建立群組，按一下**新增**然後按一下**群組**。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  在 **新增物件-群組**對話方塊方塊中，輸入群組的名稱。 如果您打算使用此群組來 [啟用] 中您樹系的所有管理帳戶，讓它為萬用安全性群組。 如果您有單一網域樹系，或如果您打算在每個網域中建立群組時，您可以建立全域安全性群組。 按一下 [確定]  以建立群組。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  在您剛剛建立的群組上按一下滑鼠右鍵，按一下 [內容]  ，然後按一下 [物件]  索引標籤。在群組的**物件屬性**對話方塊中，選取**保護物件以防止被意外刪除**，這不僅可以防止其他授權使用者刪除群組，但也將它移到另一個 OU 除非屬性是第一個取消選取此選項。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > 如果您已經在群組的父系 Ou，以限制系統管理一組受限的使用者上設定權限，您可能不需要執行下列步驟。 它們會提供以下，即使尚未實作，您已在其中建立此群組的 OU 結構的有限系統管理控制，您可以保護針對修改的群組，讓未經授權的使用者。  
  
4.  按一下 **成員**索引標籤，並新增帳戶，您的小組，負責啟用管理帳戶或填入的成員受保護群組時所需。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  如果您有不這麼做，請在**Active Directory 使用者和電腦**主控台中，按一下**檢視**，然後選取**進階功能**。 以滑鼠右鍵按一下您剛建立的群組，請按一下**屬性**，然後按一下**安全性** 索引標籤。在 安全性  索引標籤上，按一下 進階  。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  在 **的 [群組] 的 [進階安全性設定**] 對話方塊中，按一下**停用繼承**。 出現提示時，按一下**繼承到此物件的明確權限的權限的轉換**，然後按一下**確定**以返回群組**安全性** 對話方塊。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  在 **安全性**索引標籤上，移除不應該允許存取此群組的群組。 例如，如果您不想要能夠讀取群組的名稱和一般屬性的已驗證的使用者，您可以移除該 ACE。 您也可以移除 Ace，例如用於帳戶操作員和 pre-Windows 2000 Server 相容的存取。 您應，不過，保留最低限度的物件權限。 將下列 Ace 保留不變：  
  
    -   SELF  
  
    -   系統  
  
    -   Domain Admins  
  
    -   Enterprise Admins  
  
    -   Administrators  
  
    -   Windows Authorization Access Group （如果適用）  
  
    -   企業網域控制站  
  
    雖然似乎自相矛盾允許的最高特殊權限的群組來管理此群組的 Active Directory 中，您目標中實作這些設定不是為了防止已獲授權的變更那些群組的成員。 相反地，目標是確保當您有需要極高的層級的權限的情況下，已獲授權的變更會成功。 它是基於這個理由，變更預設特殊權限群組巢狀結構、 權限，以及整份文件的權限令人沮喪。 藉由讓預設的結構保持不變並清空的目錄中最高的權限群組的成員資格，您可以建立更安全的環境，仍能正常運作。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > 如果您在您用來建立此群組的 OU 結構中並不設定物件的稽核原則，您應該設定稽核以記錄變更此群組。  
  
8.  您已完成將用來 「 看看 」 群組的組態管理帳戶時所需，「 簽入 「 帳戶完成其活動。  
  
#### <a name="creating-the-management-accounts"></a>建立管理帳戶

您應該建立至少一個將用來管理在 Active Directory 安裝中，特殊權限群組的成員資格的帳戶和最好是第二個帳戶以做為備用。 您是否要在單一網域樹系中建立管理帳戶，並授與他們所有的網域的管理功能選擇保護群組，或您選擇實作管理帳戶樹系中每個網域中，程序是否實際上相同。  
  
> [!NOTE]  
> 這份文件中的步驟假設，您還沒有實作角色型存取控制和 Active Directory 的特殊權限的身分識別管理。 因此，使用者的帳戶有問題的網域的 Domain Admins 群組的成員必須執行一些程序。  
>   
> 當您使用以資料的權限的帳戶時，您可以登入執行設定活動的網域控制站。 不需要 DA 權限的步驟可以執行系統管理工作站登入的較低權限帳戶。 以淡藍色的色彩顯示起的對話方塊的螢幕擷取畫面表示可以在網域控制站執行的活動。 深藍色的色彩顯示對話方塊的螢幕擷取畫面會代表可使用有限權限帳戶的系統管理工作站執行的活動。  
  
若要建立管理帳戶，請執行下列步驟：  
  
1. 登入網域控制站是網域的資料群組的成員的帳戶。  

2. 啟動**Active Directory 使用者和電腦**並瀏覽至 OU，您會建立 「 管理帳戶。  

3. 以滑鼠右鍵按一下 OU，然後按一下 **的新**然後按一下**使用者**。  

4. 在 [**新增物件-使用者**對話方塊中，輸入您想要的命名資訊的帳戶，然後按一下**下一步]** 。  

   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. 提供初始使用者帳戶密碼，清除**使用者必須變更密碼在下次登入時**，選取**使用者不得變更密碼**並**帳戶已停用**，及按一下 [**下一步]** 。  

   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. 確認帳戶詳細資料正確，然後按一下 **完成**。  

7. 以滑鼠右鍵按一下您剛才建立的使用者物件，然後按一下 **屬性**。  

8. 按一下 [**帳戶**] 索引標籤。  

9. 在 **帳戶選項**欄位中，選取**是機密帳戶，無法委派**旗標，選取**此帳戶支援 Kerberos AES 128 位元加密**和 （或)**此帳戶支援 Kerberos AES 256 加密**旗標，然後按一下**確定**。  

   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > 因為此帳戶，例如其他帳戶，會有有限，但功能強大的函式，此帳戶應該只用於安全的系統管理主機上。 適用於所有安全系統管理主機環境中，您應該考慮實作群組原則設定**網路安全性：設定 Kerberos 允許的加密類型**允許只能使用最安全的加密類型中，您可以實作安全的主機。  
   >
   > 雖然主機實作更安全的加密類型並不能減輕認證竊取攻擊，就會適當的使用和設定的安全的主機。 將更強的加密類型設定只供特殊權限帳戶的主機時，只會降低整體被攻擊的電腦。  
   >
   > 如需有關如何設定作業系統和帳戶上的加密類型的詳細資訊，請參閱[Kerberos 支援加密類型的 Windows 組態](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)。  
   >
   > 只在執行 Windows Server 2012、 Windows Server 2008 R2、 Windows 8 或 Windows 7 的電腦上支援這些設定。  
  
10. 在 **物件**索引標籤上，選取**保護物件以防止被意外刪除**。 這不僅可以防止物件被刪除 （甚至是由授權的使用者），但可防止它被移到不同的 OU，在 AD DS 階層中，除非變更屬性的權限的使用者第一次清除核取方塊。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. 按一下 [**遙控器**] 索引標籤。  

12. 清除**啟用遠端控制**旗標。 它永遠不應該是所需的支援人員來連接到此帳戶的工作階段，以實作修正。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > 在 Active Directory 中的每個物件應該有指定的 IT 擁有者和指定的商務擁有者，如中所述[洩露的規劃](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)。 如果您要追蹤的 Active Directory 中的 AD DS 物件 （而不是外部資料庫） 的擁有權，您應該在這個物件的內容中輸入適當的擁有權資訊。  
    >
    > 在此情況下，業務擁有者很可能是 IT 部門，andthere 沒有下也很 IT 擁有者的業務擁有者，禁止。 建立物件的擁有權的重點是要讓您識別連絡人，變更所需的物件，可能是年從初始建立時。  

13. 按一下 [**組織**] 索引標籤。  

14. 在您的 AD DS 物件標準中，輸入所需的任何資訊。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. 按一下 [**撥入**] 索引標籤。  

16. 在 **網路存取權限**欄位中，選取**拒絕存取**。此帳戶應該永遠不需要透過遠端連接。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > 不太這個帳戶將用於登入您的環境中的唯讀網域控制站 (Rodc)。 不過，應該情況曾經需要帳戶來登入 RODC，您應該將此帳戶新增至拒絕的 RODC 密碼複寫群組，讓不在 RODC 上快取其密碼。  
    >
    > 雖然每次使用之後應該重設帳戶的密碼，而且應該停用的帳戶，實作這項設定並沒有帳戶，造成不利的影響，而且可能有所幫助系統管理員忘記重設的帳戶所在的情況下密碼和停用它。  

17. 按一下 [隸屬於]  索引標籤。  

18. 按一下 **\[新增\]** 。  

19. 型別**Denied RODC Password Replication Group**中**選取使用者、 連絡人、 電腦**對話方塊中，然後按一下**檢查名稱**。 當群組的名稱會加上底線，物件選擇器中時，按一下**確定**並確認帳戶現在是在下列螢幕擷取畫面顯示兩個群組的成員。 並未新增帳戶到任何受保護的群組。  

20. 按一下 [確定]  。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. 按一下 **安全性**索引標籤，然後按一下**進階**。  

22. 在 [**進階安全性設定**] 對話方塊中，按一下**停用繼承**並複製做為明確的權限繼承的權限，然後按一下**新增**。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. 在 [ **[帳戶] 的權限項目**] 對話方塊中，按一下**選取一個主體**並新增您在上一個程序中建立的群組。 捲動到底部的 [] 對話方塊中，按一下**全部清除**移除所有預設權限。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. 捲動至頂端**權限項目** 對話方塊。 請確認**型別**下拉式清單設為**允許**，然後在**適用於**下拉式清單中，選取**只有這個物件**。  

25. 在 **權限**欄位中，選取**讀取全部內容**，**讀取權限**，和**重設密碼**。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. 在 **屬性**欄位中，選取**讀取 userAccountControl**並**撰寫 userAccountControl**。  

27. 按一下 [ **[確定]** ， **[確定]** 中再次**進階安全性設定**] 對話方塊。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > **UserAccountControl**屬性可控制多個帳戶設定選項。 您無法授與變更只有部分設定選項，當您授與給屬性的寫入權限的權限。  

28. 在 **群組或使用者名稱**欄位**安全性**索引標籤上，移除不應該允許存取或管理帳戶的任何群組。 請勿移除拒絕 Ace，已設定的任何群組，例如 Everyone 群組和自助計算帳戶 (設定時該 ACE**使用者不得變更密碼**旗標已啟用的帳戶建立期間。 也請勿移除您剛才加入的群組、 系統帳戶或群組，例如 EA、 DA、 BA，還是 Windows Authorization Access Group。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. 按一下 **進階**並確認 進階安全性設定 對話方塊中看起來類似下列螢幕擷取畫面。  

30. 按一下 [ **[確定]** ，並**確定**] 以關閉帳戶的 [屬性] 對話方塊。  

    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. 安裝程式的第一個管理帳戶現已完成的。 您將在稍後的程序中測試帳戶。  

##### <a name="creating-additional-management-accounts"></a>建立額外的管理帳戶

重複上述步驟，複製您剛才建立的帳戶或建立指令碼來建立您的所需的組態設定的帳戶，您可以建立額外的管理帳戶。 不過請注意，是否您複製您剛才建立的帳戶時，許多自訂的設定和 Acl 不會複製到新的帳戶，以及您必須重複執行大部分的設定步驟。  
  
您可以改為建立的群組，您將委派權限，以填入和 unpopulate 受保護的群組，但您將需要保護的群組與您將放在它的帳戶。 因為應該會有極少數您目錄中的帳戶授與管理受保護群組的成員資格的能力，則建立個別的帳戶可能是最簡單的方法。  
  
不管您選擇如何建立群組，您可以在其中放置管理帳戶，您應該確定每個帳戶會受到保護，如先前所述。 您也應該考慮實作類似於中所述的 GPO 限制[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
##### <a name="auditing-management-accounts"></a>稽核帳戶管理

您應該設定最小值，記錄所有寫入帳戶的帳戶上的稽核。 這可讓您不僅要識別成功啟用的帳戶和重設其密碼的期間已獲授權的使用，但也會識別未經授權的使用者嘗試管理帳戶。 帳戶上的失敗的寫入應該擷取安全性資訊和事件監視 (SIEM) 系統中 （如果適用），並應該會觸發警示，提供通知給人員負責調查可能遭受的危害。  
  
SIEM 解決方案會取自涉及的安全性來源 （例如，事件記錄檔、 應用程式資料、 網路資料流、 反惡意程式碼產品和入侵偵測來源） 中的事件資訊、 定序資料，並試著將智慧型的檢視和主動的動作. 有許多商業的 SIEM 解決方案，和許多企業建立私用實作。 安全性監視和事件回應功能，可大幅增強設計完善且適當地實作的 SIEM。 不過，功能與正確性不同極大的差異方案之間。 Siem 已超出本文的範圍，但包含的特定事件建議應視為任何 SIEM 實作者。  
  
如需建議的稽核網域控制站的組態設定的詳細資訊，請參閱[監視的洩露跡象的 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。 網域控制站的特定組態設定中提供[監視的洩露跡象的 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>若要修改受保護群組的成員資格啟用的管理帳戶

在此程序中，您將網域的 AdminSDHolder 物件，以允許修改網域中的受保護群組的成員資格的新建立的管理帳戶上設定權限。 無法執行此程序，透過圖形化使用者介面 (GUI)。  
  
中所述[附錄 c:受保護的帳戶和 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)，物件會有效地 「 複製 」 到網域的 AdminSDHolder 上的 ACL 會 SDProp 工作執行時，受保護物件。 受保護的群組和帳戶不是繼承其權限自 AdminSDHolder 物件其權限明確設定為符合的 AdminSDHolder 物件上。 因此，當您修改 AdminSDHolder 物件的權限，您必須針對適用於您的目標受保護的物件類型的屬性進行修改。  
  
在此情況下，您將會被授與新建立的管理帳戶，以允許他們閱讀及成員屬性的群組物件上的寫入。 不過，AdminSDHolder 物件不是群組物件和群組屬性不會顯示在圖形化的 ACL 編輯器。 它是基於這個理由，您將實作透過 Dsacls 命令列公用程式的權限變更。 若要授與 （已停用） 的管理帳戶權限可修改受保護群組的成員資格，請執行下列步驟：  
  
1. 登入網域控制站，最好是網域控制站持有 PDC 模擬器 (PDCE) 角色，已成為 DA 群組成員的網域中的使用者帳戶的認證。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. 開啟提升權限的命令提示字元，以滑鼠右鍵按一下**命令提示字元**然後按一下**系統管理員身分執行**。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. 當系統提示您核准提升權限，按一下**是**。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > 如需提高權限和使用者帳戶控制 (UAC) 在 Windows 中的詳細資訊，請參閱[UAC 處理程序和互動](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx)TechNet 網站上。  
  
4. 在命令提示字元中 （以取代您的網域特定資訊） 的型別**Dsacls [您的網域中的 AdminSDHolder 物件的辨別名稱] [管理帳戶 UPN] /G: RPWP; 成員**。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   前一個命令 （這是不區分大小寫） 的運作方式，如下所示：  
  
   - Dsacls 設定，或顯示的目錄物件的 Ace  
  
   - CN = AdminSDHolder，CN = System，DC = TailSpinToys，DC = msft 識別要修改的物件  
  
   - /G 表示正在設定 ACE 的授與  
  
   - PIM001@tailspintoys.msft 是使用者主體名稱 (UPN) 的 Ace 會授與的安全性主體  
  
   - RPWP 授與讀取屬性和寫入屬性的權限  
  
   - 成員是屬性 （屬性） 的名稱上的使用權限設定  
  
   如需使用詳細資訊**Dsacls**，不含任何參數，在命令提示字元中輸入 Dsacls。  
  
   如果您已建立網域的多個管理帳戶，您應該執行的每個帳戶的 Dsacls 命令。 完成 AdminSDHolder 物件上的 ACL 組態後，您應該強制執行，或等候完成其排程的執行的 SDProp。 如需強制執行的 SDProp，請參閱 「 手動執行 SDProp 」 中[附錄 c:受保護的帳戶和 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
   執行 SDProp 後，您可以確認您對 AdminSDHolder 物件所做的變更，已套用至網域中的受保護群組。 您無法驗證此 AdminSDHolder 物件上的 ACL 檢視先前所述的理由，但您可以確認，已藉由檢視受保護群組上的 Acl 套用的權限。  
  
5. 在  **Active Directory 使用者和電腦**，確認您已啟用**進階功能**。 若要這樣做，請按一下**檢視**，找出**Domain Admins**群組中，以滑鼠右鍵按一下群組，然後按一下**屬性**。  
  
6. 按一下 [**安全性**索引標籤，然後按一下**進階**以開啟**Domain Admins 的進階安全性設定**] 對話方塊。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. 選取 **管理帳戶的允許 ACE**然後按一下**編輯**。 確認帳戶已被授與，只有**讀取成員**並**寫入成員**DA 群組，然後按一下 的權限**確定**。  
  
8. 按一下  **確定**中**進階安全性設定** 對話方塊中，然後按一下 **確定** 以關閉 資料群組的 屬性 對話方塊。  
  
   ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. 您可以重複上述步驟，在網域中其他受保護群組權限應該是相同的受保護的所有群組。 您現在已完成建立及設定此網域中的受保護群組的管理帳戶。  
  
    > [!NOTE]  
    > 任何可寫入 Active Directory 中的群組成員資格的權限的帳戶也可以加入本身的群組。 此行為是經過設計，而且無法停用。 基於這個理由，您應該永遠保留在未使用，停用的管理帳戶，並應該在以及正在使用中時停用這些密切監視的帳戶。  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>正在驗證的群組和帳戶設定

既然您已建立並設定管理帳戶，可以進行修改 （其中包含最高特殊權限的 EA、 DA 和 BA 群組） 的網域中的受保護群組的成員資格，您應該確認已被帳戶和其管理群組正確建立。 驗證是由這些一般工作所組成：  
  
1.  測試可以啟用和停用驗證，成員的群組可以啟用和停用的帳戶重設其密碼，但無法管理帳戶上執行其他系統管理活動的管理帳戶的群組。  
  
2.  測試管理帳戶，以確認它們可以加入和移除成員，以受保護網域中的群組，但無法變更受保護的帳戶和群組的任何其他屬性。  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>測試群組，可啟用及停用管理帳戶
  
1.  若要測試啟用的管理帳戶並重設其密碼，登入的安全系統管理工作站的群組成員的帳戶中建立[附錄 i:建立管理帳戶，受保護的帳戶和 Active Directory 中群組](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  開啟**Active Directory 使用者和電腦**，以滑鼠右鍵按一下 管理帳戶，然後按一下**啟用帳戶**。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  應該顯示一個對話方塊，確認帳戶已啟用。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  接下來，您可以重設管理帳戶的密碼。 若要這樣做，請在帳戶上按一下滑鼠右鍵，然後按一下**重設密碼**。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  輸入新帳戶密碼**新密碼**並**確認密碼**欄位，然後按一下 **確定**。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  應該會出現一個對話方塊，確認帳戶的密碼已重設。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  現在，嘗試修改的管理帳戶的其他屬性。 以滑鼠右鍵按一下帳戶，然後按一下 [**屬性**，然後按一下**擃勂**] 索引標籤。  
  
8.  選取 **啟用遠端控制**然後按一下**套用**。 操作應該會失敗並**拒絕存取**應該會顯示錯誤訊息。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. 按一下 **帳戶**帳戶索引標籤，然後嘗試變更帳戶的名稱、 登入時數或登入工作站。 所有應失敗，而且帳戶不受控制的選項**userAccountControl**屬性應該會變成灰色無法使用，以便進行修改。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. 嘗試將管理群組新增至受保護的群組，例如 [日期] 群組。 當您按一下 **確定**，應出現一則訊息，通知您您沒有修改群組的權限。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. 執行其他測試，因為需要驗證，您無法設定任何項目上的管理帳戶，除了**userAccountControl**設定及重設密碼。  
  
    > [!NOTE]  
    > **UserAccountControl**屬性可控制多個帳戶設定選項。 您無法授與變更只有部分設定選項，當您授與給屬性的寫入權限的權限。  
  
##### <a name="test-the-management-accounts"></a>測試管理帳戶

既然您已啟用一或多個可以變更受保護群組的成員資格的帳戶，您可以測試以確保它們可以修改受保護的群組成員資格，但無法對受保護的帳戶和群組執行其他修改的帳戶。  
  
1.  第一個管理帳戶登入安全的系統管理主機。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  啟動**Active Directory 使用者和電腦**並找出**Domain Admins 群組**。  
  
3.  以滑鼠右鍵按一下**Domain Admins**群組，然後按一下**屬性**。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  在 [ **Domain Admins 內容**，按一下**成員**] 索引標籤和**按一下**新增。 輸入名稱的帳戶，將會提供暫時的網域系統管理員權限，然後按一下**檢查名稱**。 當帳戶的名稱會加上底線時，按一下**確定**以返回**成員** 索引標籤。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  在 **成員**索引標籤**Domain Admins 內容** 對話方塊中，按一下 **套用**。 按一下後**套用**帳戶應該保持 DA 群組的成員，您應該會收到任何錯誤訊息。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  按一下 **管理者**索引標籤中**Domain Admins 內容**對話方塊方塊，並確認您無法在所有欄位中輸入文字，而所有的按鈕會呈現灰色。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  按一下 **一般**索引標籤中**Domain Admins 內容**對話方塊方塊，並確認您是否無法修改任何該索引標籤的相關資訊。  
  
    ![建立管理帳戶](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  視需要重複這些步驟之其他受保護的群組。 當您完成時，登入安全的系統管理主機群組的成員的帳戶，您建立啟用和停用管理帳戶。 然後重設只需要測試，並停用帳戶的管理帳戶的密碼。 您已完成安裝程式的管理帳戶和群組，負責啟用和停用的帳戶。  
