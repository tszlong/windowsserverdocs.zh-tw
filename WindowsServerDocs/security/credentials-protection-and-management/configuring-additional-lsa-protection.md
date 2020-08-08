---
title: 設定額外的 LSA 保護
description: Windows Server 安全性
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 78533908de1a1f43cbfac9054dcfe6ec83edce9d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948751"
---
# <a name="configuring-additional-lsa-protection"></a>設定額外的 LSA 保護

>適用於：Windows Server (半年度管道)、Windows Server 2016

這個適用於 IT 專業人員的主題說明如何為本機安全性授權 (LSA) 處理程序設定額外的保護，以避免可能洩漏認證的程式碼插入。

LSA 包含本機安全性授權伺服器服務 (LSASS) 處理程序，會驗證使用者的本機和遠端登入，以及強制執行本機安全性原則。 Windows 8.1 作業系統為 LSA 提供額外的保護，以防止未受保護的進程讀取記憶體和插入程式碼。 這為 LSA 儲存和管理的認證提供了額外的安全性。 您可以在 Windows 8.1 中設定 LSA 的受保護進程設定，但無法在 Windows RT 8.1 中設定。 當此設定與安全開機一起搭配使用時，因為停用 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa 登錄機碼沒有作用，所以會有額外的保護。

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>外掛程式或驅動程式的受保護處理程序需求
LSA 外掛程式或驅動程式必須符合以下條件，才能順利載入為受保護的處理程序：

1.  簽章驗證

    受保護的模式要求載入 LSA 中的所有外掛程式都要以 Microsoft 簽章進行數位簽署。 因此，未簽署或沒有以 Microsoft 簽章進行簽署的所有外掛程式將無法載入 LSA。 例如，智慧卡驅動程式、加密編譯外掛程式，以及密碼篩選器等外掛程式。

    屬於驅動程式的 LSA 外掛程式 (例如，智慧卡驅動程式) 需要使用 WHQL 認證進行簽署。 如需詳細資訊，請參閱[WHQL 版本](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx)簽章。

    沒有 WHQL 認證程序的 LSA 外掛程式必須使用[適用於 LSA 的檔案簽署服務](https://go.microsoft.com/fwlink/?LinkId=392590)進行簽署。

2.  遵守 Microsoft 安全性開發週期 (SDL) 程序指引

    所有外掛程式都必須符合適用的 SDL 程序指引。 如需詳細資訊，請參閱 [Microsoft 安全性開發週期 (SDL) 附錄](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx)。

    即使外掛程式經過 Microsoft 簽章的適當簽署，與 SDL 程序不相容也可能無法載入外掛程式。

#### <a name="recommended-practices"></a>建議的做法
全面部署功能之前，先使用以下清單徹底測試 LSA 保護是否已啟用：

-   識別您組織中使用的所有 LSA 外掛程式和驅動程式。
    這包含非 Microsoft 驅動程式，或智慧卡驅動程式和加密編譯外掛程式等外掛程式，以及用於強制執行密碼篩選器或密碼變更通知的所有內部開發軟體。

-   確認所有 LSA 外掛程式都以 Microsoft 憑證進行數位簽署，才不會無法載入外掛程式。

-   確認所有經過正確簽署的外掛程式可順利載入 LSA 中，且依預期執行。

-   使用稽核記錄識別無法以受保護的處理程序執行的 LSA 外掛程式和驅動程式。

#### <a name="limitations-introduced-with-enabled-lsa-protection"></a>啟用 LSA 保護所引進的限制

如果已啟用 LSA 保護，您就無法對自訂 LSA 外掛程式進行調試。
當它是受保護的進程時，您無法將偵錯工具附加至 LSASS。
一般情況下，沒有支援的方法可讓您偵測執行中的受保護進程。

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>如何識別無法以受保護的處理程序執行的 LSA 外掛程式和驅動程式
本節中描述的事件位於 [應用程式及服務記錄檔\Microsoft\Windows\CodeIntegrity] 底下的作業記錄中。 這些事件可以協助您識別因為簽署原因而無法載入的 LSA 外掛程式和驅動程式。 您可以使用 **wevtutil** 命令列工具管理這些事件。 如需此工具的相關資訊，請參閱 [Wevtutil](../../administration/windows-commands/Wevtutil.md)。

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>選擇之前：如何識別 lsass.exe 載入的外掛程式和驅動程式
您可以使用稽核模式識別無法在 LSA 保護模式中載入的 LSA 外掛程式和驅動程式。 在稽核模式中，系統會產生事件記錄檔，識別當 LSA 保護模式啟用時，無法在 LSA 載入的所有外掛程式和驅動程式。 系統會記錄這些訊息，但不會封鎖外掛程式或驅動程式。

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>透過編輯登錄在單一電腦上針對 Lsass.exe 啟用稽核模式

1.  開啟登錄編輯程式 (RegEdit.exe)，瀏覽到登錄機碼，位於：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe。

2.  將登錄機碼的值設定為 **AuditLevel=dword:00000008**。

3.  重新啟動電腦。

分析事件 3065 和事件 3066 的結果。

在此之後，您可能會在事件檢視器中看到這些事件： Codeintegrity/Operational：

-   **事件 3065**：此事件記錄了程式碼完整性檢查判斷某個處理程序 (通常是 lsass.exe) 嘗試載入不符合共用區段安全性需求的特定驅動程式。 但是由於所設定的系統原則，允許載入映像。

-   **事件 3066**：此事件記錄了程式碼完整性檢查判斷某個處理程序 (通常是 lsass.exe) 嘗試載入不符合 Microsoft 簽署等級需求的特定驅動程式。 但是由於所設定的系統原則，允許載入映像。

> [!IMPORTANT]
> 系統上附加和啟用核心偵錯工具時，不會產生這些操作事件。
>
> 如果外掛程式或驅動程式包含共用區段，會以事件 3065 記錄事件 3066。 除非外掛程式不符合 Microsoft 簽署等級需求，否則移除共用區段應該可以避免同時發生這兩個事件。

若要為網域中的多部電腦啟用稽核模式，您可以使用群組原則的登錄用戶端延伸，部署 Lsass.exe 稽核等級登錄值。 您需要修改 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe 登錄機碼。

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>在 GPO 中建立 AuditLevel 值設定

1.  開啟 [群組原則管理主控台] (GPMC)。

2.  建立新的群組原則物件 (GPO)，該物件在網域層級連結或連結到包含您電腦帳戶的組織單位。 您也可以選取已經部署的 GPO。

3.  在 GPO 上按一下滑鼠右鍵，然後按一下 [編輯]**** 開啟 [群組原則管理編輯器]。

4.  依序展開 [電腦設定]****、[喜好設定]**** 及 [Windows 設定]****。

5.  在 [登錄]**** 上按一下滑鼠右鍵，指向 [新增]****，然後按一下 [登錄項目]****。 隨即顯示 [新登錄內容]**** 對話方塊。

6.  在 [ **Hive** ] 清單中，按一下 [ **HKEY_LOCAL_MACHINE]。**

7.  在 [機碼路徑]**** 清單中，瀏覽到 **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe**。

8.  在 [值名稱]**** 方塊中輸入 **AuditLevel**。

9. 在 [數值類型]**** 方塊中，按一下以選取 [REG_DWORD]****。

10. 在 [數值資料]**** 方塊中輸入 **00000008**。

11. 按一下 [確定]  。

> [!NOTE]
> GPO 變更必須複寫到網域中的所有網域控制站，GPO 才會生效。

若要選擇多部電腦上的額外 LSA 保護，您可以透過修改 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa，使用群組原則的登錄用戶端延伸。 執行方法的相關步驟，請參閱本主題中的[如何設定認證的額外 LSA 保護](#BKMK_HowToConfigure)。

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>選擇之後：如何識別 lsass.exe 載入的外掛程式和驅動程式
您可以使用事件記錄檔識別無法在 LSA 保護模式中載入的 LSA 外掛程式和驅動程式。 當 LSA 受保護的處理程序啟用時，系統會產生事件記錄檔，可識別無法在 LSA 載入的所有外掛程式和驅動程式。

分析事件 3033 和事件 3063 的結果。

在此之後，您可能會在事件檢視器中看到這些事件： Codeintegrity/Operational：

-   **事件 3033**：此事件記錄了程式碼完整性檢查判斷某個處理程序 (通常是 lsass.exe) 嘗試載入不符合 Microsoft 簽署等級需求的驅動程式。

-   **事件 3063**：此事件記錄了程式碼完整性檢查判斷某個處理程序 (通常是 lsass.exe) 嘗試載入不符合共用區段安全性需求的驅動程式。

共用區段通常是程式設計技術的結果，可讓執行個體資料與使用相同資訊安全內容的其他處理程序互動。 這可能產生安全性弱點。

## <a name="how-to-configure-additional-lsa-protection-of-credentials"></a><a name="BKMK_HowToConfigure"></a>如何設定認證的額外 LSA 保護
在執行 Windows 8.1 (不含安全開機或 UEFI) 的裝置上，您可以執行本節所述的程式來進行設定。 對於執行 Windows RT 8.1 的裝置，lsass.exe 的保護一律會啟用，且無法關閉。

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>在使用或不使用安全開機和 UEFI 的 x86 型或 x64 型裝置上
在使用安全開機或 UEFI 的 x86 型或 x64 型裝置上，當使用登錄機碼啟用 LSA 保護時，uefi 固件中會設定 UEFI 變數。 此設定儲存在韌體中時，無法刪除或在登錄機碼中變更 UEFI 變數。 UEFI 變數必須重設。

不支援 UEFI 或停用安全開機的 x86 型或 x64 型裝置無法將 LSA 保護的設定儲存在韌體中，而是僅依賴登錄機碼。 在此情況下，透過遠端存取裝置可以停用 LSA 保護。

您可以使用以下程序啟用或停用 LSA 保護：

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>啟用單一電腦上的 LSA 保護

1.  開啟登錄編輯程式 (RegEdit.exe)，瀏覽到登錄機碼，位於：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  將登錄機碼的值設定為 "RunAsPPL"=dword:00000001。

3.  重新啟動電腦。

##### <a name="to-enable-lsa-protection-using-group-policy"></a>使用群組原則啟用 LSA 保護

1.  開啟 [群組原則管理主控台] (GPMC)。

2.  建立新的 GPO，該物件在網域層級連結或連結到包含您電腦帳戶的組織單位。 您也可以選取已經部署的 GPO。

3.  在 GPO 上按一下滑鼠右鍵，然後按一下 [編輯]**** 開啟 [群組原則管理編輯器]。

4.  依序展開 [電腦設定]****、[喜好設定]**** 及 [Windows 設定]****。

5.  在 [登錄]**** 上按一下滑鼠右鍵，指向 [新增]****，然後按一下 [登錄項目]****。 隨即顯示 [新登錄內容]**** 對話方塊。

6.  在 [登錄區]**** 清單中，按一下 [**HKEY_LOCAL_MACHINE**]。

7.  在 [**金鑰路徑**] 清單中，流覽至**SYSTEM\CurrentControlSet\Control\Lsa**。

8.  在 [**值名稱**] 方塊中，輸入**RunAsPPL**。

9. 在 [數值類型]**** 方塊中，按一下 [REG_DWORD]****。

10. 在 [**數值資料**] 方塊中，輸入**00000001**。

11. 按一下 [確定]  。

##### <a name="to-disable-lsa-protection"></a>停用 LSA 保護

1.  開啟登錄編輯程式 (RegEdit.exe)，瀏覽到登錄機碼，位於：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  從登錄機碼刪除以下值："RunAsPPL"=dword:00000001。

3.  如果裝置使用安全開機，使用本機安全性授權 (LSA) 受保護的處理程序選擇退出工具刪除 UEFI 變數。

    如需選擇退出工具的詳細資訊，請參閱[從 Microsoft 官方下載中心下載本機安全性授權 (LSA) 受保護的處理程序選擇退出](https://www.microsoft.com/download/details.aspx?id=40897)。

    如需管理安全開機的詳細資訊，請參閱 [UEFI 韌體](https://technet.microsoft.com/library/hh824898.aspx)。

    > [!WARNING]
    > 安全開機關閉時，會重設所有與安全開機和 UEFI 相關的設定。 您應該只在所有其他停用 LSA 保護的方法都失敗後，才關閉安全開機。

### <a name="verifying-lsa-protection"></a>驗證 LSA 保護
若要確認 Windows 啟動時 LSA 是否在受保護模式下啟動，請在 [Windows 記錄]**** 的 [系統]**** 記錄檔中搜尋下列 WinInit 事件：

-   12: LSASS.exe 已以受保護的處理程序方式啟動，層級為: 4

## <a name="additional-resources"></a>其他資源
[認證保護和管理](credentials-protection-and-management.md)

[LSA 的檔案簽署服務](https://go.microsoft.com/fwlink/?LinkId=392590)


