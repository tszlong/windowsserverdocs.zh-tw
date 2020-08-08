---
title: AD 樹系復原-Windows Server 2003 修復
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 6693f2af67e0de32e54eec309b62f5333505f559
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943646"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>AD 樹系復原-Windows Server 2003 修復

>適用于： Windows Server 2003

本主題包含執行 Windows Server 2003 之網域控制站 (Dc) 的樹系復原程式。 樹系復原的一般程式與 Windows Server 2003 Dc 沒有任何不同之處，但因為有不同的工具，所以特定程式可能有所不同。 例如，Ntdsutil.exe 可以用來備份和還原執行 Windows Server 2003 Dc 的 Dc，而 Windows Server Backup 或 Wbadmin.exe 則是用於執行 Windows Server 2008 或更新版本的 Dc。

- [備份系統狀態資料](#backing-up-the-system-state-data)
- [執行非系統授權還原](#performing-a-nonauthoritative-restore)
- [安裝和設定 DNS 伺服器服務](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>備份系統狀態資料
使用下列程式來備份執行 Windows Server 2003 之 DC 的系統狀態資料，以及您為目前備份作業選取的任何其他資料。 Windows Server 2003 包含 Ntbackup 工具，可用來備份系統狀態資料。

若要備份檔案和資料夾，至少需要**Administrators**或**Backup Operators**的成員資格或同等許可權。

如果您要將系統狀態資料備份到磁帶，而備份程式指出沒有未使用的媒體，您可能必須使用卸除式存放裝置。 這會將您的磁帶新增到可用的媒體集區，讓備份可以使用它。

您只能備份本機電腦上的系統狀態資料。 您無法在遠端電腦上進行備份。

### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>若要備份執行 Windows Server 2003 之網域控制站上的系統狀態資料

1. 按一下 [**開始**]，依序指向 [**所有程式**]、[**附屬**應用程式] 和 [**系統工具**]，然後按一下 [**備份**]。
2. 在 [**歡迎使用**] 頁面上，按一下 [**高級模式]**。
3. 在 [**備份**] 索引標籤上，選取您想要備份的任何磁片磁碟機、資料夾或檔案的核取方塊。
4. 選取 [**系統狀態**] 核取方塊。
5. 按一下 [**開始備份**]。

## <a name="performing-a-nonauthoritative-restore"></a>執行非系統授權還原

使用下列程式，執行執行 Windows Server 2003 的 DC 的非系統授權還原。 藉由在 Windows Server 2003 的 Active Directory 上執行非系統授權還原，您會自動執行 SYSVOL 的非系統授權還原。 不需要任何額外步驟。

> [!NOTE]
> 如果您也要重新安裝 Windows Server 2003 作業系統，您可能會或可能不會將電腦加入網域，而且可以在安裝作業系統期間，為電腦提供任何名稱。 請勿安裝 Active Directory。 重新安裝作業系統之後，請直接移至步驟4。

在您只還原系統狀態資料的 Windows Server 2003 網域控制站上，您也必須在復原之前，重新安裝在 Dc 上執行的任何軟體應用程式。 還原網域中第一個 DC 上的 AD DS 也會還原登錄，因為兩者都是系統狀態資料的一部分。 如果您在這些 Dc 上執行了任何應用程式，而且它們有儲存在登錄中的任何資訊，請記住這一點。

若要節省重新安裝軟體所需的時間，請判斷需要安裝在 Dc 上的應用程式是否與虛擬 DC 複製相容。 這類應用程式可以安裝在來源 DC 上，然後再進行複製，以節省在複製的虛擬 Dc 上安裝所需的時間和工作。

### <a name="to-perform-a-nonauthoritative-restore"></a>執行非系統授權還原

1. 當您啟動 DC 之後，請按 F8 以目錄服務還原模式重新開機電腦 (DSRM) 。
2. 選取 [**目錄服務還原模式] ([僅) Windows 網域控制站] **。
3. 選取您想要在還原模式中啟動的作業系統。
4. 以系統管理員身分登入 (您只能使用本機電腦帳戶，) 沒有可用的網域登入選項。
5. 在命令提示字元中，輸入**ntbackup**，然後按 enter。
6. 在 [**歡迎使用**] 頁面上，按一下 [**先進模式**]，然後選取 [**還原並管理媒體**] 索引標籤。 (不要選取 [**還原嚮導]**。 ) 
7. 選取要從中還原的適當備份檔案，並確定已選取 [**系統磁片**] 和 [**系統狀態**] 核取方塊。
8. 按一下 **[開始還原]**。
9. 當還原作業完成時，請重新開機電腦。

使用下列程式，在執行 Windows Server 2003 的 DC 上執行授權 (也稱為主要) 還原 SYSVOL。 請只在網域中還原的第一個 Windows Server 2003 DC 上執行此程式。

### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>執行 SYSVOL 的授權還原

1. 執行前一個程式中的步驟1到8。
2. 在 [**確認還原**] 對話方塊中，按一下 [ **Advanced**]。
3. 若要執行 SYSVOL 的授權還原，請選取 [**還原複寫的資料集時] 核取方塊，將還原的資料標示為所有複本的主要資料**。

   > [!NOTE]
   > 將還原的資料標示為備份中的主要資料，相當於在下列登錄子機碼下將**BurFlags**專案設定為 D4：
   >
   > **HKEY_LOCAL_MACHINE \System\currentcontrolset\services\ntfrs\parameters\cumulative 複本集 \\ ***GUID*

4. 當還原作業完成時，請重新開機電腦。

## <a name="install-and-configure-the-dns-server-service"></a>安裝和設定 DNS 伺服器服務

如果您從備份還原的 DC 正在執行 Windows Server 2003，您可以安裝 DNS 伺服器，而不需要將 DC 連接到任何網路。

### <a name="to-install-and-configure-the-dns-server-service"></a>安裝和設定 DNS 伺服器服務

1. 開啟 [Windows 元件]。 若要開啟嚮導：

   - 按一下 [開始]****、[控制台]****，然後按一下 [新增或移除程式]****。
   - 按一下 [**新增/移除 Windows 元件**]。

2. 在 [**元件**] 中，選取 [**網路服務**] 核取方塊，然後按一下 [**詳細資料**]。
3. 在 [**網路服務的子元件**] 中，選取 [**網域名稱系統 (DNS) ** ] 核取方塊，按一下 **[確定]**，然後按 **[下一步**]。
4. 如果出現提示，請在 [**複製檔案來源**] 中，輸入散發檔案的完整路徑，然後按一下 **[確定]**。

   安裝之後，請完成下列步驟來設定 DNS 伺服器。

5. 按一下 [**開始**]，指向 [**所有程式**]，指向 [系統**管理工具**]，然後按一下 [ **DNS**]。
6. 為 DNS 伺服器上裝載的相同 DNS 功能變數名稱建立 DNS 區域，然後才會發生嚴重的故障。 如需詳細資訊，請參閱 () 新增正向對應區域 [https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574) 。
7. 在嚴重的故障之前，設定 DNS 資料。 例如：

   - 設定要儲存在 AD DS 中的 DNS 區域。 如需詳細資訊，請參閱將區欄位型別變更 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)) 。
   - 設定網域控制站定位器的授權 DNS 區域 (DC 定位器) 資源記錄，以允許安全的動態更新。 如需詳細資訊，請參閱僅允許 () 的安全動態更新 [https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580) 。

8. 請確定父 DNS 區域包含委派資源記錄 (名稱伺服器 (NS) ，並將主機 () 資源記錄，) 此 DNS 伺服器上裝載的子領域。 如需詳細資訊，請參閱 () 建立區域委派 [https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562) 。
9. 設定 DNS 之後，請在命令提示字元中輸入下列命令，然後按 ENTER 鍵：

   **net stop netlogon**

10. 輸入下列命令，然後按 ENTER：

    **net start netlogon**

    > [!NOTE]
    > Net Logon 會在 DNS 中為此 DC 註冊 DC 定位器資源記錄。 如果您要在子域中的伺服器上安裝 DNS 伺服器服務，此 DC 將無法立即註冊其記錄。 這是因為它目前在復原過程中被隔離，而其主要 DNS 伺服器是樹系根 DNS 伺服器。 使用與嚴重損壞之前相同的 IP 位址來設定這部電腦，以避免 DC 服務查閱失敗。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原計畫](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系復原-識別問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-修復多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原-具有 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
