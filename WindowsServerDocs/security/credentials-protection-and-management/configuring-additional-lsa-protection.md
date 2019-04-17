---
title: "設定其他 LSA 保護"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fcfb0dab10d28413cf4ad06dd583274f217c91fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-additional-lsa-protection"></a>設定其他 LSA 保護

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員如何設定以防止危及認證的程式碼注入本機安全性授權單位 (LSA) 處理程序額外的保護。

LSA，包括本機安全性授權單位伺服器服務 (LSASS) 處理程序，驗證使用者的本機和遠端登入增益集，並執行本機安全性原則。 Windows 8.1 作業系統提供其他 LSA 避免朗讀記憶體和注入由未受保護的處理程序的程式碼保護。 新增的安全性提供的認證 LSA 儲存和管理。 LSA 的受保護的程序設定可在 Windows 8.1 中，設定，但無法在 Windows RT 8.1 中設定。 在 [安全開機搭配使用此設定時，停用 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa 登錄鍵已經不會生效，因為達成額外的保護。

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>受保護的程序需求插或驅動程式
適用於 LSA 外掛程式或驅動程式成功載入受保護的程序，必須符合下列條件：

1.  簽章驗證

    「受保護的模式需要的任何外掛程式載入 LSA 數位簽章使用 Microsoft 簽章。 因此，將無法載入 LSA 在任何插未經簽署或不登入 Microsoft 簽章。 這些插範例包括智慧卡驅動程式、密碼編譯插，以及密碼篩選。

    使用 WHQL 憑證來簽署需要 LSA 插的驅動程式，例如智慧卡驅動程式。 如需詳細資訊，請查看[WHQL 發行簽章（Windows 驅動程式）](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx)。

    必須簽署 LSA 插不具有 WHQL 認證處理程序，使用[檔案服務 LSA 登入](https://go.microsoft.com/fwlink/?LinkId=392590)。

2.  遵循 Microsoft 安全性開發週期 (SDL) 處理程序指導方針

    所有插必須符合適用 SDL 處理程序指導方針。 如需詳細資訊，請查看[Microsoft 安全性開發週期 (SDL) 附錄](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx)。

    即使插的適當簽署使用 Microsoft 簽章，不符合 SDL 程序可能會導致無法載入外掛程式。

#### <a name="recommended-practices"></a>建議的做法
使用下面完全測試 LSA 保護會讓之前廣泛地部署的功能：

-   找出的所有 LSA 插與在組織中使用的驅動程式。 
    這包括非 Microsoft 的驅動程式或插例如智慧卡驅動程式與密碼編譯插，以及任何內部開發用來執行密碼篩選或密碼變更通知的軟體。

-   請確定的所有 LSA 插以數位簽署的憑證 Microsoft 外掛程式不會載入。

-   請確定所有正確簽署插，可以順利 LSA 載入並執行如預期般運作。

-   使用稽核登找出 LSA 插和驅動程式無法執行受保護的程序。

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>如何找出 LSA 插和驅動程式無法執行受保護的程序
這一節中所述的事件位於 [可操作 \ [應用程式及服務 Logs\Microsoft\Windows\CodeIntegrity 登入。 他們可以協助您找出 LSA 插和驅動程式載入因為登入的原因而失敗。 若要管理這些活動，您可以使用**wevtutil**命令列工具。 此工具的相關資訊，請查看[Wevtutil](../../administration/windows-commands/Wevtutil.md)。

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>之前中選擇：如何找出插和載入 lsass.exe 驅動程式
您可以找出 LSA 插和驅動程式，將無法載入 LSA 保護模式中使用的稽核模式。 在稽核模式時，系統將會產生事件登，找出的所有插和驅動程式，將無法載入 LSA 下，如果 LSA 保護會讓。 訊息是登入，而不會封鎖插或驅動程式。

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>編輯登錄的 Lsass.exe 約定稽核模式一部電腦上

1.  打開作業系統 (RegEdit.exe)，並瀏覽至位於登錄鍵：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image 檔案執行 Options\LSASS.exe。

2.  設定的值登錄鍵**AuditLevel = dword:00000008**。

3.  電腦重新開機。

分析事件 3065 和事件 3066 的結果。

-   **事件 3065**：此事件記錄的程式碼完整性檢查以判斷處理程序 (通常是 lsass.exe) 嘗試載入不符合安全性需求共用區段特定驅動程式。 不過，系統原則設定，因為影像已載入允許。

-   **事件 3066**：此事件記錄的程式碼完整性檢查以判斷處理程序 (通常是 lsass.exe) 嘗試載入未符合 Microsoft 登入層級需求特定驅動程式。 不過，系統原則設定，因為影像已載入允許。

> [!IMPORTANT]
> 這些操作事件不專時附加，在系統核心偵錯工具。
> 
> 如果外掛程式或驅動程式包含共用區段，3066 事件的事件 3065 登。 移除共用區段應該避免這兩個事件發生除非外掛程式不符合 Microsoft 登入層級的需求。

若要以便稽核模式網域中的多部電腦，您可以使用適用於群組原則登錄 Client 端延伸部署 Lsass.exe 稽核層級登錄值。 您需要修改 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image 檔案執行 Options\LSASS.exe 登錄金鑰。

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>若要建立 GPO AuditLevel 值設定

1.  打開群組原則管理主控台 (GPMC)。

2.  建立新群組原則物件 (GPO) 網域層級的連結或組織單位，其中包含您電腦帳號的連結。 或者，您可以選取 GPO 已經部署。

3.  GPO 上按一下滑鼠右鍵，然後按一下**編輯**打開群組原則管理編輯器。

4.  展開**電腦設定**，展開 [**的喜好設定**，然後展開 [**的 Windows 設定**。

5.  以滑鼠右鍵按一下**登錄**，指向 [**新**，然後按一下 [**登錄項目**。 **新登錄屬性**對話方塊中出現。

6.  在**Hive**清單中，按**跳。**

7.  在**鍵路徑**清單中，瀏覽] **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image 檔案執行 Options\LSASS.exe**。

8.  在**值名稱**方塊中，輸入**AuditLevel**。

9. 在**值類型**方塊中，按一下以選取**呼叫完成**。

10. 在**數值資料**方塊中，輸入**00000008**。

11. 按一下**[確定]**。

> [!NOTE]
> Gpo 才會生效，必須網域中的所有網域控制站都複製 GPO 變更。

來選擇中的多部電腦上的其他 LSA 保護，您可以使用登錄 Client 端延伸適用於群組原則來修改 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。 了解如何執行此步驟，請查看[如何設定額外的 LSA 保護認證的](#BKMK_HowToConfigure)本主題中。

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>之後中選擇：如何找出插和載入 lsass.exe 驅動程式
找出 LSA 插和驅動程式無法載入 LSA 保護模式，您可以使用事件登入。 當支援的受保護的 LSA 程序時，系統會產生找出的所有插和驅動程式無法在 LSA 載入的事件登。

分析事件 3033 和事件 3063 的結果。

-   **事件 3033**：此事件記錄的程式碼完整性檢查判斷處理程序 (通常是 lsass.exe) 嘗試載入的驅動程式，不符合 Microsoft 登入層級的需求。

-   **事件 3063**：此事件記錄的程式碼完整性檢查以判斷處理程序 (通常是 lsass.exe) 嘗試載入的驅動程式，不符合安全性需求共用區段。

共用的章節通常程式設計技術，允許執行個體資料與使用的相同的安全性層級的其他處理程序進行互動的結果。 這可以建立安全性弱點。

## <a name="BKMK_HowToConfigure"></a>如何設定認證其他 LSA 保護
在裝置上執行 Windows 8.1（含或 UEFI 安全開機不），設定可能是執行此一節中所述的程序。 針對執行 Windows RT 8.1 的裝置，一律會支援 lsass.exe 保護，並不會被關閉。

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>X86 型或 x64 型或不使用安全開機和 UEFI 的裝置
X86 型或 x64 型在裝置上使用，以及 UEFI 安全開機的 UEFI 變數設定 UEFI 韌體中使用登錄鍵可以 LSA 保護。 設定會儲存在韌體中，無法刪除或變更登錄鍵 UEFI 變數。 UEFI 變數必須重設。

x86 型或 x64 型不支援 UEFI 或安全開機的裝置已停用，無法儲存在韌體中的 LSA 保護的設定和只依賴登錄金鑰的狀態。 在本案例中，就可以使用裝置的遠端存取停用 LSA 保護。

您可以使用下列程序，讓或停用 LSA 保護：

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>若要讓 LSA 一部電腦上的保護

1.  打開作業系統 (RegEdit.exe)，並瀏覽至位於登錄鍵：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  設定的值登錄鍵: [RunAsPPL」= dword:00000001。

3.  電腦重新開機。

##### <a name="to-enable-lsa-protection-using-group-policy"></a>若要讓 LSA 保護使用群組原則

1.  打開群組原則管理主控台 (GPMC)。

2.  建立新的 GPO 連結網域層級，或包含您電腦帳號組織單位，連結。 或者，您可以選取 GPO 已經部署。

3.  GPO 上按一下滑鼠右鍵，然後按一下**編輯**打開群組原則管理編輯器。

4.  展開**電腦設定**，展開 [**的喜好設定**，然後展開 [**的 Windows 設定**。

5.  以滑鼠右鍵按一下**登錄**，指向 [**新**，然後按一下 [**登錄項目**。 **新登錄屬性**對話方塊中出現。

6.  在**Hive**清單中，按**跳**。

7.  在**鍵路徑**清單中，瀏覽] **SYSTEM\CurrentControlSet\Control\Lsa**。

8.  在**值名稱**方塊中，輸入**RunAsPPL**。

9. 在**值類型**方塊中，按**呼叫完成**。

10. 在**數值資料**方塊中，輸入**00000001**。

11. 按一下**[確定]**。

##### <a name="to-disable-lsa-protection"></a>若要停用 LSA 保護

1.  打開作業系統 (RegEdit.exe)，並瀏覽至位於登錄鍵：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  從登錄鍵 delete 下列值:「RunAsPPL「= dword:00000001。

3.  使用 [本機安全性授權單位 (LSA) 受保護的處理程序退出工具 delete 如果裝置使用 [安全開機的 UEFI 變數。

    適用於退出工具的相關詳細資訊，請查看[下載本機安全性授權單位 (LSA) 受保護的處理程序退出官方的 Microsoft 下載中心」的](https://www.microsoft.com/download/details.aspx?id=40897)。

    如需有關管理安全開機的詳細資訊，請查看[UEFI 韌體](https://technet.microsoft.com/library/hh824898.aspx)。

    > [!WARNING]
    > 當安全開機已關閉時，則會重設所有的安全開機和相關 UEFI 設定。 停用 LSA 保護所有其他方式失敗時，才，您應該會關閉安全開機。

### <a name="verifying-lsa-protection"></a>確認 LSA 保護
若要探索如果 LSA 開始使用受保護模式開始使用 Windows 時，搜尋下列 WinInit 事件在**系統**在登入**Windows 登**:

-   12: LSASS.exe 受保護層級的處理程序以開始使用：4

## <a name="additional-resources"></a>其他資源
[認證保護與管理](credentials-protection-and-management.md)

[登入服務 LSA 檔案](https://go.microsoft.com/fwlink/?LinkId=392590)


