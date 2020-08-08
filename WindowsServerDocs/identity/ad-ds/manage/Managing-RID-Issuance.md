---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: 管理 RID 發行
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4d3e242bf151650144f8350a4665196672425530
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943602"
---
# <a name="managing-rid-issuance"></a>管理 RID 發行

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明 RID 主機 FSMO 角色的變更，包括 RID 主機中新的發行和監督功能，以及如何分析和疑難排解 RID 發行。

- [管理 RID 發行](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)

- [疑難排解 RID 發行](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)

如需詳細資訊，請[請參閱 askds 的 Blog](/archive/blogs/askds/managing-rid-issuance-in-windows-server-2012)。

## <a name="managing-rid-issuance"></a><a name="BKMK_Manage"></a>管理 RID 發行
根據預設，網域有約 10 億個安全性主體 (例如使用者、群組及電腦) 的容量。 當然，沒有一個網域有那麼多正在使用的物件。 不過，Microsoft 客戶支援服務卻發現以下情況：

- 佈建軟體或系統管理指令碼會意外大量產生建立的使用者、群組及電腦。

- 委派使用者建立了許多未使用的安全性和通訊群組

- 許多網域控制站經過降級、還原或清除了中繼資料

- 樹系執行了復原

- InvalidateRidPool 作業經常執行

- RID 區塊大小登錄值不正常地增加

上述這些情況都造成 RID 不必要的浪費 長期以來，有一些環境因用完了 RID 而被迫移轉至新網域或執行樹系復原。

Windows Server 2012 可解決隨 Active Directory 的成熟發展和普遍性而益發嚴重的 RID 配置問題。 其中包括更好的事件記錄、更適當的限制，以及能夠在緊急情況下，讓網域的全域 RID 空間的整體大小加倍。

### <a name="periodic-consumption-warnings"></a>定期消耗警告
Windows Server 2012 新增了全域 RID 空間事件追蹤，可在越過主要里程碑時提供早期的警告。 此模型會計算全域集區中使用了 10% 的標記，並在達到時記錄一個事件。 然後它會計算剩餘部分下一個使用了 10% 的標記，事件週期也會繼續。 隨著全域 RID 空間耗盡，減少的集區中的事件將因為更快達到 10% 而加速 (但事件記錄檔抑制功能會防止每小時超過 1 個項目)。 記錄在網域控制站中的系統事件會寫入 Directory-Services-SAM 警告事件 16658 中。

假設預設的全域 RID 空間是 30 位元，則會在配置包含第 107,374,182<sup></sup> 個 RID 的集區時記錄第一個事件。 事件速率會自然加速，直到最後一個檢查點 100,000 為止，共產生 110 個事件。 若有一個未鎖定的 31 位元的全域 RID 空間，此行為是類似的：於 214,748,365 開始，於第 117 個事件時完成。

> [!IMPORTANT]
> 此事件並非預期事件；請立即調查網域中的使用者、電腦和群組建立程序。 建立上億個 AD DS 物件並不正常。

![RID 發行](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)

### <a name="rid-pool-invalidation-events"></a>RID 集區失效事件
有新的事件會警示本機 DC RID 集區已被捨棄。 這些是參考事件，並且可以預期，尤其是因為新的 VDC 功能時。 如需事件的詳細資訊，請參閱下列事件清單。

### <a name="rid-block-size-limit"></a><a name="BKMK_RIDBlockMaxSize"></a>RID 區塊大小限制
一般來說，網域控制站會要求一次在區塊中配置 500 個 RID。 您可以使用下列登錄 REG_DWORD 值來覆寫網域控制站上的這個預設值：

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values
RID Block Size

```

在 Windows Server 2012 之前，該登錄值並無最大值的限制，只有隱含的 DWORD 最大值 (值為 0xffffffff 或 4294967295)。 此值遠超過全域 RID 總空間。 系統管理員有時不慎或意外設定的 RID 區塊大小值會大量地耗盡全域 RID 。

在 Windows Server 2012 中，這個登錄值無法設定為超過 15,000 十進位 (0x3A98 十六進位)。 這樣可防止大量不必要的 RID 配置。

如果該值設為*超過* 15,000，會將該值視為 15,000，而網域控制站會在每次重新開機時在目錄服務中記錄事件 16653，一直到該值更正為止。

### <a name="global-rid-space-size-unlock"></a><a name="BKMK_GlobalRidSpaceUnlock"></a>解除全域 RID 空間大小限制
在 Windows Server 2012 之前，全域 RID 空間大小限制為 2<sup>30</sup> (或共 1,073,741,823 個 RID)。 一旦達到此限制，只能移轉網域或將樹系復原到早期允許新建 SID 的時間範圍內，亦即用盡一切方法執行災難復原。 自 Windows Server 2012 起，已可解除 2<sup>31</sup> 位元大小的限制，以將全域集區增加到 2,147,483,648 個 RID。

AD DS 將此設定儲存在所有網域控制站的 RootDSE 內容內一個名為 **SidCompatibilityVersion** 的特殊隱藏屬性中。 這個屬性無法透過 ADSIEdit、LDP 或其他工具來讀取。 如果要查看全域 RID 空間是否增加，請自 Directory-Services-SAM 檢查系統事件記錄檔中是否有警告事件 16655，或使用下列 Dcdiag 命令：

```
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"

```

如果您增加全域 RID 集區，可用的集區將變為 2,147,483,647，而非預設的 1,073,741,823。 例如：

![RID 發行](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)

> [!WARNING]
> 解除這項限制*僅*是為了防止 RID 用完，而且*僅*能與「RID 上限強制」方法 (請參閱下一節) 一起使用。 請勿在還有數百萬個剩餘的 RID 且消耗速度慢的環境中先行設定此最大值，因為從解除大小限制的 RID 集區中產生的 SID 可能存在著應用程式相容性問題。
>
> 除非是將樹系完整復原到較早的備份，否則這項解除大小限制作業無法還原或取消。

#### <a name="important-caveats"></a>重要的注意事項
Windows Server 2003 與 Windows Server 2008 網域控制站無法在解除全域 RID 集區第 <sup>31</sup> 個位元的大小限制後發行 RID。 Windows Server 2008 R2 網域控制站*可以*使用 31<sup>St</sup>位的 rid，*但只有*在已安裝修補程式[KB 2642658](https://support.microsoft.com/kb/2642658)的情況。 不支援和未更新的網域控制站在解除大小限制時會將全域 RID 集區視為耗盡。

此功能不是由任何網域功能等級強制實行；請特別注意只有 Windows Server 2012 或更新的 Windows Server 2008 R2 網域控制站存在網域中。

#### <a name="implementing-unlocked-global-rid-space"></a>實作無大小限制的 RID 空間
在收到 RID 上限警示 (請參閱下方) 後，如果要將 RID 集區的大小限制解除為第 <sup>31</sup> 個位元，請執行下列步驟：

1. 確定 RID 主機角色正在 Windows Server 2012 網域控制站上執行。 如果不是，請將其傳輸到 Windows Server 2012 網域控制站。

2. 執行 LDP.exe

3. 以網域系統管理員的身分按一下 [連線]**** 功能表，針對連接埠 389 上的 Windows Server 2012 RID 主機按一下 [連線]****，然後按一下 [繫結]****。

4. 按一下 [瀏覽]**** 功能表，再按一下 [修改]****。

5. 確認 [DN]**** 為空白。

6. 在 [**編輯專案] 屬性**中，輸入：

    ```
    SidCompatibilityVersion
    ```

1. 在 [值]**** 中輸入：

    ```
    1
    ```

2. 確認已在 [操作]**** 中選取 [新增]****，然後按一下 [Enter]****。 這樣會更新 [項目清單]****。

3. 選取 [同步]**** 和 [延伸]**** 選項，然後按一下 [執行]****。

    ![RID 發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)

4. 如果成功，LDP 輸出視窗會顯示：

    ```
    ***Call Modify...
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);
    modified "".

    ```

    ![RID 發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)

5. 檢查該網域控制站上系統事件記錄檔的 Directory-Services-SAM 資訊事件 16655，確認全域 RID 集區已增加。

### <a name="rid-ceiling-enforcement"></a>RID 上限強制
為提供保護措施和提高系統管理的意識，Windows Server 2012 引進人為設定全域 RID 範圍上限的功能，可設定為全域空間中剩餘的 RID 的 10%。 當落在 1% 的人為設定的上限內時，要求 RID 集區的網域控制站會將 Directory-Services-SAM 警告事件 16656 寫入系統事件記錄檔。 當達到 RID 主機 FSMO 的百分之十的上限時，它會將 Directory-Services-SAM 事件 16657 寫入其系統事件記錄檔中，而且在覆寫最大值之前不會配置任何另外的 RID 集區。 這可強迫您評估網域中的 RID 主機狀態，解決可能發生的失控 RID 配置；這也會保護網域免於耗盡整體的 RID 空間。

此上限是硬式編碼在剩餘的可用 RID 空間的百分之十。 也就是說，當 RID 主機配置的集區中包含的 RID 是相當於全域 RID 空間的百分之九十 (90) 時，即會啟用上限設定。

- 如果是預設網域，第一個觸發點是 2<sup>30</sup>-1 * 0.90 = 966,367,640 (或剩餘的 107,374,183 個 RID)。

- 對於解除了 31 位元的 RID 空間限制的網域，觸發點是 2<sup>31</sup>-1 * 0.90 = 1,932,735,282 個 RID (或剩餘的 214,748,365 個 RID)。

一旦觸發時，RID 主機會將物件上的 Active Directory 屬性 [msDS-RIDPoolAllocationEnabled]**** (一般名稱 [ms-DS-RID-Pool-Allocation-Enabled]****) 設為 FALSE：

CN = RID 管理員 $，CN = System，DC =*<domain>*

這會寫入 16657 事件，並防止另外的 RID 區塊發行到所有網域控制站。 網域控制站會繼續使用已發行給它們的任何待處理的 RID 集區。

如果要移除區塊並允許繼續配置 RID 集區，請將該值設為 TRUE。 下一次 RID 主機執行 RID 配置時，屬性將回復為其預設的 NOT SET 值。 之後就沒有任何另外的上限，最終全域 RID 空間用完後，便需要復原樹系或移轉網域。

#### <a name="removing-the-ceiling-block"></a>移除上限區塊
如果要在達到人為設定的上限時移除區塊，請執行下列步驟：

1. 確定 RID 主機角色正在 Windows Server 2012 網域控制站上執行。 如果不是，請將其傳輸到 Windows Server 2012 網域控制站。

2. 執行 LDP.exe。

3. 以網域系統管理員的身分按一下 [連線]**** 功能表，針對連接埠 389 上的 Windows Server 2012 RID 主機按一下 [連線]**，然後按一下 [繫結]****。

4. 按一下 [檢視]**** 功能表，按一下 [樹狀目錄]****，然後為 [基本 DN]**** 選取 RID 主機自己的網域命名內容。 按一下 [確定] 。

5. 在瀏覽窗格中，向下切入至 [CN = 系統]**** 容器，然後按一下 [CN = RID 管理員 $]**** 物件。 以滑鼠右鍵按一下該物件，按一下 [修改]****。

6. 在 [編輯項目屬性] 中輸入：

    ```
    MsDS-RidPoolAllocationEnabled
    ```

7. 在 [值]**** 中輸入 (大寫)：

    ```
    TRUE
    ```

8. 在 [作業]**** 中選取 [取代]****，然後按一下 [Enter]****。 這樣會更新 [項目清單]****。

9. 啟用 [同步]**** 和 [延伸]**** 選項，然後按一下 [執行]****：

    ![RID 發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)

10. 如果成功，LDP 輸出視窗會顯示：

    ```
    ***Call Modify...
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);
    Modified "CN=RID Manager$,CN=System,DC=<domain>".

    ```

    ![RID 發行](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)

### <a name="other-rid-fixes"></a>其他 RID 修正
舊版的 Windows Server 作業系統在遺失 rIDSetReferences 屬性時會發生 RID 集區流失。 若要在執行 Windows Server 2008 R2 的網域控制站上解決此問題，請從[KB 2618669](https://support.microsoft.com/kb/2618669)安裝此修補程式。

### <a name="unfixed-rid-issues"></a>未修正的 RID 問題
過去在帳戶建立失敗時，往往會導致 RID 流失；在建立帳戶時，即使失敗仍會用掉一個 RID。 常見的範例為使用不符合複雜性的密碼建立使用者。

### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>舊版的 Windows Server 的 RID 修正
上述所有修正和變更包含在已發行的 Windows Server 2008 R2 Hotfix 中。 目前沒有計劃中或進行中的 Windows Server 2008 Hotfix。

## <a name="troubleshooting-rid-issuance"></a><a name="BKMK_Tshoot"></a>疑難排解 RID 發行

### <a name="introduction-to-troubleshooting"></a>疑難排解簡介
RID 發行的疑難排解需要邏輯與線性方法。 除非您正在仔細監視事件記錄檔中 RID 觸發的警告與錯誤，否則您發生問題的第一個指標可能會是帳戶建立失敗。 疑難排解 RID 發行的重點是了解當時的徵兆是否為預期的徵兆；許多 RID 發行問題可能只會影響一個網域控制站，而與元件改良無關。 下面這張簡單的圖表有助於更清楚說明這些決策：

![RID 發行](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)

### <a name="troubleshooting-options"></a>疑難排解選項

#### <a name="logging-options"></a>記錄選項
RID 發行的所有記錄都在系統事件記錄檔的來源 Directory-Services-SAM 底下。 記錄預設已啟用和設定為最詳細等級。 如果未記錄到 Windows Server 2012 中新元件變更的任何項目，請將問題視為發生在 Windows 2008 R2 或舊版作業系統中的典型 (又稱為傳統的 Windows Server 2012 之前的) RID 發行問題。

#### <a name="utilities-and-commands-for-troubleshooting"></a>疑難排解的公用程式與命令
若要疑難排解上述記錄檔未說明的問題，尤其是舊版的 RID 發行問題，請使用下列工具清單做為起點：

- Dcdiag.exe

- Repadmin.exe

- 網路監視器 3.4

### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>疑難排解網域控制站設定的一般方法

1. 錯誤是因為簡單的權限或是網域控制站可用性問題所造成？

    1. 您是否嘗試在未使用必要權限的情況下建立安全性主體？ 檢查輸出中是否有拒絕存取錯誤。

    2. 網域控制站是否可用？ 檢查傳回的錯誤或 LDAP 或網域控制站可用性訊息。

2. 傳回的錯誤是否明確提及 RID，是否明確到能做為指導方針？ 如果是，請依循指導方針。

3. 傳回的錯誤是否明確提及 RID，但其他方面卻不明確？ 例如，「 Windows 無法建立物件，因為目錄服務無法配置相關的識別元。 」

    1. 檢查網域控制站上的系統事件記錄檔中是否有「舊版」 2012 () rid[集區要求](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee406152(v=ws.10))中詳述的 rid 事件 (16642、16643、16644、16645、16656) 。

    2. 檢查網域控制站上的系統事件與 RID 主機是否有詳細記載於本主題下方的新區塊指標事件 (16655、16656、16657)。

    3. 使用 Repadmin.exe 驗證 Active Directory 複寫健康情況，使用 [Dcdiag.exe /test:ridmanager /v]**** 驗證 RID 主機可用性。 如果這些測試結果不明，請在網域控制站與 RID 主機之間啟用雙面網路擷取。

### <a name="troubleshooting-specific-problems"></a>疑難排解特定問題

下列新訊息會記錄在 Windows Server 2012 網域控制站上的系統事件記錄檔中。 自動化的 AD 健康情況追蹤系統 (例如 System Center Operations Manager) 應監視這些事件；所有事件都是值得注意的，某些事件則是重要網域問題的指標。

| 事件識別碼 | 16653 |
|--|--|
| 來源 | Directory-Services-SAM |
| 嚴重性 | 警告 |
| 訊息 | 系統管理員設定的帳戶識別元 (RID) 的集區大小超過支援的最大值。 當網域控制站是 RID 主機時，將使用最大值 %1。<p>如需詳細資訊，請參閱 [RID 區塊大小限制](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize)。 |
| 注意事項與解決方法 | RID 區塊大小的最大值現在是 15000 十進位 (3A98 十六進位)。 網域控制站無法要求 15000 個以上的 RID。 此事件會在每次開機時記錄，直到此值設為等於或低於此最大值為止。 |

| 事件識別碼 | 16654 |
|--|--|
| 來源 | Directory-Services-SAM |
| 嚴重性 | 資訊 |
| 訊息 | 帳戶識別碼 (RID) 集區成為無效。 在下列預期情況中可能會發生此問題：<p>1. 從備份還原網域控制站。<p>2. 在虛擬機器上執行的網域控制站會從快照集還原。<p>3. 系統管理員已手動將集區失效。<p>如需相關資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=226247 。 |
| 注意事項與解決方法 | 如果此事件是非預期的，請連絡所有網域系統管理員，確定由誰執行了動作。 目錄服務事件記錄檔也包含這些步驟的其中之一是在何時執行時的詳細資訊。 |

| 事件識別碼 | 16655 |
|--|--|
| 來源 | Directory-Services-SAM |
| 嚴重性 | 資訊 |
| 訊息 | 帳戶識別元 (RID) 的全域最大值已增加至 %1。 |
| 注意事項與解決方法 | 如果此事件是非預期的，請連絡所有網域系統管理員，確定由誰執行了動作。 此事件會記錄整體的 RID 集區大小增加到超過預設值 2<sup>30</sup>，但不會自動記錄；需由系統管理員指定。 |

| 事件識別碼 | 16656 |
|--|--|
| 來源 | Directory-Services-SAM |
| 嚴重性 | 警告 |
| 訊息 | 帳戶識別元 (RID) 的全域最大值已增加至 %1。 |
| 注意事項與解決方法 | 需要採取動作！ 帳戶識別元 (RID) 集區已配置給此網域控制站。 集區值指示這個網域耗用了可用的帳戶識別元總數中相當大的一部分。<p>當網域達到下列可用帳戶識別碼總計的閾值時，將會啟用保護機制： %1。 直到您在 RID 主機網域控制站上手動重新啟用帳戶識別元配置之前，保護機制可防止建立帳戶。<p>如需相關資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=228610 。 |

| 事件識別碼 | 16657 |
|--|--|
| 來源 | Directory-Services-SAM |
| 嚴重性 | 錯誤 |
| 訊息 | 需要採取動作！ 這個網域耗用了可用的帳戶識別元 (RID) 總數中相當大的一部分。 已啟用保護機制，因為可用的帳戶識別碼總計小於： X% [人工上限引數]。<p>直到您在 RID 主機網域控制站上手動重新啟用帳戶識別元配置之前，保護機制可防止建立帳戶。<p>重要的是，在重新啟用帳戶建立功能之前必須執行某些診斷，以確保此網域不會以異常高的速率耗用帳戶識別元。 在重新啟用帳戶建立功能之前，應先解決發現的任何問題。<p>未能診斷和修正導致帳戶識別元以異常高的速率消耗的相關問題，可能會導致網域中的帳戶識別元耗盡，因而使這個網域中的帳戶建立功能永久停用。<p>如需相關資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=228610 。 |
| 注意事項與解決方法 | 連絡所有網域系統管理員，通知他們在覆寫此保護機制前不能在此網域中建立任何安全性主體。 如需如何覆寫保護機制與增加整體 RID 集區 (可能的話) 的詳細資訊，請參閱[解除全域 RID 空間大小限制](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock)。 |

| 事件識別碼 | 16658 |
|--|--|
| 來源 | Directory-Services-SAM |
| 嚴重性 | 警告 |
| 訊息 | 此事件會定期更新剩餘的可用帳戶識別元 (RID) 總數。 剩餘的帳戶識別碼數目約為： %1。<p>帳戶建立時會使用帳戶識別元，當帳戶識別元用完後，即無法在網域中建立新帳戶。<p>如需相關資訊，請參閱 https://go.microsoft.com/fwlink/?LinkId=228745 。 |
| 注意事項與解決方法 | 連絡所有網域系統管理員，通知他們 RID 耗用量已越過主要里程碑；複查安全信任者建立模式以判斷這是否為預期行為。 看到此事件是極不尋常的，因為這表示至少已配置了上億個 RID。 |

## <a name="see-also"></a>另請參閱

- [管理 Windows Server 2012 中的 RID 發行](/archive/blogs/askds/managing-rid-issuance-in-windows-server-2012)
