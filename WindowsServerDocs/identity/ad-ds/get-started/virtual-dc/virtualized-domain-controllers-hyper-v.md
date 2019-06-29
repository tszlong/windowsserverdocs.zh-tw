---
Title: 使用 HYPER-V 的虛擬化網域控制站
description: 若要讓虛擬化 Windows Server Active Directory 網域控制站在 HYPER-V 中時的考量
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.openlocfilehash: 297c2a26f10503cb68ae241576a72e08aa4e55a0
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469573"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>使用 HYPER-V 的虛擬化網域控制站

> 適用於：Windows Server 2016

本主題將會更新，才能進行本指南適用於 Windows Server 2016。 Windows Server 2012 引進了許多增強功能的虛擬的網域控制站 (Dc)，包括保護機制，可防止虛擬網域控制站上的 USN 復原和複製虛擬網域控制站的能力。 如需有關這些增強功能的詳細資訊，請參閱 < [Active Directory 網域服務 (AD DS) 虛擬化 (等級 100) 簡介](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md)。

HYPER-V 會合併到單一的實體電腦上的不同的伺服器角色。 本指南會說明執行的網域控制站做為 32 位元或 64 位元的客體作業系統。

## <a name="planning-to-virtualize-domain-controllers"></a>規劃虛擬化網域控制站

本節涵蓋適用於 hyper-v server 的硬體需求如何避免單一失敗點，選取適當的 HYPER-V 伺服器和虛擬機器和安全性和效能的決策的組態類型。

## <a name="hyper-v-requirements"></a>HYPER-V 需求

若要安裝並使用 HYPER-V 角色，您必須具備下列項目：

   - **X64 處理器**
      - HYPER-V 是適用於 x64 型版本的 Windows Server 2008 或更新版本。  
   - **硬體輔助虛擬化**
      - 這項功能可在包含虛擬化選項，明確地說，Intel Virtualization Technology (Intel VT) 或 AMD 虛擬化 (AMD-V) 的處理器。  
   - **硬體資料執行防止 (DEP)**
      - 硬體 DEP 必須可用且處於啟用狀態。 具體來說，您必須啟用 Intel XD 位元 （執行停用位元） 或 AMD NX 位元 （無執行位元）。  

## <a name="avoid-creating-single-points-of-failure"></a>避免單一失敗點

您應該嘗試避免建立潛在單一失敗點，當您計劃您的虛擬網域控制站部署。 您可以實作系統重複性以避免潛在單一失敗點。 例如，考慮下列建議時可能會增加管理成本並記住：

1. 執行每個網域的兩個以上的虛擬的網域控制站，不同的虛擬化主機，可降低風險的單一虛擬化主機失敗時，會遺失所有網域控制站上。  
2. 對於其他技術的建議，多樣化 （使用不同的 Cpu、 主機板、 網路介面卡或其他硬體） 的硬體上的網域控制站正在執行。 硬體多樣化限制可能特定廠商設定、 驅動程式，或一筆或類型的硬體故障所造成的損害。  
3. 可能的話，網域控制站應該位於世界各地的不同區域中的硬體上執行。 這有助於減少災害或影響的網域控制站會裝載站台的失敗的影響。  
4. 維護中每個網域實體的網域控制站。 這能減緩會影響使用該平台的所有主機系統虛擬化平台故障的風險。  

## <a name="security-considerations"></a>安全性考量

必須為可寫入的網域控制站，請仔細管理與虛擬網域控制站正在執行的主機電腦，即使該電腦是只在已加入網域或工作群組電腦。 這是一項重要的安全性考量。 管理不善的主應用程式容易遭受提高權限攻擊時，其發生於當惡意使用者獲得存取和未授權或合法指派的系統權限。 惡意使用者可以使用這種攻擊危害的所有虛擬機器，網域和樹系，此電腦的主機。

請務必在規劃虛擬化網域控制站時，請記住下列安全性考量：

   - 裝載虛擬、 可寫入網域控制站被視為等同於認證的所有網域及樹系的網域控制站隸屬於預設網域系統管理員的電腦本機系統管理員。  
   - 建議的設定，以避免發生安全性和效能的問題是執行 Server Core 安裝的 Windows Server 2008 或更新版本中，使用 HYPER-V 以外的任何應用程式的主機。 此設定限制應用程式和服務安裝在伺服器上，這應該會導致更高的效能和較少的應用程式和可能遭惡意利用來攻擊的電腦或網路的服務的數目。 這種組態的效果也稱為減少受攻擊面。 在分公司或其他無法滿意保護的位置中，建議使用唯讀網域控制站 (RODC)。 有個別的管理網路時，我們建議，主應用程式僅能連線至管理網路。  
   - 您可以使用您的網域控制站中的 Bitlocker、 Windows Server 2016 之後您可以使用虛擬的 TPM 功能，也提供客體金鑰資料來解除鎖定系統磁碟區。
   - [受防護網狀架構和受防護的 Vm](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)可以提供額外的控制，以保護您的網域控制站。

Rodc 的相關資訊，請參閱[唯讀網域控制站的規劃與部署指南 》](../../deploy/rodc/read-only-domain-controller-updates.md)。

如需有關如何保護網域控制站的詳細資訊，請參閱 <<c0> [ 保護 Active Directory 安裝的最佳做法指南](../../plan/security-best-practices/best-practices-for-securing-active-directory.md)。

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>不同的主機和客體組態的安全性界限

使用虛擬機器讓您能夠有許多不同的網域控制站組態。 請仔細考慮必須指定界限和信任您的 Active Directory 拓樸中，會影響虛擬機器的方式。 下表說明可能的組態，Active Directory 網域控制站和主機 （HYPER-V 伺服器） 和其來賓電腦 （在 HYPER-V 伺服器上執行的虛擬機器）。

|Machine|設定 1|Configuration 2|
|-------|---------------|---------------|
|主機|工作群組或成員的電腦|工作群組或成員的電腦|
|Guest|網域控制站|工作群組或成員的電腦|

![](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - 在主機電腦上的系統管理員可寫入網域控制站在客體上具有相同的存取權，以網域系統管理員，並必須以此方式處理。 在 RODC 訪客的情況下在主機電腦的系統管理員會將相同的存取權，以本機系統管理員身分對來賓 RODC。   
   - 如果主機已加入相同網域中的網域控制站虛擬機器主機上具有系統管理權限。 沒有惡意使用者危害的所有虛擬機器，如果惡意使用者第一次取得存取權的虛擬機器 1 的機會。 這是所謂的攻擊媒介。 如果有多個網域或樹系的網域控制站，這些網域應該有一個網域的系統管理員信任的所有網域中的集中式的管理。  
   - 從虛擬機器 1 攻擊的機會存在，即使虛擬機器 1 會安裝為 RODC。 雖然 RODC 的系統管理員明確地不具有網域系統管理員權限，RODC 可以用於傳送到主機電腦的原則。 這些原則可能包括啟動指令碼。 如果這項作業成功時，主機電腦遭到入侵，，然後可以用來危害主機電腦上的其他虛擬機器。  

## <a name="security-of-vhd-files"></a>VHD 檔案的安全性

虛擬網域控制站的 VHD 檔案相當於實體硬碟的實體的網域控制站。 此情況下，它應該以相同的數量，進入 設定安全性實體的網域控制站的硬碟機的照護的保護。 請確定只有可靠、 備受信賴的系統管理員可以存取網域控制站的 VHD 檔案。

## <a name="rodcs"></a>RODCs

Rodc 的其中一個優點是能夠將它們放在其中實體安全性無法保證，例如如在分公司位置。 您可以使用 Windows BitLocker 磁碟機加密來保護本身的 VHD 檔案 (不是檔案系統投影) 在主機上透過實體磁碟的竊盜風險受到危害。 

## <a name="performance"></a>效能

使用新的微核心 64 位元架構，有從先前的虛擬化平台的 HYPER-V 效能大幅提升。 為了達到最佳的主應用程式效能，主應用程式應該是 Server Core 安裝的 Windows Server 2008 或更新版本，而且它不應該有安裝 HYPER-V 以外的伺服器角色。

虛擬機器效能，特別是工作負載而定。 若要保證 Active Directory 令人滿意的效能，測試特定的拓撲。 評估目前的工作負載一段時間的可靠性和效能監視器 (Perfmon.msc) 之類的工具或[Microsoft Assessment and Planning (MAP) toolkit](https://go.microsoft.com/fwlink/?linkid=137077)。 對應工具也可以有價值，如果您想要清查的所有伺服器和目前存在於您的網路中的伺服器角色。

若要取得的虛擬的網域控制站效能的一般概念，以下的效能測試已執行[Active Directory 效能測試工具 (ADTest.exe)](https://go.microsoft.com/fwlink/?linkid=137088)。

輕量型目錄存取通訊協定 (LDAP) 測試執行 ADTest.exe 實體的網域控制站，然後按一下 虛擬機器裝載在相同實體的網域控制站的伺服器上。 只有一個邏輯處理器用於實體電腦，並只有一個虛擬處理器用於虛擬機器輕鬆連線到 100 %cpu 使用率。 下表中，每個測試完成之後的括號括住的數字與字母表示 ADTest.exe 中特定的測試。 這項資料顯示，如同虛擬的網域控制站的效能就會是效能的 88 至 98%的實體的網域控制站。

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>度量單位</th>
<th>測試</th>
<th>實體</th>
<th>虛擬</th>
<th>差異</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>搜尋數/秒</p></td>
<td><p>搜尋基底領域 (L1) 中的一般名稱</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10.71%</p></td>
</tr>
<tr class="even">
<td><p>搜尋數/秒</p></td>
<td><p>搜尋基底領域 (L2) 中的屬性集</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11.04%</p></td>
</tr>
<tr class="odd">
<td><p>搜尋數/秒</p></td>
<td><p>搜尋基底領域 (L3) 中的所有屬性</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3.27%</p></td>
</tr>
<tr class="even">
<td><p>搜尋數/秒</p></td>
<td><p>樹狀子目錄範圍 (6) 中的一般名稱的搜尋</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8.23%</p></td>
</tr>
<tr class="odd">
<td><p>成功的繫結/sec</p></td>
<td><p>執行快速的繫結 (B1)</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4.45%</p></td>
</tr>
<tr class="even">
<td><p>成功的繫結/sec</p></td>
<td><p>執行簡單繫結 (B2)</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9.98%</p></td>
</tr>
<tr class="odd">
<td><p>成功的繫結/sec</p></td>
<td><p>使用 NTLM 來執行繫結 (B5)</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1.12%</p></td>
</tr>
<tr class="even">
<td><p>Writes/sec</p></td>
<td><p>撰寫多個屬性 (W2)</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9.00%</p></td>
</tr>
</tbody>
</table>

若要確保令人滿意的效能，允許來賓作業系統，使用 「"enlightenment 或感知 hypervisor 的綜合的驅動程式已安裝整合元件 (IC)。 在安裝過程中，可能必須使用模擬整合驅動電子 (IDE) 或網路介面卡驅動程式。 在生產環境中，您應該將這些模擬驅動程式取代綜合的驅動程式，以提升效能。

當您監視虛擬機器的效能與可靠性和效能管理 (Perfmon.msc) 時，虛擬機器內的 CPU 資訊不會完全精確，因為排定實體處理器的虛擬 CPU 的方式。 當您想要取得之 HYPER-V 伺服器執行的虛擬機器的 CPU 資訊時，請在主應用程式磁碟分割中使用 HYPER-V Hypervisor 邏輯處理器計數器。

如需有關效能調整 AD DS 和 HYPER-V，請參閱[效能微調指導方針的 Windows Server 2016](../../../../administration/performance-tuning/index.md)。

此外，不打算使用差異磁碟 VHD 可能會降低效能，因為設定為網域控制站的虛擬機器上的差異磁碟 VHD。 若要深入了解 HYPER-V 磁碟類型，包括 差異磁碟，請參閱[新的虛擬硬碟精靈](http://go.microsoft.com/fwlink/?linkid=137279)。

如需其他有關虛擬的裝載環境中的 AD DS 的詳細資訊，請參閱[主控件裝載的虛擬環境中的 Active Directory 網域控制站時的考慮事項](https://go.microsoft.com/fwlink/?linkid=141292)Microsoft 知識庫中。

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>虛擬的網域控制站的部署考量

有數個常見的 「 虛擬機器 」 做法，您應該避免當您部署網域控制站，以及時間同步處理和儲存體的特殊考量。

## <a name="virtualization-deployment-practices-to-avoid"></a>若要避免的虛擬化部署作法

HYPER-V 等虛擬化平台，提供方便的功能，可讓管理、 維護、 備份，以及移轉電腦更容易許多。 不過，下列常見的部署做法與功能應不適用於虛擬網域控制站：

- 若要確保 Active Directory 寫入的持久性，不會部署虛擬網域控制站的資料庫檔案 (Active Directory 資料庫 (NTDS。DIT)，記錄檔和 SYSVOL) 虛擬 IDE 磁碟上。 相反地，建立連接至虛擬 SCSI 控制器的第二個 VHD，並確保該資料庫、 記錄和 SYSVOL 期間的放置虛擬機器的 SCSI 磁碟上安裝網域控制站。  
- 請勿在您要設定的網域控制站的虛擬機器上實作差異磁碟的虛擬硬碟 (Vhd)。 這樣很容易就能還原為之前的版本，它也會降低效能。 如需 VHD 類型的詳細資訊，請參閱[新的虛擬硬碟精靈](https://go.microsoft.com/fwlink/?linkid=137279)。  
- 不會部署新的 Active Directory 網域及樹系，在一份已不先備妥使用系統準備工具 (Sysprep) 的 Windows Server 作業系統上。 如需執行 Sysprep 的詳細資訊，請參閱[Sysprep （系統準備） 概觀](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

   > [!WARNING]
   > 不支援在網域控制站上執行 Sysprep。

- 為了協助防止潛在的更新序列號碼 (USN) 復原情況，請勿使用代表已部署的網域控制站，來部署其他網域控制站的 VHD 檔案的複本。 如需有關 USN 復原的詳細資訊，請參閱 < [USN 和 USN 回復](#usn-and-usn-rollback)。
   - Windows Server 2012 和更新版本，可讓 「 複製網域控制站的映像，如果他們想要部署其他網域控制站時，請準備正確的系統管理員
- 請勿使用 HYPER-V 匯出 」 功能來匯出虛擬機器執行的網域控制站。
  - Windows Server 2012 及更新版本，匯出和匯入的網域控制站的虛擬客體被處理像在非權威還原因為偵測到世代識別碼變更，而且未針對複製。
  - 請確定您不使用客體再匯出。
    - 您可以使用 HYPER-V 複寫將網域控制站的第二個非使用中複本。 如果您啟動複寫的映像，您也需要執行適當的清除，基於相同理由，做為匯出的 DC 來賓映像之後，未使用的來源。

## <a name="physical-to-virtual-migration"></a>實體對虛擬的移轉

System Center Virtual Machine Manager (VMM) 2008年提供統一的管理實體機器和虛擬機器。 它也提供讓您將實體機器移轉至虛擬機器。 此程序稱為實體至虛擬機器轉換 （P2V 轉換）。 在 P2V 轉換過程中，新的虛擬機器和實體的網域控制站要移轉不能執行在此同時，若要避免 USN 復原情況下，如中所述[USN 和 USN 回復](#usn-and-usn-rollback)。

您應該執行 P2V 轉換，使網域控制站重新開啟時，目錄資料能夠一致使用離線模式。 離線模式的選項是提供，也建議在轉換實體伺服器精靈。 如需線上模式和離線模式之間差異的說明，請參閱[P2V:將實體電腦轉換成 VMM 中的虛擬機器](https://go.microsoft.com/fwlink/?linkid=155072)。 進行 P2V 轉換時，虛擬機器不應該連線到網路。 P2V 轉換程序已完成且已驗證時，才應該啟用虛擬機器的網路介面卡。 此時，實體來源機器將會關閉。 重新格式化硬碟之前請勿再讓回網路的實體來源機器。

> [!NOTE]
> 有更安全的選項，以建立新虛擬網域控制站，不會執行建立 USN 回復的風險。 如果您已經有至少一個虛擬的網域控制站，您可能會設定新的虛擬 DC 依定期的促銷，從安裝媒體 (IfM)，同時也使用 網域控制站複製，從提升。
這也有助於避免發生硬體問題，或可能會遇到與平台相關的問題 P2V 轉換的虛擬客體。

> [!WARNING]
> 若要防止 Active Directory 複寫問題，請確定該只有一個執行個體 （實體或虛擬） 指定網域控制站存在指定的網路，在任何時間點上的時間。
> 您可以降低舊複製品正在發生問題的可能性：
> 
> - 當執行新的虛擬 DC 時，變更兩次使用電腦帳戶密碼： netdom resetpwd /Server: < 網域控制站 >...
> - 匯出和匯入新的虛擬客體，以強制其成為新的世代識別碼，因此資料庫引動過程識別碼。
> 

## <a name="using-p2v-migration-to-create-test-environments"></a>若要建立測試環境中使用 P2V 移轉

若要建立測試環境，您可以使用透過 VMM 的 P2V 移轉。 若要建立測試環境，而不需要永久關閉實際執行網域控制站的虛擬機器，您可以從實體機器移轉生產網域控制站。 不過，測試環境必須是從生產環境的不同網路上兩個相同的網域控制站執行個體是否存在。 十分小心必須納入 P2V 移轉的測試環境的建立，以避免可能會影響您的測試和生產環境的 USN 復原。 以下是您可以使用 p2v 建立測試環境的方法。

在實際執行網域控制站從每個網域移轉至測試虛擬機器使用 P2V 根據實體對虛擬的 「 移轉 」 一節所述的指導方針。 實體的實際執行機器和測試虛擬機器必須位於不同的網路時重新上線。 若要避免 USN 復原在測試環境中的，會從實體機器移轉到虛擬機器的所有網域控制站必須都離線。 （您可以停止 ntds 服務，或在目錄服務還原模式 (DSRM) 重新啟動電腦。）網域控制站離線之後，應該不引進任何新的更新環境。 P2V 移轉; 期間，電腦必須保持離線狀態沒有任何電腦應該會重新上線之前完全移轉的所有電腦。 若要深入了解 USN 復原，請參閱 USN 和 USN 回復。

後續的測試網域控制站應該升級為測試環境中的複本。

## <a name="time-service"></a>時間服務

對於設定為網域控制站的虛擬機器，建議您停用主機系統與客體作業系統做為網域控制站之間的時間同步處理。 這可讓客體網域控制站同步處理時間與網域階層。

若要停用 HYPER-V 時間同步處理提供者，請關閉 VM 並清除 [Integration Services 底下的時間同步處理] 核取方塊。

> [!NOTE]
> 本指南最近已更新以反映目前的建議，同步處理只網域階層中，從來賓網域控制站的時間，而不是部分停用主機之間的時間同步處理的上一個建議系統與客體的網域控制站。

## <a name="storage"></a>儲存體

若要將網域控制站虛擬機器的效能最佳化，並確保 Active Directory 寫入的持久性，使用下列建議來儲存作業系統、 Active Directory，以及 VHD 檔案：

- **客體儲存體**。 儲存 Active Directory 資料庫檔案 (Ntds.dit)、 記錄檔和 SYSVOL 檔案從作業系統檔案的個別虛擬磁碟上。 建立連接至虛擬 SCSI 控制器的第二個 VHD，並儲存虛擬機器的虛擬 SCSI 磁碟上的資料庫、 記錄和 SYSVOL。 虛擬 SCSI 磁碟提供更高的效能相較於虛擬 IDE，並支援強制 Unit Access (FUA)。 FUA 可確保作業系統寫入和讀取資料直接從媒體安裝略過所有快取機制。

  > [!NOTE]
  > 如果您打算要用於虛擬 DC 客體中的 Bitlocker，您需要確定其他磁碟區都已設定為 「 自動解除鎖定 」。
  > 詳細資訊設定自動解除鎖定可在[啟用 BitLockerAutoUnlock](https://docs.microsoft.com/powershell/module/bitlocker/enable-bitlockerautounlock)

- **裝載的 VHD 檔案的儲存體**。 建議：裝載 VHD 檔案的儲存體建議位址儲存的體。 為達最佳效能，不要儲存磁碟是經常使用的其他服務或應用程式，例如主機 Windows 作業系統安裝所在的系統磁碟上的 VHD 檔案。 儲存在不同的磁碟分割，從主機作業系統上的每個 VHD 檔案和任何其他 VHD 檔案。 理想的設定是儲存在個別的實體磁碟機上的每個 VHD 檔案。  

  主機的實體磁碟系統也必須滿足**至少一個**的下列準則，以符合需求的虛擬化工作負載資料完整性：  

   - 系統會使用伺服器類別磁碟 （SCSI、 光纖通道）。  
   - 系統可確保磁碟已連接到電池供電的快取主機匯流排介面卡 (HBA)。  
   - 系統會使用儲存體控制器 （例如 RAID 系統），做為儲存體裝置。  
   - 系統可確保磁碟能夠受到不斷電供應系統 (UPS)。  
   - 系統可確保磁碟寫入快取功能已停用。  

- **傳遞磁碟與固定 VHD**。 有許多方式來設定虛擬機器儲存體。 VHD 檔案使用時，因為在建立時配置的固定大小 Vhd 記憶體固定大小 Vhd 是動態 Vhd 比更有效率。 傳遞磁碟，虛擬機器可用來存取實體的儲存媒體，更會針對效能最佳化。 傳遞磁碟基本上是實體磁碟或連接至虛擬機器的邏輯單元編號 (Lun)。 傳遞磁碟不支援快照集功能。 因此，傳遞磁碟都是慣用的硬碟設定，因為不建議使用具有網域控制站的快照集。  

若要減少 Active Directory 資料損毀的可能性，請使用虛擬 SCSI 控制器：

   - 使用裝載虛擬網域控制站的 HYPER-V 伺服器上的 SCSI （而不是 IDE ATA/磁碟機） 的實體磁碟機。 如果您無法使用 SCSI 磁碟機，請確定裝載虛擬網域控制站的 IDE ATA/磁碟機上，停用寫入快取。 如需詳細資訊，請參閱 <<c0> [ 事件識別碼 1539 – 資料庫完整性](https://go.microsoft.com/fwlink/?linkid=162419)。
   - 若要確保持久性 Active Directory 的寫入，Active Directory 資料庫、 記錄和 SYSVOL 必須放在虛擬 SCSI 磁碟上。 虛擬 SCSI 磁碟支援強制 Unit Access (FUA)。 FUA 可確保作業系統寫入和讀取資料直接從媒體安裝略過所有快取機制。  

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>虛擬的網域控制站的操作考量

在虛擬機器執行的網域控制站具有操作不會套用至實體機器執行的網域控制站的限制。 當您使用虛擬的網域控制站時，有一些虛擬化軟體功能和您不應該使用的做法：

   - 執行不暫停、 停止或存放虛擬機器中的網域控制站的儲存的狀態，超過標記存留期的樹系的時間週期，然後從儲存或暫停狀態恢復。 如此一來可能會妨礙複寫。 若要了解如何判斷樹系的標記存留時間，請參閱[判斷樹系的標記存留期](https://go.microsoft.com/fwlink/?linkid=137177)。  
   - 請勿複製或複製虛擬硬碟 (Vhd)。 即使使用就地客體 VM 的防護措施，個別 Vhd 仍然會複製，並造成 USN 復原。
   - 不需要或不使用虛擬網域控制站的快照集。 技術支援與 Windows Server 2012 和更新版本中，它不是個不錯的備份策略的取代項目。 有幾個原因建立 DC 的快照集，或還原的快照集。
   - 請勿設定為網域控制站的虛擬機器上使用差異磁碟 VHD。 這可讓還原為先前的版本太容易，並也會降低效能。  
   - 請勿使用 「 匯出 」 功能執行的網域控制站的虛擬機器上。  
   - 不要還原網域控制站，或嘗試任何以外的方式使用支援的備份復原 Active Directory 資料庫的內容。 如需詳細資訊，請參閱 <<c0> [ 備份和還原虛擬網域控制站的考量](#backup-and-restore-practices-to-avoid)。  

所有這些建議會對協助避免更新序列號碼 (USN) 復原的可能性。 如需有關 USN 復原的詳細資訊，請參閱 USN 和 USN 回復。

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>備份和還原虛擬的網域控制站的考量事項

備份網域控制站是針對任何環境的重要需求。 備份會發生網域控制站失敗或管理錯誤時防止資料遺失。 如果發生這種情況下，就必須復原的網域控制站系統狀態點的時間之前的失敗或錯誤。 狀況良好的狀態還原網域控制站的支援的方法是網域的使用 Active Directory 相容備份應用程式，例如 Windows Server Backup，還原系統狀態備份產生從目前安裝控制站。 如需與 Active Directory 網域服務 (AD DS) 中使用 Windows Server Backup 的詳細資訊，請參閱[AD DS 備份與復原逐步指南 》](https://go.microsoft.com/fwlink/?LinkId=138501)。

虛擬機器的技術，利用 Active Directory 的特定需求會還原作業的時間，在新增的重要性。 比方說，您可以使用虛擬硬碟 (VHD) 檔案的複本，以還原網域控制站，如果您略過更新之後已還原的資料庫版本的網域控制站的重要步驟。 複寫會繼續進行不適當的追蹤號碼，導致網域控制站複本之間不一致的資料庫。 在大部分情況下，此問題會偵測到複寫系統，並不回報任何錯誤，即使網域控制站之間的不一致。

還有一個支援的方法，來執行備份和還原虛擬的網域控制站：

1. 在客體作業系統中執行 Windows Server Backup。  

與 Windows Server 2012 和更新版本的 HYPER-V 主機和來賓，您可能需要支援的備份，使用快照集、 客體 VM 匯出和匯入以及 Hyper-v VM 複寫的網域控制站。 所有這些不過並不適合用於建立適當的備份歷程記錄，與客體 VM 匯出其例外。

與 Windows Server 2016 HYPER-V 的 「 實際執行快照集 」 支援 HYPER-V 伺服器就會觸發客體的 VSS 為基礎的備份和來賓完成的快照集時，主應用程式會擷取的 Vhd，並將它們儲存在備份的位置。

雖然這適用於 Windows Server 2012 和更新版本中，沒有與 Bitlocker 不相容：

- 當執行 VSS 貼齊螢幕擷取，AD 想要執行後快照集的工作，若要將資料庫標示為即將推出，從備份，或如果是準備 RODC 的 IFM 來源，請從資料庫移除認證。
- HYPER-V 會掛接快照磁碟區，這項工作，則會解除鎖定磁碟區未加密存取任何設備。 讓 AD 資料庫引擎無法存取資料庫，並最終會失敗，快照集。

> [!NOTE]
> 受防護的 VM 專案提到先前已導向備份為非為目標的客體 VM 的最大的資料保護的 HYPER-V 主機。

## <a name="backup-and-restore-practices-to-avoid"></a>備份與還原若要避免的作法

如所述，在虛擬機器中執行的網域控制站會有限制，不會套用至實體機器中執行的網域控制站。 當您備份或還原虛擬網域控制站時，有特定的虛擬化軟體功能和您不應該使用的做法：

   - 請勿複製或複製的網域控制站，而不是執行定期備份的 VHD 檔案。 如果他 VHD 檔案會複製或再製，它會變成過時。 然後，如果 VHD 以正常模式啟動，就會發生 USN 復原。 您應該執行適當的備份作業所支援的 Active Directory 網域服務 (AD DS)，例如使用 Windows Server Backup 功能。  
   - 請勿還原已設定為網域控制站的虛擬機器中使用做為備份快照集功能。 當您將虛擬機器還原成先前的狀態與 Windows Server 2008 R2 和較舊版本時，複寫將會發生問題。 如需詳細資訊，請參閱 < [USN 和 USN 回復](#usn-and-usn-rollback)。 雖然使用快照集還原唯讀網域控制站 (RODC) 並不會導致複寫問題，但仍不建議這種還原方法。  

## <a name="restoring-a-virtual-domain-controller"></a>還原虛擬網域控制站

若要還原的網域控制站，而失敗時，您必須定期備份系統狀態。 系統狀態包含 Active Directory 資料和記錄檔、 登錄、 系統磁碟區 （SYSVOL 資料夾） 和作業系統的各種項目。 這項需求是相同的實體和虛擬網域控制站。 系統狀態還原程序執行 Active Directory 相容的備份應用程式，專為還原程序，包括要複寫的通知之後確保本機和複寫 Active Directory 資料庫的一致性引動過程識別碼重設的合作夥伴。 不過，使用裝載的虛擬環境和磁碟或作業系統映像建立的應用程式可讓系統管理員能夠略過檢查，而且還原網域控制站系統狀態時，通常發生的驗證。

當網域控制站虛擬機器失敗，而且尚未發生更新序列號碼 (USN) 復原，有兩種支援的情況，來還原虛擬機器：

   - 如果存在有效的系統狀態資料備份之前失敗，您可以使用您用來建立備份的備份公用程式的還原選項來還原系統狀態。 系統狀態資料備份必須已建立的標記存留時間，也就是根據預設，不能超過 180 天的範圍內使用 Active Directory 相容的備份公用程式。 您應該備份您的網域控制站至少每隔半標記存留期。 如需有關如何判斷特定的標記存留期，您的樹系中的指示，請參閱[判斷樹系的標記存留期](https://go.microsoft.com/fwlink/?linkid=137177)。  
   - 如果 VHD 檔案的工作複本可用，但沒有系統狀態備份可供使用，您可以移除現有的虛擬機器。 將現有的虛擬機器還原使用先前複製的 VHD，但務必先在目錄服務還原模式 (DSRM) 啟動，並在下一節中設定正確，如所述登錄。 以標準模式，然後重新啟動網域控制站。

使用在下圖中的程序，來判斷要還原您的虛擬的網域控制站的最佳方式。

![](media/virtualized-domain-controller-architecture/Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

在 rodc 方面，還原程序和決策會更簡單。

![](media/virtualized-domain-controller-architecture/Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>還原虛擬網域控制站系統狀態備份

如果網域控制站虛擬機器有有效的系統狀態備份，您可以安全地將備份還原遵循備份所規定的還原程序的工具，您用來備份 VHD 檔案。

> [!IMPORTANT]
> 若要正確地還原網域控制站，您必須以 DSRM 啟動它。 您必須允許以正常模式啟動的網域控制站。 如果您錯過機會輸入 DSRM 系統啟動時，關閉網域控制站的虛擬機器，才能完全以標準模式開始。 請務必啟動網域控制站以 DSRM 啟動網域控制站，以正常模式遞增其 Usn，因為即使與網路中斷連線的網域控制站。 如需有關 USN 復原的詳細資訊，請參閱 USN 和 USN 回復。 

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>若要還原虛擬網域控制站系統狀態備份

1. 啟動網域控制站的虛擬機器，並按下 f5 鍵來存取 Windows 開機管理程式 畫面。 如果您需要輸入連接的認證，立即按一下**暫停**按鈕在虛擬機器上，使它不會繼續啟動。 然後，輸入您的連接認證，然後按一下 **播放**虛擬機器上的按鈕。 在 [虛擬機器] 視窗中，按一下，然後按 F5。

   如果您看不見 [Windows 開機管理程式] 畫面，而且網域控制站會開始以正常模式啟動，請關閉虛擬機器，以防止它完成啟動。 重複此步驟為視需要多次前，都能夠存取 Windows 開機管理程式 畫面。 您無法從 [Windows 錯誤復原] 功能表存取 DSRM。 因此，關閉虛擬機器，然後重試，如果出現 [Windows 錯誤復原] 功能表。

2. 在 Windows 開機管理程式 畫面中，請按 F8 以存取進階的開機選項。
3. 在 **進階開機選項**畫面上，選取**目錄服務還原模式**，然後按 ENTER 鍵。 這會以 DSRM 啟動網域控制站。
4. 您用來建立系統狀態備份的工具，使用適當的還原方法。 如果您使用 Windows Server Backup，請參閱[執行 AD ds 非系統授權還原](https://go.microsoft.com/fwlink/?linkid=132637)。

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>無法使用適當的系統狀態資料的備份時，還原虛擬網域控制站

如果您沒有比虛擬機器失敗的系統狀態資料的備份，您可以使用先前的 VHD 檔案來還原虛擬機器執行的網域控制站。 如果可以，您可以建立一份 VHD，因此，如果您在程序期間發生的問題，或遺漏的步驟，您可以再試一次複製的 VHD。

> [!IMPORTANT]
> - 您不應該考慮使用下列程序來取代，定期計劃與排程的備份。
> - **使用下列程序，來執行的還原不會受到 Microsoft 並沒有其他替代方案時，才應該使用。**
> - 如果您要還原之 vhd 的複本已啟動以標準模式的任何虛擬機器，請不要使用此程序。

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>若要還原舊版虛擬網域控制站的 VHD，而不需要系統狀態資料備份

1. 使用先前的 VHD，dsrm，啟動虛擬網域控制站，如上一節所述。 不允許以正常模式啟動的網域控制站。 如果您遺失 Windows 開機管理程式 畫面，網域控制站會開始以正常模式啟動，請關閉虛擬機器，以防止它完成啟動。 請參閱上的一節，如需詳細指示，輸入 DSRM。
2. 開啟「登錄編輯程式」。 若要開啟 登錄編輯程式，請按一下**開始**，按一下**執行**，型別**regedit**，然後按一下 確定。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。 在登錄編輯器中，展開下列路徑：**HKEY\_本機\_MACHINE\\系統\\CurrentControlSet\\Services\\NTDS\\參數**。 尋找名為的值**DSA 上一個還原計數**。 如果值存在時，請記下設定。 如果值不存在，就有一個設定相當於預設值，也就是零。 如果看不到其中一個發生，請不要新增的值。
3. 以滑鼠右鍵按一下**參數**鍵，按一下**新增**，然後按一下**DWORD （32 位元） 值**。
4. 輸入新名稱**從備份還原資料庫**，然後按 ENTER 鍵。
5. 按兩下您剛才建立開啟的值**編輯 DWORD （32 位元） 值** 對話方塊中，然後按**1**中**數值資料** 方塊中。 **備份項目從還原的資料庫**在執行 Windows 2000 Server Service Pack 4 (SP4)，隨附於更新的 Windows Server 2003 的網域控制站會提供選項[如何偵測從 Windows Server 2003、 Windows Server 2008 和 Windows Server 2008 R2 中的 USN 回復和復原](https://go.microsoft.com/fwlink/?linkid=137182)中安裝的 Microsoft 知識庫和 Windows Server 2008。
6. 正常模式重新啟動網域控制站。
7. 網域控制站重新啟動時，請開啟 事件檢視器。 若要開啟 [事件檢視器]，請按一下 [開始]  ，再按一下 [控制台]  ，連按兩下 [系統管理工具]  ，再連按兩下 [事件檢視器]  。
8. 依序展開**Application and Services Logs**，然後按一下**Directory Services**記錄檔。 請確定事件會出現在 [詳細資料] 窗格中。
9. 以滑鼠右鍵按一下**Directory Services**記錄，然後**尋找**。 在**Find what**，型別**1109年**，然後按一下 **尋找下一個**。
10. 您應該會看到至少一個事件識別碼 1109年項目。 如果看不到此項目，請繼續下一個步驟。 否則，按兩下項目，然後檢閱 確認更新已對 InvocationID 的文字：

    ```
    Active Directory has been restored from backup media, or has been configured to host an application partition. 
    The invocationID attribute for this directory server has been changed. 
    The highest update sequence number at the time the backup was created is <time>

    InvocationID attribute (old value):<Previous InvocationID value>
    InvocationID attribute (new value):<New InvocationID value>
    Update sequence number:<USN>

    The InvocationID is changed when a directory server is restored from backup media or is configured to host a writeable application directory partition.
    ```

11. 關閉 [事件檢視器]。
12. 使用登錄編輯程式來確認中的值**DSA 上一個還原計數**相當於先前的值加一。 如果這不是正確的值，而且您無法在事件檢視器中的事件識別碼 1109年到的項目，確認目前的網域控制站的服務組件。 您無法再試一次在相同的 VHD 上的這個程序。 您可以再試一次將 VHD 或尚未啟動以正常模式啟動透過步驟 1 的不同 VHD 的複本上。
13. 結束登錄編輯程式。

## <a name="usn-and-usn-rollback"></a>USN 和 USN 回復

本章節描述可能是因為不正確的 Active Directory 資料庫還原較舊版本的虛擬機器的複寫問題。 如需 Active Directory 複寫程序的詳細資訊，請參閱[Active Directory 複寫概念](../replication/active-directory-replication-concepts.md)

## <a name="usns"></a>Usn

Active Directory 網域服務 (AD DS) 會使用更新序列號碼 (Usn) 來追蹤複寫的網域控制站之間的資料。 變更資料在目錄中，每次的 USN 遞增，以表示已按下變更。

目的地網域控制站儲存每個目錄磁碟分割，Usn 用來追蹤的最新來源的更新，網域控制站導入它的資料庫，以及儲存複本的每個其他網域控制站的狀態目錄磁碟分割。 當網域控制站會將變更複寫到另一個時，它們會查詢大於最後一個變更，從每個交易夥伴接收的網域控制站的 USN 的 usn 變更其複寫協力電腦。

下列兩個複寫中繼資料資料表包含 Usn。 來源和目的地網域控制站會使用它們來篩選目的地網域控制站所需要的更新。

1. **最新狀態向量**:目的地網域控制站會維護追蹤所有的來源網域控制站接收的原始更新資料表。 目的地網域控制站要求變更時的目錄磁碟分割，它會提供其來源網域控制站的最新狀態向量。 然後，來源網域控制站會使用這個值來篩選的更新，它會傳送至目的地網域控制站。 來源網域控制站傳送至目的地成功複寫週期完成其最新狀態向量，以確保目的地網域控制站，知道它有與每個網域控制站的同步處理原始的更新和更新位於與來源相同的層級。  
2. **高水位**:目的地網域控制站維護來追蹤它已收到來自特定資料分割的特定來源網域控制站的最新變更的值。 上限可防止來源網域控制站傳送的目的地網域控制站已從其接收的變更。  

## <a name="directory-database-identity"></a>目錄資料庫身分識別

除了 Usn，網域控制站記錄的目錄資料庫的來源複寫協力電腦。 目錄資料庫伺服器上執行的身分識別與伺服器物件本身的身分識別會分開維護的。 每個網域控制站上的目錄資料庫識別會儲存在**invocationID** NTDS 設定物件，位於下列的輕量型目錄存取通訊協定 (LDAP) 路徑下的屬性： cn = NTDS設定]、 [cn = ServerName，cn = Servers，cn =*SiteName*，cn = Sites，cn = Configuration，dc =*ForestRootDomain*。 伺服器物件身分識別會儲存在**objectGUID** NTDS 設定物件的屬性。 伺服器物件的識別永遠不會變更。 不過，目錄資料庫變更的身分識別的伺服器上的系統狀態還原程序發生時應用程式目錄分割新增時，然後移除，或稍後從伺服器重新加入。 (其他案例： HyperV 的執行個體就會觸發其包含虛擬 DC 的 VHD 的磁碟分割上的 VSS 寫入器，當客體接著會觸發它自己的 VSS 寫入器 （使用備份/還原上述的相同機制） 導致 invocationID 是另一個方法重設）

因此， **invocationID**有效地與一組的來源目錄資料庫的特定版本的網域控制站上的更新。 最新狀態向量和上限標準標記資料表使用**invocationID**和 DC 的 GUID 分別讓的網域控制站知道從 Active Directory 的複本資料庫的複寫資訊時，才會推出。

**InvocationID**是全域唯一識別碼 (GUID) 值，可看見最上方的輸出之後執行命令**repadmin /showrepl**。 下列文字表示的命令範例輸出：

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

當網域控制站上正確地還原 AD DS **invocationID**會重設。 因為這項變更，您會遇到增加的複寫流量 – 都是相對於正在複寫的磁碟分割大小

例如，假設 VDC1 和 DC2 都是相同的網域中的兩個網域控制站。 在適當的還原的情況下重設 invocationID 值時下, 圖會顯示有關 VDC1 DC2 的認知。

![](media/virtualized-domain-controller-architecture/Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>USN 復原

Usn 正常更新會規避和網域控制站便會嘗試使用 USN 小於其最新的更新時，就會發生 USN 復原。 會偵測到 USN 回復，並先建立樹系中的分歧時，在大部分情況下，將會停止複寫。 

USN 復原可能會造成在許多方面，比方說，當使用舊的虛擬硬碟 (VHD) 檔案，或如果不確定該實體機器會維持離線永久轉換後執行實體對虛擬轉換 （P2V 轉換）。 採取下列預防措施以確保不會不會發生 USN 復原：

   - 未執行時 Windows Server 2012 或更新版本，不需要或不使用網域控制站虛擬機器的快照集。
   - 請勿複製網域控制站的 VHD 檔案。  
   - 當並未執行 Windows Server 2012 或更新版本中，不要匯出虛擬機器執行的網域控制站。  
   - 不要還原網域控制站，或嘗試任何其他方法比受支援的備份解決方案，例如 Windows Server Backup 復原 Active Directory 資料庫的內容。  

在某些情況下，可能會偵測到 USN 復原。 在其他情況下，它可能會導致其他的複寫錯誤。 在這些情況下，就必須找出問題的範圍，並及時處理它。 如需有關如何移除可能會造成 USN 復原的延遲物件的資訊，請參閱[過期的 Active Directory 物件在 Windows Server 2003 中產生事件識別碼 1988年](https://go.microsoft.com/fwlink/?linkid=137185)Microsoft 知識庫中。

## <a name="usn-rollback-detection"></a>USN 回復偵測

在大部分情況下，沒有相對應的 USN 復原重設**invocationID**因不正確的還原程序偵測到。 不正確的網域控制站還原作業之後，Windows Server 2008 會提供保護以防範不適當的複寫。 這些保護會觸發，不正確的還原作業會導致較低的 Usn，代表來自變更的複寫夥伴已收到的事實。

在 Windows Server 2008 和 Windows Server 2003 SP1 中，使用先前使用過的 USN，在目的地網域控制站要求變更時由其來源複寫協力電腦的回應來解譯目的地網域控制站，以表示其複寫中繼資料已過時。 這表示，Active Directory 資料庫來源網域控制站上的已復原回先前的狀態。 比方說，虛擬機器的 VHD 檔案具有已回復為先前的版本。 在此情況下，目的地網域控制站會起始下列 「 隔離 」 量值，已被識別為具有經歷不正確的還原網域控制站上：

   - AD DS 會暫停 Net Logon 服務，以防止使用者帳戶和電腦帳戶，變更帳戶密碼。 如果它們發生不正確的還原完成後，此動作可防止這類變更遺失。  
   - AD DS 會停用輸入和輸出的 Active Directory 複寫。  
   - AD DS 會產生事件識別碼 2095年目錄服務事件記錄檔，以指出條件中。  

下圖顯示當 VDC2，虛擬機器執行的目的地網域控制站偵測到 USN 回復時，就會發生事件的順序。 在此圖中，偵測到 USN 回復，就會發生在 VDC2 上複寫協力電腦偵測到 VDC2 傳送目的地網域控制站，這表示 VDC2 的資料庫已還原回看到最新 USN 值時時間不正確。

![](media/virtualized-domain-controller-architecture/Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

如果目錄服務事件記錄檔會報告事件識別碼 2095年，請立即完成下列程序。

## <a name="to-resolve-event-id2095"></a>若要解決事件識別碼 2095

1. 隔離之虛擬機器的記錄從網路錯誤。
2. 嘗試判斷是否源自此網域控制站的任何變更，並傳播到其他網域控制站。 如果事件已快照集的結果或正在啟動虛擬機器的複本，請試著判斷發生 USN 復原的時間。 然後，您可以檢查該網域控制站的複寫協力電腦，以判斷是否複寫就會發生在那之後。

   您可以使用 Repadmin 工具來進行這項決定。 如需如何使用 Repadmin 資訊，請參閱[監視和疑難排解 Active Directory 複寫使用 Repadmin](https://go.microsoft.com/fwlink/?linkid=122830)。 如果您不能自行決定，請連絡[Microsoft 支援服務](https://support.microsoft.com)尋求協助。

3. 強制降級網域控制站。 這牽涉到網域控制站的中繼資料清理和拿取操作主機 （也稱為彈性單一主機操作或 FSMO） 角色。 如需詳細資訊，請參閱 「 從 USN 復原正在復原 」 一節[如何偵測和復原從 USN 復原在 Windows Server 2003、 Windows Server 2008 和 Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) Microsoft 知識庫中。
4. 刪除所有先前的網域控制站的 VHD 檔案。

## <a name="undetected-usn-rollback"></a>未偵測到的 USN 復原

USN 復原可能無法偵測在兩種情況的其中一個：

1. 將 VHD 檔案附加至不同的虛擬機器，同時執行多個位置中。  
2. 過去的其他網域控制站已收到的最後一個 USN，增加還原的網域控制站上的 USN。  

在第一次的情況下，可能會使用任何虛擬機器，複寫其他網域控制站，並變更可能會在發生可能是虛擬機器而不需要複寫到另。 此樹系的分歧性很難偵測，並會導致無法預期的目錄回應。 P2V 移轉後可能會發生這種情況，如果在相同網路上執行的實體 」 和 「 虛擬機器。 這也可能會發生如果多個虛擬網域控制站處於建立從相同的實體的網域控制站，然後再執行相同的網路上。

在第二個的情況下，範圍的 Usn 適用於兩組不同的變更。 這個過程可以偵測不到持續很長。 每次修改，在這段期間所建立物件時，會偵測到延遲物件，並回報為 事件檢視器中的事件識別碼 1988年。 下圖顯示如何 USN 復原可能無法偵測在這種情況下。

![](media/virtualized-domain-controller-architecture/Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>唯讀網域控制站

Rodc 會裝載 Active Directory 資料庫中的分割區的唯讀副本的網域控制站。 Rodc 會避免大部分的 USN 回復問題，因為它們不會複寫任何變更的其他網域控制站。 不過，如果 RODC 複寫受到 USN 復原的可寫入的網域控制站，也會影響 RODC。

建議您不要還原 RODC 使用快照集。 還原 RODC 使用的 Active Directory 相容的備份應用程式。 此外，與可寫入網域控制站，必須小心為不允許離線超過標記存留期的 RODC。 這種狀況可能會導致在 RODC 上的延遲物件。

如需 Rodc 的詳細資訊，請參閱[唯讀網域控制站的規劃與部署指南 》](../../deploy/rodc/read-only-domain-controller-updates.md)。
