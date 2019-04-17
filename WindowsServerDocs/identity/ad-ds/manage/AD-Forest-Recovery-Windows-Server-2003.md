---
title: "廣告樹系修復 Windows Server 2003 修復"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: daebbc65b9864554fde839c3865f507ab787e6b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>廣告樹系修復 Windows Server 2003 修復

>適用於： Windows Server 2003

本主題包含網域控制站 （網域控制站），執行 Windows Server 2003 的樹系修復程序。 一般的樹系修復程序不不同與 Windows Server 2003 Dc，但特定程序可以不同的工具而有所不同。 例如，Ntdsutil.exe 可以用於備份及還原 Dc 執行 Windows Server 2003 Dc，而 Windows Server 備份或 Wbadmin.exe 是執行 Windows Server 2008 的網域控制站用於或更新版本。  
  
-   [備份資料](#Backing-up-the-System-State-data)  
  
-   [執行未授權還原](#Performing-a-nonauthoritative restore)  
  
-   [安裝和設定的 DNS 伺服器服務](#Install-and-configure-the-DNS-Server-service)  
  
  
## <a name="backing-up-the-system-state-data"></a>備份資料  
 使用下列程序資料備份系統狀態，以及您已經選取目前的備份操作，DC 執行 Windows Server 2003 的任何其他資料。 Windows Server 2003 包含 Ntbackup 工具，您可以用來資料備份系統狀態。  
  
 資格在**系統管理員**或**備份電信業者**，或等的最低需求備份的檔案和資料夾。   
  
 如果您的備份資料磁帶，備份程式指出不未使用的 media 可用，您可能必須使用抽取式存放裝置。 這會將磁帶的免費媒體集區加入備份使用。  
  
 您只備份本機電腦上的資料。 您無法備份遠端電腦上。  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>若要備份資料網域控制站在執行 Windows Server 2003  
  
1.  按一下**[開始]**，指向 [**所有程式**，指向 [**附屬應用程式]**，指向 [**系統工具**，，然後按一下 [**備份**。  
  
2.  在**歡迎**頁面上，按**進階模式**。  
  
3.  在**備份**索引標籤上，選取您想要備份的檔案、 資料夾，或磁碟機] 核取方塊。  
  
4.  選取 [**系統狀態**核取方塊。  
  
5.  按一下**開始備份]**。  
  
  
## <a name="performing-a-nonauthoritative-restore"></a>執行未授權還原  
 使用下列程序未授權的執行 Windows Server 2003 俠還原。 在 Windows Server 2003 Active Directory 執行未授權的還原，自動執行未授權的 SYSVOL 還原。 不所需的任何額外的步驟。  
  
> [!NOTE]
>  如果您也會重新安裝 Windows Server 2003 作業系統，您可能會或可能不加入網域的電腦，您可以命名任何到電腦時設定的作業系統。 無法安裝 Active Directory。 之後重新安裝作業系統，直接移至步驟 4。  
  
 在 Windows Server 2003 網域控制站還原系統狀態資料，您需要也重新安裝前修復 Dc 執行的任何軟體應用程式。 還原 AD DS 網域中的第一個 DC 上也會還原登錄因為兩者是系統狀態資料的一部分。 如果您有任何這些 Dc 上執行的應用程式，以及他們必須登錄中儲存的任何資訊，請牢記這點。  
  
 來節省時間需要重新安裝軟體判斷需要網域控制站上安裝的應用程式是否與 virtual 俠複製相容。 這類應用程式可以來源 DC 上安裝之前複製以節省時間和為了需要複製 virtual 網域控制站在您安裝它們。  
  
#### <a name="to-perform-a-nonauthoritative-restore"></a>若要還原未授權  
  
1.  開始 DC 之後，長按 F8 中 Directory 服務還原模式 (DSRM) 重新開機。  
  
2.  選取 [ **Directory 服務還原模式 （Windows 只有網域控制站）**。  
  
3.  選取您想要還原模式中的 [開始] 的作業系統。  
  
4.  （您可以只使用本機電腦帳號，不網域登入選項可） 以系統管理員身分登入。  
  
5.  在命令提示字元中，輸入**ntbackup**，然後按 ENTER 鍵。  
  
6.  在**歡迎**頁面上，按一下 [**進階模式**，]，然後選取**還原和管理」媒體**索引標籤 (不要選取 [**還原精靈**。)  
  
7.  選取適當的備份還原的並確保檔案**系統磁碟**和**系統狀態**核取方塊已經選取。  
  
8.  按一下**開始還原]**。  
  
9. 還原完成時，將電腦重新開機。  
  
 使用下列程序上執行 Windows Server 2003 俠執行的 SYSVOL 授權 （也稱為主要） 還原。 僅第一個 Windows Server 2003 DC 還原網域中的執行此程序。  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>若要執行的 SYSVOL 授權還原  
  
1.  執行步驟 1 到 8 之前的程序。  
  
2.  在**確認還原**對話方塊中，按**進階]**。  
  
3.  若要執行的 SYSVOL 授權，選取核取方塊**還原複製資料集，還原的資料標示為所有複本主要資料**。  
  
    > [!NOTE]
    >  將還原的資料的主要資料備份相當設定為**BurFlags**下下列子機碼 D4 到的項目：  
    >   
    >  **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative 複本 Sets\\** *GUID*  
  
4.  還原完成時，將電腦重新開機。  
  
 
## <a name="install-and-configure-the-dns-server-service"></a>安裝和設定的 DNS 伺服器服務  
 如果您已從備份還原俠執行的 Windows Server 2003，您可以安裝不需要任何網路來連接 DC 的 DNS 伺服器。  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>安裝和設定的 DNS 伺服器服務  
  
1.  打開 Windows 元件精靈。 打開精靈：  
  
    -   按一下**[開始]**，按一下 [ **[控制台]**，然後按一下 [**新增或移除程式**。  
  
    -   按一下**[新增/移除 Windows 元件**。  
  
2.  在**元件**，請選取**網路服務]**核取方塊，並按**詳細資料**。  
  
3.  在**的網路服務的元件**，請選取**網域名稱系統 」 (DNS)**核取方塊、 按一下 [ **[確定]**，然後按一下**下一步**。  
  
4.  如果您的提示，請在**複製檔案的**，輸入完整 distribution 檔案的路徑，然後按**[確定]**。  
  
     安裝之後，請完成下列步驟來設定 DNS 伺服器。  
  
5.  按一下**[開始]**，指向 [**所有程式**，指向 [**系統管理工具]**，，然後按一下**DNS**。  
  
6.  重要故障之前的 DNS 伺服器建立已裝載的相同 DNS 網域名稱 DNS 區域。 如需詳細資訊，查看 [新增正向對應區域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。  
  
7.  設定存在之前重要故障 DNS 資料。 例如：  
  
    -   設定，並儲存在 AD DS DNS 區域。 如需詳細資訊，請變更區域類型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。  
  
    -   設定的網域控制站定位器 （DC 定位） 以允許安全的動態更新的資源記錄授權 DNS 區域。 如需詳細資訊，查看 [允許只有安全動態更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。  
  
8.  確定家長 DNS 區域包含委派資源記錄 （伺服器 （奈秒） 和名稱黏附主機的資源 (A) 記錄） 子女區此 DNS 伺服器上。 如需詳細資訊，請建立區域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。  
  
9. 設定 DNS 之後，在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
     **網路停止 netlogon**  
  
10. 輸入下列命令，並按一下 ENTER:  
  
     **網路的 [開始] 畫面 netlogon**  
  
    > [!NOTE]
    >  網路登入將進行登記 DC 定位資源記錄 DNS 在這個網域控制站。 如果您的子女網域中的伺服器上安裝的 DNS 伺服器服務，此 DC 將無法立即登記其記錄。 這是因為它是目前隔離的修復程序，並為主要的 DNS 伺服器的一部分森林根 DNS 伺服器。 如同之前嚴重損壞，以避免 DC 服務查詢失敗相同的 IP 位址設定這部電腦。

## <a name="next-steps"></a>後續步驟
-   [廣告樹系修復必要條件](AD-Forest-Recovery-Prerequisties.md)  
-   [廣告樹系修復-設計自訂樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [廣告樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
-   [廣告樹系修復-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [廣告樹系修復-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  
-   [廣告樹系修復-常見問題集](AD-Forest-Recovery-FAQ.md)  
-   [廣告樹系修復-復原 Multidomain 樹系單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [廣告樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md) 
