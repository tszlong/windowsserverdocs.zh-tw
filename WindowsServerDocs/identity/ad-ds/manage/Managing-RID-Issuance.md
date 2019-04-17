---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: "管理 RID 發行"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f84bcc1aa32e9993903e094fc43feffcbe16a05b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="managing-rid-issuance"></a>管理 RID 發行

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題以 RID FSMO 主角，包括發行新及監視功能 RID 主機，以及如何分析及疑難排解 RID 發行解釋變更。  
  
-   [管理 RID 發行](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [疑難排解 RID 發行](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
更多的資訊可在[AskDS 部落格](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)。  
  
## <a name="BKMK_Manage"></a>管理 RID 發行  
根據預設，網域容量大約億安全性原則，例如使用者、 群組和電腦的。 當然，不是網域的許多主動使用物件。 不過，Microsoft 客戶支援發現案例位置：  
  
-   提供的軟體或不小心大量管理指令碼建立的使用者、 群組和電腦。  
  
-   許多未使用的安全性和 distribution 群組所建立的使用者委派  
  
-   許多網域控制站已降級還原，或中繼資料已清除  
  
-   復原森林屬於執行  
  
-   InvalidateRidPool 操作經常執行  
  
-   移除封鎖大小登錄值是正確增加  
  
所有的這些情形使用向上 Rid 必要經常誤。 許多年來，在 Rid 退出的幾個環境，這被迫它們移轉到新的網域或執行樹系復原。  
  
Windows Server 2012 位址有只會發生問題的 Active Directory 無所不在年齡與 RID 配置的問題。 其中包括更好的事件登入，更適當限制，以及的能力來-緊急位在連整體網域的全域 RID 空間的大小。  
  
### <a name="periodic-consumption-warnings"></a>定期消耗警告  
Windows Server 2012 中新增追蹤的全域 RID 空間事件交叉主要里程碑時，提供早期警告。 型號計算 10%標記使用中的全域集區與登達到事件。 計算剩餘的可用的下一步 10%，然後繼續事件循環。 在全球 RID 空間耗盡時，為 10%點擊降低集區中的更快速地加速的事件會 （但事件登入抑制會防止每小時多個項目）。 每個網域控制站系統事件登入寫入 Directory-服務-坡警告事件 16658。  
  
假設預設 30 位元全球 RID 空間，請先事件登配置包含 107,374,182 集區時<sup>二個</sup>RID。 事件速率加速自然檢查 100000 的最後一個點之前的總 110 事件。 行為會類似解除鎖定 31 元全球 RID 空間： 開頭 214,748,365 和 117 活動中完成。  
  
> [!IMPORTANT]  
> 未如預期般事件這個;調查使用者、 電腦及群組立即網域中的建立程序。 建立多個 100 萬 AD DS 物件是非常不正常。  
  
![RID 的發行](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>移除集區無效事件  
有新事件警示已捨棄 [本機俠移除集區。 這些的資訊，可能會如預期般，尤其是因為 VDC 的新功能。 事件上看到下列事件清單的詳細資訊。  
  
### <a name="BKMK_RIDBlockMaxSize"></a>清除 [封鎖大小限制  
一般而言，網域控制站要求的 500 Rid 區塊 RID 配置一次。 您可以覆寫預設的網域控制站使用下列登錄呼叫完成值：  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Windows Server 2012 時前, 時發生不登錄金鑰，除了隱含 DWORD 最大值 （此為 0xffffffff 或 4294967295） 中執行的最大值。 這個值是相當大於總全球 RID 空間。 系統管理員有時不當不小心設定或清除 [封鎖大小具有耗盡那速率全球 RID 的值。  
  
在 Windows Server 2012，您無法設定此登錄值高於 15000 小數點 (十六進位 0x3A98)。 如此可防止那意外的 RID 配置。  
  
如果您將值設定*高*15000，比值會被視為 15000 和網域控制站登入事件 16653 Directory 服務事件登入，直到修正值每個重新開機。  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>解除鎖定全球 RID 空間的大小  
Windows Server 2012 時前的全球 RID 空間限於 2<sup>30</sup> （或 1073741823） 總 Rid。 之後，只有網域移轉或樹系復原到較舊的時間範圍允許任何測量 Sid 建立-復原嚴重損壞。 開始在 Windows Server 2012，2]<sup>31</sup>為了提高 2147483648 Rid 全球集區元可以解除鎖定。  
  
AD DS 將此設定儲存在特殊隱藏屬性名為**SidCompatibilityVersion**上的所有網域控制站進行 RootDSE 操作。 此屬性不可讀取使用 ADSIEdit、 LDP 或其他工具。 增加中的全域 RID 空間，請檢查 Directory-服務-坡從警告事件 16655 系統事件登入或使用下列命令 Dcdiag:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
如果增加全球 RID 集區，而不是預設 1,073,741,823 2147483647 會變更提供集區。 例如：  
  
![RID 的發行](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> 是此解除鎖定*只*以避免 RID 用盡是使用*只*移除頂執法搭配 （查看下一節）。 不 」 主動 」 設定此環境中數以百萬計的剩餘 Rid 和低成長，當應用程式的相容性問題可能會有產生解除鎖定 RID 集區的 Sid。  
>   
> 這解除鎖定作業無法還原或移除，除非完整的樹系復原到先前的備份。  
  
#### <a name="important-caveats"></a>重要事項需要注意  
Windows Server 2003 及 Windows Server 2008 網域控制站無法發出 Rid 全球 RID 集區 31<sup>時間</sup>元已解除鎖定。 Windows Server 2008 R2 網域控制站*可以*使用 31<sup>時間</sup>位元 Rid *，但是只要*他們使用的是 hotfix [KB 2642658](https://support.microsoft.com/kb/2642658)安裝。 不支援和未網域控制站在用完時，將全球 RID 集區。  
  
這項功能會不執行任何網域正常運作的層級。小心謹慎只有 Windows Server 2012 或更新的 Windows Server 2008 R2 網域控制站存在網域中。  
  
#### <a name="implementing-unlocked-global-rid-space"></a>實作解除鎖定的全域清除空間  
若要解除鎖定 31 RID 集區<sup>時間</sup>之後接收 RID 頂警示 （如下所示） 元執行下列步驟：  
  
1.  最好的角色執行 Windows Server 2012 網域控制站移除主機。 如果不行，Windows Server 2012 」 的網域控制站傳輸。  
  
2.  執行 LDP.exe  
  
3.  按一下**連接**功能表和**連接**的 Windows Server 2012 移除主機上 389 連接埠，然後按一下 [**繫結**網域系統管理員。  
  
4.  按一下**瀏覽]**功能表和**修改]**。  
  
5.  確認**DN**是空白的。  
  
6.  在**編輯項目屬性**，輸入：  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  在**值**，輸入：  
  
    ```  
    1  
    ```  
  
8.  確認**新增**中選取**作業**，按一下 [**輸入**。 此更新**的項目清單**。  
  
9. 選取 [**同步**和**延伸**然後按一下 [選項]**執行**。  
  
    ![RID 的發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. 如果成功，LDP 輸出視窗中所示：  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![RID 的發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. 請確認增加了系統事件登入資訊 Directory-服務-坡事件 16655 網域控制站的全域 RID 集區。  
  
### <a name="rid-ceiling-enforcement"></a>移除頂執法  
保護的負擔得起和提高權限管理的感知、 Windows Server 2012 導入人造的最高的全域 RID 範圍 10%剩餘 Rid 中的全域空間。 在一 （1） %的人造上限中, 要求 RID 集區的網域控制站寫入 Directory-服務-坡警告事件 16656 他們系統事件登入。 當達到上限移除主機 FSMO 10%，它 Directory-服務-坡事件 16657 寫入其系統事件登入，並將配置進一步 RID 集區之前覆寫上限。 這會強迫您評估 RID 主機網域中的狀態和地址潛在失控 RID 配置。這也會從耗盡整個 RID 空間保護網域。  
  
此上限是固定，10%剩餘的可用空間 RID。 是的頂啟動當 RID 主機配置包含相對應的全域 RID 空間 90 （90） %RID 集區。  
  
-   網域預設，第一個點觸發程序為 2<sup>30</sup>-1 * 0.90 = 966,367,640 （或 107,374,183 Rid 剩餘）。  
  
-   觸發程序點是 2 解除鎖定的位元 31 RID 空間的網域中，<sup>31</sup>-1 * 0.90 = 1,932,735,282 Rid （或 214,748,365 Rid 剩餘）。  
  
RID 主機觸發時，將 Active Directory 屬性設定**msDS-RIDPoolAllocationEnabled** (通用名稱**ms-DS-RID-Pool-Allocation-Enabled**) 為物件 FALSE:  
  
DATA-CN = RID 管理員 $DATA-CN = 系統特區 =*<domain>*  
  
這將 16657 事件並將無法使用所有網域控制站 RID 封鎖發行。 網域控制站繼續消耗發出它們未執行任何 RID 集區。  
  
移除區塊允許 RID 集區配置，才能繼續，該將值設定為 TRUE。 在 RID 主機所執行的下一步 RID 配置，屬性將無法設定的值為預設值。 之後，有不進一步天花板，最後的全域 RID 空間耗盡，需要修復或網域移轉的樹系。  
  
#### <a name="removing-the-ceiling-block"></a>移除頂封鎖  
若要移除一次到達人造頂封鎖，請執行下列步驟：  
  
1.  最好的角色執行 Windows Server 2012 網域控制站移除主機。 如果不行，Windows Server 2012 」 的網域控制站傳輸。  
  
2.  執行 LDP.exe。  
  
3.  按一下**連接**功能表和*連接*的 Windows Server 2012 移除主機上 389 連接埠，然後按一下 [**繫結**網域系統管理員。  
  
4.  按一下**檢視**功能表和**樹**，然後為**基本 DN**選取 [移除主機的自己網域命名操作。 按一下**[確定]**。  
  
5.  瀏覽窗格中深入**DATA-CN = 系統**容器和**DATA-CN = RID 管理員 $**物件。 以滑鼠右鍵按一下，按**修改]**。  
  
6.  在 [編輯項目屬性，請輸入：  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  在**值**，輸入 （中大寫）：  
  
    ```  
    TRUE  
    ```  
  
8.  選取**取代**在**作業**並按**輸入**。 此更新**的項目清單**。  
  
9. 讓**同步**和**延伸**然後按一下 [選項]**執行**:  
  
    ![RID 的發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. 如果成功，LDP 輸出視窗中所示：  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![RID 的發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>其他 RID 修正  
先前的 Windows Server 作業系統必須遺漏時 RID 集區遺失 rIDSetReferences 屬性。 這個問題上執行 Windows Server 2008 R2 網域控制站，hotfix 從[KB 2618669](https://support.microsoft.com/kb/2618669)。  
  
### <a name="unfixed-rid-issues"></a>RID 未修正的問題  
在過去有 RID 遺漏 account 建立失敗。建立帳號，失敗仍然會使用 RID 上。 常見的範例是建立的使用者使用密碼不符合複雜。  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>修正移除較舊版本的 Windows Server 的  
所有的修正與上述變更已推出的 Windows Server 2008 R2 hotfix。 目前有不 Windows Server 2008 hotfix 計劃，或在進行中。  
  
## <a name="BKMK_Tshoot"></a>疑難排解 RID 發行  
  
### <a name="introduction-to-troubleshooting"></a>疑難排解簡介  
疑難排解 RID 的發行需要邏輯和線性的方法。 監視您的事件登仔細的 RID 觸發警告和錯誤，除非您第一次訊息的問題，包括很可能會失敗的 account 作品。 疑難排解 RID 發行的關鍵在於以了解當症狀會如預期般或許多 RID 發行問題可能會影響只有一個網域控制站和無關元件的改進。 下列此簡單圖表協助做出那些更清楚：  
  
![RID 的發行](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>疑難排解選項  
  
#### <a name="logging-options"></a>登入選項  
所有登入 RID 發行，就會發生系統事件登入，在來源 Directory-服務-坡中。 登入是支援和最大的詳細資訊，預設設定。 如果任何項目針對 Windows Server 2012 中的新元件變更登不入，問題視為 (亦舊版、 前 Windows Server 2012) 的傳統 Windows 2008 R2 或較舊的作業系統中看到 RID 發行的問題。  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>公用程式 」 和 「 疑難排解的命令  
如果要不解釋上述登-問題的疑難排解尤其是較舊的 RID 發行問題-使用的起點工具下列清單：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   網路監視器 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>一般的網域控制站設定進行疑難排解的方法  
  
1.  錯誤造成簡單的權限] 或 [網域控制站可用性問題嗎？  
  
    1.  您想要建立安全性主體不必要的權限嗎？ 檢查拒絕錯誤存取的輸出。  
  
    2.  已網域控制站嗎？ 檢查傳回的錯誤或 LDAP 網域控制站可用性訊息。  
  
2.  尤其是傳回錯誤是否連接 Rid，達到特定作為指導方針和？ 若是如此，請依照下列指導方針。  
  
3.  尤其是傳回錯誤是否連接 Rid，但其他非特定？ 例如，「 Windows 無法建立物件因為 Directory 服務無法配置相關的 id。 」  
  
    1.  請檢查 「 傳統 」 (前 Windows Server 2012) 的網域控制站系統事件登入 RID 的事件詳細在[移除集區要求](https://technet.microsoft.com/en-us/library/ee406152(WS.10).aspx)（16642 16643、 16644、 16645、 16656）。  
  
    2.  檢查系統事件網域控制站和新封鎖指出事件下述 16655、 16656 （16657） 本主題中移除主機。  
  
    3.  Active Directory 複寫健康驗證 Repadmin.exe 和移除主機的可用性使用**Dcdiag.exe /test:ridmanager /v**。 如果這些測試結果不明可讓雙面網路擷取之間的網域控制站與移除主機。  
  
### <a name="troubleshooting-specific-problems"></a>特定的問題進行疑難排解  
下列新訊息登入 Windows Server 2012 網域控制站系統事件登入。 自動的 AD 健康追蹤系統，例如 System Center Operations Manager，應該監視這些事件。所有值得注意，且部分是指示器的網域重大問題。  
  
|||  
|-|-|  
|事件編號|16653|  
|來源|薩姆-directory-服務|  
|嚴重性|警告|  
|訊息|集區大小 account 識別碼 (Rid) 是由系統管理員的身分設定大於支援的最大值。 網域控制站 RID 主機時，將會使用 %1 的最大值。<br /><br />如需詳細資訊，請查看[排除封鎖大小限制](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize)。|  
|筆記與解析度|關閉封鎖大小的最大值現在已 15000 小數點 (十六進位 3A98)。 網域控制站無法要求超過 15000 Rid。 每次開機值設定為下列這個最大值或時，才此事件登。|  
  
|||  
|-|-|  
|事件編號|16654|  
|來源|薩姆-directory-服務|  
|嚴重性|資訊|  
|訊息|已無效 account 識別碼 (Rid) 的集區。 這可能是因為預期如下：<br /><br />1.網域控制站是從備份還原。<br /><br />2.網域控制站執行一樣會還原的快照。<br /><br />3.系統管理員的身分，以手動方式有失效集區。<br /><br />查看 https://go.microsoft.com/fwlink/?LinkId=226247 如需詳細資訊。|  
|筆記與解析度|如果這個活動是預期，請連絡所有網域系統管理員，並判斷其中執行動作。 Directory 服務事件登入也包含進一步資訊上執行下列步驟進行時。|  
  
|||  
|-|-|  
|事件編號|16655|  
|來源|薩姆-directory-服務|  
|嚴重性|資訊|  
|訊息|已 %1 增加 account 識別碼 (Rid) 的全球最大值。|  
|筆記與解析度|如果這個活動是預期，請連絡所有網域系統管理員，並判斷其中執行動作。 這個事件筆記整體 RID 增加集區大小預設為 2 超過<sup>30</sup>，將不會自動; 以及只要管理動作。|  
  
|||  
|-|-|  
|事件編號|16656|  
|來源|薩姆-directory-服務|  
|嚴重性|警告|  
|訊息|已 %1 增加 account 識別碼 (Rid) 的全球最大值。|  
|筆記與解析度|儘速 ！ 這個網域控制站配置識別碼 account (RID) 集區。 集區表示這個網域耗用總提供 account 識別碼相當。<br /><br />將啟動保護機制時網域到達的總提供 account-識別碼剩餘下列臨界值,： 1%。  保護機制會阻止 account 建立，直到您以手動方式重新讓 RID 主要網域控制站 account 識別碼配置。<br /><br />查看 https://go.microsoft.com/fwlink/?LinkId=228610 如需詳細資訊。|  
  
|||  
|-|-|  
|事件編號|16657|  
|來源|薩姆-directory-服務|  
|嚴重性|錯誤|  
|訊息|儘速 ！ 這個網域耗用相當總提供 account-識別碼 (Rid)。 因為總提供 account-識別字的剩餘已經啟動保護機制小於： X %[人造頂引數]。<br /><br />保護機制，可避免 account 建立，直到您以手動方式重新讓 RID 主要網域控制站 account 識別碼配置。<br /><br />請務必非常執行特定診斷是以之前重新讓 account 建立確保這個網域並未消耗高不正常速度 account 識別項。 應該解析之前重新讓 account 建立任何辨識的問題。<br /><br />Account 識別碼耗盡之後 account 建立會永久停用這個網域中的網域中可能會導致診斷及修正 account 識別碼消耗高不正常率任何基礎問題失敗。<br /><br />查看 https://go.microsoft.com/fwlink/?LinkId=228610 如需詳細資訊。|  
|筆記與解析度|請連絡所有網域系統管理員，通知，建立在這個網域中的任何進一步的安全性原則直到此保護會覆寫。 如需詳細資訊，了解如何保護會覆寫和可能增加整體 RID 集區，查看[全球清除空間大小解除鎖定](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock)。|  
  
|||  
|-|-|  
|事件編號|16658|  
|來源|薩姆-directory-服務|  
|嚴重性|警告|  
|訊息|這個活動是剩餘的可用 account 識別碼 (Rid) 總數量定期更新。 剩餘的 account 識別碼數目大約是： 1%。<br /><br />Account-識別碼建立帳號，耗盡，可能網域中建立任何新帳號時使用。<br /><br />查看 https://go.microsoft.com/fwlink/?LinkId=228745 如需詳細資訊。|  
|筆記與解析度|請連絡所有網域系統管理員，通知，RID 消耗有交叉重要里程碑。如果這是您預期的行為，或不是藉由檢視安全性信任建立模式判斷。 得更容易看到這個活動是常見，因為這表示該至少 ~ 100 萬 RID 有尚未配置。|  
  
## <a name="see-also"></a>也了  
[Windows Server 2012 中管理 RID 發行](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


