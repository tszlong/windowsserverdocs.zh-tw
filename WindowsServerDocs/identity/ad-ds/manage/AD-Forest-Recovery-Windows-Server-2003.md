---
title: AD 樹系復原-Windows Server 2003 復原
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: bd15df5360a50e417881d83319344dbdf48f35fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829639"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>AD 樹系復原-Windows Server 2003 復原

>適用於：Windows Server 2003

本主題包含執行 Windows Server 2003 的網域控制站 (Dc) 的樹系修復程序。 樹系復原的一般程序並無不同，使用 Windows Server 2003 Dc，但特定程序可能會因為不同的工具不同。 比方說，Ntdsutil.exe 可以用來備份和還原網域控制站執行 Windows Server 2003 Dc，而 Windows Server Backup 或 Wbadmin.exe 是用於執行 Windows Server 2008 的網域控制站或更新版本。  
  
- [備份系統狀態資料](#Backing-up-the-System-State-data)  
- [執行非系統授權還原](#Performing-a-nonauthoritative restore)  
- [安裝和設定 DNS 伺服器服務](#Install-and-configure-the-DNS-Server-service)  

## <a name="backing-up-the-system-state-data"></a>備份系統狀態資料
您可以使用下列程序來備份系統狀態資料，以及您已選取目前的備份作業，執行 Windows Server 2003 DC 的任何其他資料。 Windows Server 2003 包含 Ntbackup 工具，可用來備份系統狀態資料。  
  
中的成員資格**系統管理員**或是**Backup Operators**，或同等權限，以將備份檔案和資料夾所需的最小。   
  
如果您已備份系統狀態資料到磁帶上，備份程式會指出有可用的任何未使用的媒體，您可能必須使用卸除式儲存體。 這樣會加入您的磁帶可用媒體集區，以供備份使用。  
  
您只可以備份系統狀態資料，在本機電腦上。 您無法將它備份在遠端電腦上。  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>若要備份系統狀態資料，網域控制站上執行 Windows Server 2003  
  
1. 按一下 **開始**，指向**所有程式**，指向**附屬應用程式**，指向**系統工具**，然後按一下 **備份**.  
2. 在 **歡迎**頁面上，按一下**進階模式**。  
3. 在 **備份**索引標籤上，選取核取方塊的任何磁碟機、 資料夾或您想要備份的檔案。  
4. 選取 **系統狀態**核取方塊。  
5. 按一下 **開始備份**。  
  
## <a name="performing-a-nonauthoritative-restore"></a>執行非系統授權還原  

您可以使用下列程序來執行的 DC，執行 Windows Server 2003 的非系統授權還原。 藉由執行 Windows Server 2003 中的 Active Directory 上的非系統授權還原，您會自動執行 SYSVOL 的非系統授權還原。 不不需要任何額外的步驟。  
  
> [!NOTE]
> 如果您也要重新安裝 Windows Server 2003 作業系統，您可能會或可能未將電腦加入網域，您可以提供任何名稱給電腦的作業系統安裝期間。 不會安裝 Active Directory。 在之後重新安裝作業系統，直接移至步驟 4。  
  
Windows Server 2003 網域控制站上還原系統狀態資料，您需要也重新安裝任何已在網域控制站執行復原之前的軟體應用程式。 還原網域中的第一個 DC 上的 AD DS 也會還原登錄因為它們都是系統狀態資料的一部分。 如果您有任何這些網域控制站上執行的應用程式，而且必須儲存在登錄中的任何資訊，請記住這點。  
  
若要儲存重新安裝軟體所需的時間，判斷是否必須在網域控制站上安裝的應用程式與虛擬 DC 複製相容。 這類應用程式可以安裝在來源 DC，以節省時間和複製虛擬網域控制站上安裝它們所需的投入時間複製之前。  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>若要執行非系統授權還原
  
1. 啟動 DC 之後，請按 F8 以重新啟動電腦以目錄服務還原模式 (DSRM)。  
2. 選取 **目錄服務還原模式 （Windows 網域控制站只）**。  
3. 選取您想要在還原模式中啟動的作業系統。  
4. （您只能使用本機電腦帳戶，沒有網域登入選項可） 的系統管理員身分登入。  
5. 在命令提示字元中，輸入**ntbackup**，然後按 ENTER 鍵。  
6. 在上**歡迎**頁面上，按一下**進階模式**，然後選取**還原和管理媒體** 索引標籤。(請勿選取**還原精靈**。)  
7. 選取適當的備份檔案還原，然後確認**系統磁碟**並**系統狀態**已選取核取方塊。  
8. 按一下 **啟動還原**。  
9. 還原作業完成時，重新啟動電腦。  
  
使用下列程序來執行 SYSVOL 的授權 （也稱為主要） 還原在執行 Windows Server 2003 網域控制站。 只能在還原網域中第一個 Windows Server 2003 DC 上執行此程序。  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>若要執行 SYSVOL 的系統授權還原  
  
1. 在上一個程序中執行步驟 1 到 8。  
2. 在 [**確認還原**] 對話方塊中，按一下**進階**。  
3. 若要執行 SYSVOL 的授權還原，選取核取方塊**還原複寫的資料集時，將已還原的資料標示為所有複本的主要資料**。  

   > [!NOTE]
   > 將標記在還原的資料，為備份中的主要資料就相當於**BurFlags**到 D4 下列登錄子機碼下的項目：  
   >   
   > **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\\** *GUID*  

4. 還原作業完成時，重新啟動電腦。  
  
## <a name="install-and-configure-the-dns-server-service"></a>安裝和設定 DNS 伺服器服務

如果您從備份還原的 DC 正在執行 Windows Server 2003，您可以安裝 DNS 伺服器不會將 DC 連接至任何網路。  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>安裝和設定 DNS 伺服器服務  
  
1. 開啟 [Windows 元件精靈]。 若要開啟精靈：  

   - 按一下 [開始]、[控制台]，然後按一下 [新增或移除程式]。  
   - 按一下 **新增/移除 Windows 元件**。  

2. 在 **元件**，選取**Networking Services**核取方塊，然後按一下**詳細資料**。  
3. 在 **的網路服務的子元件**，選取**網域名稱系統 (DNS)** 核取方塊，按一下  **確定**，然後按一下 **下一步**。  
4. 如果系統提示您，在**檔案複製來源**，輸入散發檔案的完整路徑，然後按一下**確定**。  

   安裝之後，完成下列步驟來設定 DNS 伺服器。  

5. 按一下 **開始**，指向**所有程式**，指向**系統管理工具**，然後按一下**DNS**。  
6. 嚴重的故障前的 DNS 伺服器上建立相同的 DNS 網域名稱裝載 DNS 區域。 如需詳細資訊，請參閱 < 新增正向對應區域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。  
7. 重要的問題之前就存在，請設定 DNS 資料。 例如:   

   - 設定儲存在 AD DS 的 DNS 區域。 如需詳細資訊，請參閱變更區域類型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。  
   - 設定 DNS 區域的網域控制站定位程式 （DC 定位程式） 資源記錄以允許安全動態更新授權。 如需詳細資訊，請參閱允許只安全動態更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。  

8. 確認父系 DNS 區域包含委派的資源記錄 （名稱伺服器 (NS) 」 和 「 黏附主機 (A) 資源記錄） 此 DNS 伺服器裝載子區域。 如需詳細資訊，請參閱 < 建立區域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。  
9. 設定 DNS 之後，請在命令提示字元中，輸入下列命令，並再按 ENTER 鍵：  

   **net stop netlogon**

10. 輸入下列命令，然後按 ENTER：  

   **net start netlogon&lt**

   > [!NOTE]
   > Net Logon 將 DC 定位程式資源記錄登錄在 DNS 中針對此網域控制站。 如果您要在子網域的伺服器上安裝 DNS 伺服器服務，這個網域控制站將無法立即註冊其記錄。 這是因為它是目前的隔離，因為復原程序和其主要的 DNS 伺服器的一部分是樹系根 DNS 伺服器。 使用相同的 IP 位址，因為它必須以避免 DC 服務查閱失敗的嚴重損壞之前設定此電腦。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原-必要條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂的樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始的復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題集](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-復原 Multidomain 樹系內的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md) 
