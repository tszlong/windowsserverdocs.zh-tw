---
title: 使用 Hyper-v 虛擬化網域控制站
description: 在 Hyper-v 中將 Windows Server Active Directory 網網域控制站虛擬化時的考慮
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: 209e87b90656555062d9f7e343beedb0143c1df2
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949116"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>使用 Hyper-v 虛擬化網域控制站

> 適用於：Windows Server 2016

本主題將會更新，以便將指引適用于 Windows Server 2016。 Windows Server 2012 引進許多虛擬網域控制站（Dc）的增強功能，包括防止虛擬 Dc 上的 USN 復原以及複製虛擬 Dc 的能力。 如需這些改良功能的詳細資訊，請參閱[Active Directory Domain Services （AD DS）虛擬化的簡介（層級100）](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md)。

Hyper-v 會將不同的伺服器角色合併到單一實體電腦上。 本指南說明如何以32位或64位的客體作業系統身分執行網域控制站。

## <a name="planning-to-virtualize-domain-controllers"></a>規劃虛擬化網域控制站

本節涵蓋 Hyper-v 伺服器的硬體需求、如何避免單一失敗點、為您的 Hyper-v 伺服器和虛擬機器選取適當的設定類型，以及安全性和效能決策。

## <a name="hyper-v-requirements"></a>Hyper-V 需求

若要安裝和使用 Hyper-v 角色，您必須具備下列各項：

   - **X64 處理器**
      - Hyper-v 可在 x64 版本的 Windows Server 2008 或更新版本中使用。  
   - **硬體輔助虛擬化**
      - 這項功能適用于包含虛擬化選項的處理器，特別是 Intel 虛擬化技術（Intel VT）或 AMD 虛擬化（AMD-V）。  
   - **硬體資料執行防止（DEP）**
      - 硬體 DEP 必須可用且已啟用。 具體而言，您必須啟用 Intel XD 位（執行停用位）或 AMD NX 位（無執行位）。  

## <a name="avoid-creating-single-points-of-failure"></a>避免建立單一失敗點

當您規劃虛擬網域控制站部署時，應該嘗試避免建立潛在的單一失敗點。 您可以藉由執行系統冗余來避免引進潛在的單一失敗點。 例如，請考慮下列建議，同時記住可能會增加系統管理成本：

1. 在不同的虛擬化主機上，每個網域至少執行兩個虛擬網域控制站，如果單一虛擬化主機失敗，則會降低遺失所有網域控制站的風險。  
2. 如需其他技術的建議，請 多樣化網域控制站執行所在的硬體（使用不同的 Cpu、主機板、網路介面卡或其他硬體）。 硬體多樣化會限制可能因廠商設定、驅動程式或單一或硬體類型的特定問題而造成的損害。  
3. 可能的話，網域控制站應該在位於世界各地不同區域的硬體上執行。 這有助於降低嚴重損壞或失敗的影響，這會影響裝載網域控制站的網站。  
4. 維護每個網域中的實體網域控制站。 這可降低虛擬化平臺故障的風險，而影響到使用該平臺的所有主機系統。  

## <a name="security-considerations"></a>安全性考量

執行虛擬網域控制站的主機電腦必須謹慎地管理為可寫入的網域控制站，即使該電腦只是已加入網域或工作組的電腦。 這是重要的安全性考慮。 管理不善主機容易遭受權限提高攻擊，這會在惡意使用者取得未經授權或合法指派的存取權和系統許可權時發生。 惡意使用者可以使用這種類型的攻擊來危害這部電腦主控的所有虛擬機器、網域和樹系。

當您打算虛擬化網域控制站時，請務必記住下列安全性考慮：

   - 裝載虛擬、可寫入的網域控制站之電腦的本機系統管理員，在認證中應該被視為等同于這些網域控制站所屬之所有網域和樹系的預設網域系統管理員。  
   - 若要避免安全性和效能問題，建議的設定是執行 Windows Server 2008 或更新版本之 Server Core 安裝的主機，但不含 Hyper-v 的應用程式。 此設定會限制伺服器上所安裝的應用程式和服務數目，這應該會導致效能增加，而且可能會有惡意攻擊的應用程式和服務攻擊電腦或網路。 這種設定的效果稱為縮小攻擊面。 在分公司或其他無法嚴密保護的位置中，建議使用唯讀網域控制站（RODC）。 如果有個別的管理網路存在，我們建議主機只能連線到管理網路。  
   - 您可以將 Bitlocker 與您的網域控制站搭配使用，因為 Windows Server 2016 您可以使用虛擬 TPM 功能，也提供來賓金鑰材料來解除鎖定系統磁碟區。
   - [受防護的網狀架構與受防護的 vm](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)可以提供其他控制來保護您的網域控制站。

如需 Rodc 的詳細資訊，請參閱[唯讀網域控制站規劃和部署指南](../../deploy/rodc/read-only-domain-controller-updates.md)。

如需保護網域控制站的詳細資訊，請參閱[保護 Active Directory 安裝的最佳做法指南](../../plan/security-best-practices/best-practices-for-securing-active-directory.md)。

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>不同主機和來賓設定的安全性界限

使用虛擬機器可讓您有許多不同的網域控制站設定。 若要讓虛擬機器影響 Active Directory 拓撲中的界限和信任，必須謹慎考慮。 下表說明 Active Directory 網域控制站和主機（Hyper-v 伺服器）及其來賓電腦（在 Hyper-v 伺服器上執行的虛擬機器）的可能設定。

|Machine|設定1|Configuration 2|
|-------|---------------|---------------|
|主機|工作組或成員電腦|工作組或成員電腦|
|Guest|網域控制站|工作組或成員電腦|

![](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - 主機電腦上的系統管理員與可寫入網域控制站來賓上的網域系統管理員具有相同的存取權，因此必須將其視為。 在 RODC 來賓的案例中，主機電腦的系統管理員與來賓 RODC 上的本機系統管理員具有相同的存取權。   
   - 如果主機已加入相同的網域，則虛擬機器中的網域控制站在主機上具有系統管理許可權。 如果惡意使用者第一次取得虛擬機器1的存取權，惡意使用者就有機會入侵所有虛擬機器。 這就是所謂的攻擊媒介。 如果有多個網域或樹系的網域控制站，則這些網域應該具有集中式管理，其中一個網域的系統管理員會在所有網域上受到信任。  
   - 即使虛擬機器1已安裝為 RODC，虛擬機器1的攻擊機會也存在。 雖然 RODC 的系統管理員未明確擁有網域系統管理員許可權，但 RODC 可以用來將原則傳送至主機電腦。 這些原則可能包括啟動腳本。 如果此操作成功，主機電腦可能會遭到入侵，然後用來危害主機電腦上的其他虛擬機器。  

## <a name="security-of-vhd-files"></a>VHD 檔案的安全性

虛擬網域控制站的 VHD 檔案相當於實體網域控制站的實體硬碟。 因此，它應該受到保護以保護實體網域控制站之硬碟的相同程度。 請確定只有可靠且受信任的系統管理員可以存取網域控制站的 VHD 檔案。

## <a name="rodcs"></a>Rodc

Rodc 的一項優點是能夠將它們放在無法保證實體安全性的位置，例如分公司。 您可以使用 Windows BitLocker 磁碟機加密保護 VHD 檔案本身（而不是其中的檔案系統）透過實體磁片的遭竊來危害主機。 

## <a name="performance"></a>效能

有了新的種微核心64位架構，舊版虛擬化平臺的 Hyper-v 效能大幅增加了。 為獲得最佳主機效能，主機應該是 Windows Server 2008 或更新版本的 Server Core 安裝，而且不應該有安裝 Hyper-v 以外的伺服器角色。

虛擬機器的效能特別取決於工作負載。 若要保證 Active Directory 的效能，請測試特定的拓撲。 使用可靠性和效能監視器（Perfmon）或[Microsoft 評估及規劃（MAP）](https://go.microsoft.com/fwlink/?linkid=137077)工具組等工具，在一段時間內評估目前的工作負載。 如果您想要清查目前存在於網路中的所有伺服器和伺服器角色，則地圖工具也會很有價值。

若要取得虛擬網域控制站效能的一般概念，請使用[Active Directory 效能測試控管（ADTest）](https://go.microsoft.com/fwlink/?linkid=137088)執行下列效能測試。

輕量型目錄存取協定（LDAP）測試是在具有 ADTest 的實體網域控制站上執行，然後在與實體網域控制站相同的伺服器上主控的虛擬機器上執行。 只有一個邏輯處理器用於實體電腦，而且只有一個虛擬處理器用於虛擬機器，才能輕鬆達到100% 的 CPU 使用率。 在下表中，每個測試之後，括弧中的字母和數位會指出 ADTest 中的特定測試。 如這份資料所示，虛擬網域控制站的效能是88到98% 的實體網域控制站效能。

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
<th>測量</th>
<th>Test</th>
<th>實體</th>
<th>Virtual</th>
<th>Delta</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>搜尋數/秒</p></td>
<td><p>搜尋基底範圍（L1）中的一般名稱</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10.71%</p></td>
</tr>
<tr class="even">
<td><p>搜尋數/秒</p></td>
<td><p>搜尋基底範圍（L2）中的一組屬性</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11.04%</p></td>
</tr>
<tr class="odd">
<td><p>搜尋數/秒</p></td>
<td><p>搜尋基底範圍（L3）中的所有屬性</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3.27%</p></td>
</tr>
<tr class="even">
<td><p>搜尋數/秒</p></td>
<td><p>在子樹狀結構範圍中搜尋一般名稱（L6）</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8.23%</p></td>
</tr>
<tr class="odd">
<td><p>成功的系結/秒</p></td>
<td><p>執行快速系結（B1）</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4.45%</p></td>
</tr>
<tr class="even">
<td><p>成功的系結/秒</p></td>
<td><p>執行簡單系結（B2）</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9.98%</p></td>
</tr>
<tr class="odd">
<td><p>成功的系結/秒</p></td>
<td><p>使用 NTLM 來執行系結（B5）</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1.12%</p></td>
</tr>
<tr class="even">
<td><p>寫入次數/秒</p></td>
<td><p>寫入多個屬性（W2）</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9.00%</p></td>
</tr>
</tbody>
</table>

為了確保效能令人滿意，已安裝整合元件（IC），以允許客體作業系統使用「enlightenment」或以虛擬機器感知的綜合驅動程式。 在安裝過程中，可能需要使用模擬的整合式磁片磁碟機（IDE）或網路介面卡驅動程式。 在生產環境中，您應該以綜合驅動程式取代這些模擬驅動程式，以提高效能。

當您監視具有可靠性和效能管理員（Perfmon）的虛擬機器效能時，在虛擬機器內，CPU 資訊將不會因為虛擬 CPU 排定在實體處理器上的方式而完全精確。 當您想要取得 Hyper-v 伺服器上執行之虛擬機器的 CPU 資訊時，請使用主機磁碟分割中的 Hyper-v 虛擬機器邏輯處理器計數器。

如需 AD DS 和 Hyper-v 的效能微調的詳細資訊，請參閱[Windows Server 2016 的效能微調方針](../../../../administration/performance-tuning/index.md)。

此外，不打算在設定為網域控制站的虛擬機器上使用差異磁片 VHD，因為差異磁片 VHD 可能會降低效能。 若要深入瞭解 Hyper-v 磁片類型（包括差異磁片），請參閱[新增虛擬硬碟 Wizard](https://go.microsoft.com/fwlink/?linkid=137279)。

如需有關虛擬主控環境中 AD DS 的詳細資訊，請參閱 Microsoft 知識庫中的在[虛擬主控環境中裝載 Active Directory 網域控制站時所要考慮的事項](https://go.microsoft.com/fwlink/?linkid=141292)。

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>虛擬網域控制站的部署考慮

當您部署網域控制站時，有幾個常見的虛擬機器實務做法，以及時間同步處理和儲存的特殊考慮。

## <a name="virtualization-deployment-practices-to-avoid"></a>要避免的虛擬化部署做法

虛擬化平臺（如 Hyper-v）提供一些便利功能，讓您更輕鬆地管理、維護、備份和遷移電腦。 不過，下列常見的部署做法和功能不應用於虛擬網域控制站：

- 為確保 Active Directory 寫入的持久性，請不要部署虛擬網域控制站的資料庫檔案（Active Directory 資料庫（NTDS）。DIT）、記錄和 SYSVOL 等虛擬 IDE 磁片。 相反地，請建立連接到虛擬 SCSI 控制器的第二個 VHD，並確定在安裝網域控制站期間，會將資料庫、記錄檔和 SYSVOL 放在虛擬機器的 SCSI 磁片上。  
- 請勿在您要設定為網域控制站的虛擬機器上，執行差異磁片虛擬硬碟（Vhd）。 這使得還原成先前的版本變得太容易，而且也會降低效能。 如需有關 VHD 類型的詳細資訊，請參閱[新增虛擬硬碟 Wizard](https://go.microsoft.com/fwlink/?linkid=137279)。  
- 不要在第一次使用系統準備工具（Sysprep）準備的 Windows Server 作業系統複本上部署新的 Active Directory 網域和樹系。 如需執行 Sysprep 的詳細資訊，請參閱[sysprep （系統準備）總覽](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

   > [!WARNING]
   > 不支援在網域控制站上執行 Sysprep。

- 若要協助防止潛在的更新序號（USN）復原狀況，請勿使用代表已部署網域控制站的 VHD 檔案複本來部署其他網域控制站。 如需 USN 復原的詳細資訊，請參閱[usn 和 Usn 回復](#usn-and-usn-rollback)。
   - Windows Server 2012 和更新版本可讓系統管理員在想要部署其他網域控制站時，複製網域控制站映射（如果已正確準備）
- 請勿使用 Hyper-v 匯出功能來匯出正在執行網域控制站的虛擬機器。
  - 使用 Windows Server 2012 和更新版本時，會將網域控制站虛擬來賓的匯出和匯入處理為非權威還原，因為它會偵測世代識別碼的變更，而且未設定進行複製。
  - 請確定您不再使用已匯出的來賓。
    - 您可以使用 Hyper-v 複寫來保留網域控制站的第二個非使用中複本。 如果您啟動複寫的映射，則也需要執行適當的清除，原因是在匯出 DC 來賓映射後，不使用來源。

## <a name="physical-to-virtual-migration"></a>實體對虛擬遷移

System Center Virtual Machine Manager （VMM）2008提供實體機器和虛擬機器的統一管理。 它也提供將實體機器遷移至虛擬機器的能力。 此程式稱為實體對虛擬機器轉換（P2V 轉換）。 在 P2V 轉換過程中，要遷移的新虛擬機器和實體網域控制站不得同時執行，以避免 usn[和 Usn 復原](#usn-and-usn-rollback)中所述的 usn 復原狀況。

您應該使用離線模式執行 P2V 轉換，如此一來，當網域控制站重新開啟時，目錄資料就會是一致的。 [轉換實體伺服器] 會提供和建議 [離線模式] 選項。 如需線上模式與離線模式之間差異的說明，請參閱[P2V：將實體電腦轉換成 VMM 中的虛擬機器](https://go.microsoft.com/fwlink/?linkid=155072)。 在 P2V 轉換期間，虛擬機器不應連線到網路。 只有在 P2V 轉換程式完成和驗證之後，才應該啟用虛擬機器的網路介面卡。 此時，實體來源機器將會關閉。 請不要在重新格式化硬碟之前，再次將實體來源機器重新帶入網路上。

> [!NOTE]
> 有更安全的選項可讓您建立新的虛擬 Dc，而不會執行建立 USN 復原的風險。 如果您已經有至少一個虛擬 DC，您可以透過定期升級、從媒體安裝（IfM）升級，以及使用網域控制站複製來設定新的虛擬 DC。
這也有助於避免硬體或平臺相關問題 P2V 轉換的虛擬來賓可能會遇到的問題。

> [!WARNING]
> 若要避免 Active Directory 複寫的問題，請確定指定的網路上有一個特定網域控制站只有一個實例（實體或虛擬）存在於任何時間點。
> 您可以降低舊複製發生問題的可能性：
> 
> - 當新的虛擬 DC 正在執行時，請使用： netdom resetpwd/Server： < 網域控制站 >，來變更電腦帳戶密碼兩次 。
> - 匯出並匯入新的虛擬來賓，以強制它成為新的世代識別碼，因此是資料庫的調用識別碼。
> 

## <a name="using-p2v-migration-to-create-test-environments"></a>使用 P2V 遷移來建立測試環境

您可以透過 VMM 使用 P2V 遷移來建立測試環境。 您可以將生產網域控制站從實體機器遷移至虛擬機器，以建立測試環境，而不需要永久關閉實際執行的網域控制站。 不過，如果相同網域控制站的兩個實例存在，測試環境必須位於與生產環境不同的網路上。 建立具有 P2V 遷移的測試環境時，必須特別小心，以避免可能影響您測試和生產環境的 USN 復原。 下列方法可讓您用來建立具有 P2V 的測試環境。

根據實體對虛擬遷移一節中所述的指導方針，從每個網域中的一個生產環境網域控制站，使用 P2V 遷移至測試虛擬機器。 實體生產機器和測試虛擬機器在恢復上線時，必須位於不同的網路。 若要避免在測試環境中進行 USN 回復，所有要從實體機器遷移至虛擬機器的網域控制站，都必須離線。 （您可以藉由停止 ntds 服務，或在目錄服務還原模式（DSRM）中重新開機電腦）來執行此動作。網域控制站離線之後，就不應該對環境引進新的更新。 電腦必須在 P2V 遷移期間保持離線;在所有電腦都已完全遷移之前，不應該將任何電腦重新上線。 若要深入瞭解 USN 復原，請參閱 USN 和 USN 回復。

後續的測試網域控制站應該升級為測試環境中的複本。

## <a name="time-service"></a>時間服務

針對設定為網域控制站的虛擬機器，建議您停用做為網域控制站的主機系統與客體作業系統之間的時間同步處理。 這可讓您的來賓網域控制站從網域階層同步處理時間。

若要停用 Hyper-v 時間同步處理提供者，請關閉 VM，並清除 [Integration Services] 底下的 [時間同步處理] 核取方塊。

> [!NOTE]
> 本指南最近已更新，以反映目前的建議，只從網域階層同步處理來賓網域控制站的時間，而不是先前的建議，以部分停用主機之間的時間同步。系統和來賓網域控制站。

## <a name="storage"></a>存放

若要將網域控制站虛擬機器的效能優化，並確保 Active Directory 寫入的持久性，請使用下列建議來儲存作業系統、Active Directory 和 VHD 檔案：

- **來賓儲存體**。 將 Active Directory 資料庫檔案（ntds.dit）、記錄檔和 SYSVOL 檔案儲存在與作業系統檔案不同的虛擬磁片上。 建立連接到虛擬 SCSI 控制器的第二個 VHD，並將資料庫、記錄和 SYSVOL 儲存在虛擬機器的虛擬 SCSI 磁片上。 相較于虛擬 IDE，虛擬 SCSI 磁片可提供更高的效能，並支援強制單元存取（FUA）。 FUA 可確保作業系統直接從媒體寫入和讀取資料，略過任何和所有快取機制。

  > [!NOTE]
  > 如果您打算使用虛擬 DC 來賓的 Bitlocker，您必須確定已將其他磁片區設定為「自動解除鎖定」。
  > 如需設定自動解除鎖定的詳細資訊，請參閱[BitLockerAutoUnlock](https://docs.microsoft.com/powershell/module/bitlocker/enable-bitlockerautounlock)

- **VHD 檔案的主機存放裝置**。 建議：主機儲存體建議會處理 VHD 檔案的儲存。 為了達到最大效能，請不要將 VHD 檔案儲存在其他服務或應用程式經常使用的磁片上，例如安裝主機 Windows 作業系統的系統磁片。 將每個 VHD 檔案儲存在與主機作業系統和任何其他 VHD 檔案不同的磁碟分割上。 理想的設定是將每個 VHD 檔案儲存在不同的實體磁片磁碟機上。  

  主機實體磁片系統也必須滿足下列**其中一個**準則，才能符合虛擬化工作負載資料完整性的需求：  

   - 系統會使用伺服器類別磁片（SCSI、光纖通道）。  
   - 系統會確定磁片已連線到電池支援的快取主機匯流排介面卡（HBA）。  
   - 系統會使用存放控制器（例如，RAID 系統）作為存放裝置。  
   - 系統會確保磁片的電源受到不斷電供應系統（UPS）的保護。  
   - 系統會確定磁片的寫入快取功能已停用。  

- 已**修正 VHD 與傳遞磁片**的比較。 有許多方式可設定虛擬機器的存放裝置。 使用 VHD 檔案時，固定大小的 Vhd 會比動態 Vhd 更有效率，因為固定大小 Vhd 的記憶體會在建立時進行配置。 可供虛擬機器用來存取實體存放媒體的傳遞磁片，甚至更適合效能。 傳遞磁片基本上是連接至虛擬機器的實體磁片或邏輯單元編號（Lun）。 傳遞磁片不支援快照集功能。 因此，傳遞磁片是慣用的硬碟設定，因為不建議使用具有網域控制站的快照集。  

若要減少 Active Directory 資料損毀的機會，請使用虛擬 SCSI 控制器：

   - 在裝載虛擬網域控制站的 Hyper-v 伺服器上，使用 SCSI 實體磁片磁碟機（而不是 IDE/ATA 磁片磁碟機）。 如果您無法使用 SCSI 磁片磁碟機，請確定裝載虛擬網域控制站的 ATA/IDE 磁片磁碟機上已停用寫入快取。 如需詳細資訊，請參閱[事件識別碼1539–資料庫完整性](https://go.microsoft.com/fwlink/?linkid=162419)。
   - 為了保證 Active Directory 寫入的持久性，Active Directory 資料庫、記錄和 SYSVOL 必須放在虛擬 SCSI 磁片上。 虛擬 SCSI 磁片支援強制單元存取（FUA）。 FUA 可確保作業系統直接從媒體寫入和讀取資料，略過任何和所有快取機制。  

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>虛擬網域控制站的操作考慮

在虛擬機器上執行的網域控制站具有作業限制，不適用於在實體機器上執行的網域控制站。 當您使用虛擬網域控制站時，有一些您不應該使用的虛擬化軟體功能和作法：

   - 請勿在虛擬機器中暫停、停止或儲存網域控制站的儲存狀態，時間超過樹系的標記存留期，然後從暫停或儲存狀態繼續。 這麼做可能會干擾複寫。 若要瞭解如何判斷樹系的標記存留期，請參閱[判斷樹](https://go.microsoft.com/fwlink/?linkid=137177)系的標記存留期。  
   - 請勿複製或複製虛擬硬碟（Vhd）。 即使來賓 VM 具有保護措施，仍然可以複製個別的 Vhd，並導致 USN 回復。
   - 請勿採用或使用虛擬網域控制站的快照集。 在技術上，Windows Server 2012 和更新版本支援它，並不是良好的備份策略的替代方案。 取得 DC 快照集或還原快照集的原因有很多。
   - 請勿在設定為網域控制站的虛擬機器上使用差異磁片 VHD。 這會讓還原到舊版變得太簡單，而且也會降低效能。  
   - 請勿在執行網域控制站的虛擬機器上使用匯出功能。  
   - 請勿還原網域控制站，或嘗試復原 Active Directory 資料庫的內容，而不是使用支援的備份。 如需詳細資訊，請參閱[虛擬網域控制站的備份和還原考慮](#backup-and-restore-practices-to-avoid)。  

所有這些建議都是為了協助避免更新序號（USN）復原的可能性。 如需 USN 復原的詳細資訊，請參閱 USN 和 USN 回復。

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>虛擬網域控制站的備份和還原考慮

備份網域控制站是任何環境的關鍵需求。 備份會在網域控制站失敗或系統管理錯誤發生時防止資料遺失。 如果發生這類事件，則必須將網域控制站的系統狀態復原到失敗或錯誤之前的時間點。 將網域控制站還原為狀況良好狀態時，支援的方法是使用與 Active Directory 相容的備份應用程式（例如 Windows Server Backup）來還原源自目前網域安裝的系統狀態備份。控制器. 如需有關使用 Windows Server Backup 搭配 Active Directory Domain Services （AD DS）的詳細資訊，請參閱[AD DS 備份和復原逐步指南](https://go.microsoft.com/fwlink/?LinkId=138501)。

使用虛擬機器技術時，Active Directory 還原作業的特定需求會增加重要性。 例如，如果您使用虛擬硬碟（VHD）檔案的複本來還原網域控制站，就會略過還原後更新網域控制站資料庫版本的重要步驟。 複寫會繼續進行不當的追蹤號碼，導致網域控制站複本之間的資料庫不一致。 在大部分情況下，複寫系統會偵測到這個問題，而且不會報告任何錯誤，儘管網域控制站之間不一致。

有一種支援的方法可以執行虛擬網域控制站的備份和還原：

1. 在客體作業系統中執行 Windows Server Backup。  

使用 Windows Server 2012 和更新版本的 Hyper-v 主機和來賓，您可以使用快照集、來賓 VM 匯出和匯入，以及 Hyper-v 複寫，來進行支援的網域控制站備份。 不過，這些全都不適合用來建立適當的備份歷程記錄，但虛擬機器匯出的情況稍有例外。

在 Windows Server 2016 Hyper-v 中，支援「生產快照」，其中 Hyper-v 伺服器會觸發來賓的 VSS 備份，而當來賓使用快照集完成時，主機會提取 Vhd，並將它們儲存在備份位置。

雖然這適用于 Windows Server 2012 和更新版本，但與 Bitlocker 不相容：

- 執行 VSS 快照集時，AD 想要執行快照集後工作，將資料庫標示為來自備份，或在準備 RODC 的 IFM 來源的情況下，從資料庫移除認證。
- 當 Hyper-v 掛接這項工作的快照磁片區時，沒有設備可以解除鎖定磁片區以進行未加密的存取。 因此 AD 資料庫引擎無法存取資料庫，最後會導致快照集失敗。

> [!NOTE]
> 先前提到的受防護 VM 專案具有 Hyper-v 主機驅動型備份，做為來賓 VM 的最大資料保護的非目標。

## <a name="backup-and-restore-practices-to-avoid"></a>要避免的備份與還原做法

如先前所述，在虛擬機器中執行的網域控制站具有不適用於在實體機器中執行之網域控制站的限制。 當您備份或還原虛擬網域控制站時，有一些您不應該使用的虛擬化軟體功能和作法：

   - 請勿複製或複製網域控制站的 VHD 檔案，而不是執行定期備份。 如果複製或複製 VHD 檔案，它就會過時。 然後，如果 VHD 以標準模式啟動，您將會遇到 USN 回復。 您應該執行 Active Directory Domain Services （AD DS）支援的適當備份作業，例如使用 Windows Server Backup 功能。  
   - 請勿使用快照集功能做為備份，來還原已設定為網域控制站的虛擬機器。 當您使用 Windows Server 2008 R2 和更舊版本將虛擬機器還原為先前的狀態時，複寫就會發生問題。 如需詳細資訊，請參閱[usn 和 Usn 回復](#usn-and-usn-rollback)。 雖然使用快照還原唯讀網域控制站（RODC）並不會造成複寫問題，但仍不建議這種還原方法。  

## <a name="restoring-a-virtual-domain-controller"></a>還原虛擬網域控制站

若要在網域控制站失敗時進行還原，您必須定期備份系統狀態。 系統狀態包括 Active Directory 資料和記錄檔、登錄、系統磁碟區（SYSVOL 資料夾），以及作業系統的各種元素。 實體和虛擬網域控制站的這項需求是相同的。 Active Directory 相容備份應用程式執行的系統狀態還原程式，是為了確保在還原程式之後（包括複寫通知）進行本機和複寫 Active Directory 資料庫的一致性。調用識別碼的夥伴會重設。 不過，使用虛擬裝載環境和磁片或作業系統映射應用程式，系統管理員可以略過網域控制站系統狀態還原時通常會發生的檢查和驗證。

當網域控制站虛擬機器失敗，而且未發生更新序號（USN）復原時，有兩種支援的虛擬機器還原情況：

   - 如果比較舊失敗的有效系統狀態資料備份存在，您可以使用您用來建立備份的備份公用程式的 [還原] 選項來還原系統狀態。 系統狀態資料備份必須在標記存留期的範圍內，使用 Active Directory 相容的備份公用程式建立，這預設為不超過180天。 您應至少每隔一半的標記存留期備份網域控制站。 如需如何判斷樹系特定標記存留期的指示，請參閱[判斷樹](https://go.microsoft.com/fwlink/?linkid=137177)系的標記存留期。  
   - 如果 VHD 檔案的工作複本可以使用，但沒有可用的系統狀態備份，您可以移除現有的虛擬機器。 使用先前的 VHD 複本來還原現有的虛擬機器，但請務必以目錄服務還原模式（DSRM）啟動它，並正確設定登錄，如下一節所述。 然後，以正常模式重新開機網域控制站。

使用下圖中的程式來判斷還原虛擬網域控制站的最佳方式。

![](media/virtualized-domain-controller-architecture/Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

對於 Rodc，還原程式和決策會比較簡單。

![](media/virtualized-domain-controller-architecture/Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>還原虛擬網域控制站的系統狀態備份

如果網域控制站虛擬機器有有效的系統狀態備份，您可以遵循用來備份 VHD 檔案的備份工具所指定的還原程式，安全地還原備份。

> [!IMPORTANT]
> 若要正確還原網域控制站，您必須以 DSRM 啟動它。 您不能允許網域控制站在正常模式下啟動。 如果您在系統啟動期間錯過了輸入 DSRM 的機會，請關閉網域控制站的虛擬機器，然後才能在正常模式中完全啟動。 請務必以 DSRM 啟動網域控制站，因為以標準模式啟動網域控制站時，即使網域控制站已與網路中斷連線，仍會遞增其 Usn。 如需 USN 復原的詳細資訊，請參閱 USN 和 USN 回復。 

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>還原虛擬網域控制站的系統狀態備份

1. 啟動網域控制站的虛擬機器，然後按 F5 存取 Windows 開機管理程式畫面。 如果您需要輸入連線認證，請立即按一下虛擬機器上的 [**暫停**] 按鈕，使其不會繼續啟動。 然後，輸入您的連線認證，然後按一下虛擬機器上的 [**播放**] 按鈕。 按一下 [虛擬機器] 視窗內部，然後按 F5。

   如果您看不到 [Windows 開機管理程式] 畫面，且網域控制站開始處於正常模式，請關閉虛擬機器，以防止它完成啟動。 請視需要重複此步驟，直到您能夠存取 Windows 開機管理程式畫面為止。 您無法從 Windows 錯誤修復功能表存取 DSRM。 因此，請關閉虛擬機器，然後在 [Windows 錯誤修復] 功能表出現時再試一次。

2. 在 [Windows 開機管理程式] 畫面中，按 F8 以存取 advanced 開機選項。
3. 在 [**高級開機選項**] 畫面中，選取 [**目錄服務還原模式]** ，然後按 enter。 這會以 DSRM 啟動網域控制站。
4. 針對您用來建立系統狀態備份的工具，請使用適當的還原方法。 如果您使用 Windows Server Backup，請參閱[執行 AD DS 的非系統授權還原](https://go.microsoft.com/fwlink/?linkid=132637)。

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>當有適當的系統狀態資料備份無法使用時，還原虛擬網域控制站

如果您沒有比較舊虛擬機器失敗的系統狀態資料備份，您可以使用先前的 VHD 檔案還原在虛擬機器上執行的網域控制站。 如果您可以，請建立 VHD 的複本，如此一來，如果您在程式期間遇到問題或錯過某個步驟，可以使用複製的 VHD 再試一次。

> [!IMPORTANT]
> - 您不應該考慮使用下列程式來取代定期規劃和排程的備份。
> - **使用下列程式執行的還原不受 Microsoft 支援，只有在沒有其他替代方案時才應該使用。**
> - 如果您要還原的 VHD 複本已由任何虛擬機器以標準模式啟動，請勿使用此程式。

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>還原先前版本的虛擬網域控制站 VHD，而不使用系統狀態資料備份

1. 使用先前的 VHD，以 DSRM 啟動虛擬網域控制站，如上一節所述。 不允許網域控制站在正常模式下啟動。 如果您錯過 Windows 開機管理程式畫面，而網域控制站在正常模式下開始啟動，請關閉虛擬機器，以防止它完成啟動。 如需輸入 DSRM 的詳細指示，請參閱上一節。
2. 開啟「登錄編輯程式」。 若要開啟登錄編輯程式，請依序按一下 [**開始**] 和 [**執行**]、輸入**regedit**，然後按一下 [確定]。 如果出現 [ **使用者帳戶控制** ] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [ **是**]。 在 [登錄編輯程式] 中，展開下列路徑： **HKEY\_本機\_機\\系統\\CurrentControlSet\\Services\\NTDS\\參數**。 尋找名為 [ **DSA 先前的還原計數**] 的值。 如果值在該處，請記下設定。 如果值不存在，則設定等於預設值，也就是零。 如果您在該處看不到值，請不要加一。
3. 以滑鼠右鍵按一下 [**參數**] 索引鍵，按一下 [**新增**]，然後按一下 [ **DWORD （32-位）值**]。
4. 輸入**從備份還原**的新名稱資料庫，然後按 enter。
5. 按兩下您剛才建立的值，以開啟 [**編輯 DWORD （32-位）值**] 對話方塊，然後在 [**數值資料**] 方塊中輸入**1** 。 在執行 Windows 2000 Server Service Pack 4 （SP4）、Windows Server 2003 的網域控制站上，您可以從已安裝 Microsoft 知識庫中的[Windows server 2003、Windows server 2008 和 Windows server 2008 R2 中的 USN 回復](https://go.microsoft.com/fwlink/?linkid=137182)，以及 windows server 2008 中，取得**從備份輸入選項還原的資料庫**。
6. 以正常模式重新開機網域控制站。
7. 當網域控制站重新開機時，請開啟事件檢視器。 若要開啟 [事件檢視器]，請按一下 [開始]，再按一下 [控制台]，連按兩下 [系統管理工具]，再連按兩下 [事件檢視器]。
8. 展開 [**應用程式及服務記錄**檔]，然後按一下 [**目錄服務**] 記錄檔。 確定事件出現在詳細資料窗格中。
9. 在**目錄服務**記錄檔上按一下滑鼠右鍵，然後按一下 [**尋找**]。 在 [**尋找目標**] 中，輸入**1109**，然後按一下 **[尋找下一個]** 。
10. 您應該會看到至少一個事件識別碼1109專案。 如果您看不到此專案，請繼續進行下一個步驟。 否則，請按兩下該專案，然後查看確認已對 InvocationID 進行更新的文字：

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
12. 使用 [登錄編輯程式] 確認 [ **DSA 先前的還原計數**] 中的值是否等於先前的值加一。 如果這不是正確的值，而且在事件檢視器中找不到事件識別碼1109的專案，請確認網域控制站的 service pack 是最新的。 您無法在相同的 VHD 上再次嘗試此程式。 從步驟1開始，您可以在 VHD 複本或尚未以標準模式啟動的不同 VHD 上再試一次。
13. 關閉登錄編輯程式。

## <a name="usn-and-usn-rollback"></a>USN 和 USN 復原

本節說明因使用較舊版本的虛擬機器來還原 Active Directory 資料庫不正確時，可能會發生的複寫問題。 如需 Active Directory 複寫程式的其他詳細資料，請參閱[Active Directory 複寫概念](../replication/active-directory-replication-concepts.md)

## <a name="usns"></a>Usn

Active Directory Domain Services （AD DS）會使用更新序號（Usn）來追蹤網域控制站之間的資料複寫。 每次變更目錄中的資料時，USN 會遞增以指出已進行變更。

對於目的地網域控制站儲存的每個目錄分割，會使用 Usn 來追蹤網域控制站在其資料庫中引進的最新原始更新，以及每個儲存複本的網域控制站的狀態。目錄分割。 當網域控制站將變更複寫到另一個時，他們會查詢其複寫協力電腦是否有 Usn 的變更，而該變更的 USN 大於網域控制站從每個夥伴收到的最後一項變更。

下列兩個複寫中繼資料表包含 Usn。 來源和目的地網域控制站會使用它們來篩選目的地網域控制站所需的更新。

1. **最新向量**：目的地網域控制站為了追蹤從所有來源網域控制站接收的原始更新而維護的資料表。 當目的地網域控制站要求目錄磁碟分割的變更時，它會將其最新向量提供給來源網域控制站。 接著，來源網域控制站會使用此值來篩選其傳送至目的地網域控制站的更新。 來源網域控制站會在成功複寫週期完成時，將其最新向量傳送至目的地，以確保目的地網域控制站知道它已與每個網域控制站同步處理。原始更新和更新的層級與來源相同。  
2. **上限**標準：目的地網域控制站維護的值，用來追蹤從特定資料分割的特定來源網域控制站收到的最近變更。 上限標準可防止來源網域控制站送出目的地網域控制站已收到的變更。  

## <a name="directory-database-identity"></a>目錄資料庫身分識別

除了 Usn 以外，網域控制站還會追蹤來源複寫協力電腦的目錄資料庫。 在伺服器上執行之目錄資料庫的識別會與伺服器物件本身的識別分開維護。 每個網域控制站上的目錄資料庫身分識別會儲存在 [NTDS 設定] 物件的 [ **invocationID** ] 屬性中，其位於下列輕量型目錄存取協定（LDAP）路徑： CN = NTDS Settings，Cn = ServerName，Cn = Servers，Cn =*SiteName*，cn = Sites，cn = Configuration，dc =*ForestRootDomain*。 伺服器物件識別會儲存在 NTDS 設定物件的**objectGUID**屬性中。 伺服器物件的身分識別永遠不會變更。 不過，目錄資料庫的身分識別會在伺服器上發生系統狀態還原程式時，或在新增應用程式目錄分割時變更，然後從伺服器中移除並重新加入。 （其他案例：當 HyperV 實例在包含虛擬 DC 的 VHD 的磁碟分割上觸發其 VSS 寫入器時，來賓會輪流觸發自己的 VSS 寫入器（上面的備份/還原所使用的相同機制），而導致 invocationID 重設的另一種方法）

因此， **invocationID**實際上會在網域控制站上，將一組原始更新與特定版本的目錄資料庫相關聯。 最新向量和上限標準資料表會分別使用**invocationID**和 DC GUID，讓網域控制站知道複寫資訊即將推出的 Active Directory 資料庫複本。

**InvocationID**是全域唯一識別碼（GUID）值，在您執行命令**repadmin/showrepl**之後，會在輸出頂端附近看到。 下列文字代表命令的範例輸出：

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

在網域控制站上正確還原 AD DS 時，會重設**invocationID** 。 由於這項變更，您將會遇到複寫流量增加的情況–這是相對於所複寫之分割區大小的持續時間

例如，假設 VDC1 和 DC2 是相同網域中的兩個網域控制站。 下圖顯示在適當的還原情況下重設 invocationID 值時，DC2 對 VDC1 的認知。

![](media/virtualized-domain-controller-architecture/Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>USN 復原

當 usn 的一般更新被規避，且網域控制站嘗試使用低於其最新更新的 USN 時，就會發生 USN 復原。 系統會偵測到 USN 復原，並在建立樹系中的分歧之前停止複寫（在大多數情況下）。 

USN 復原可能會以許多方式造成，例如，使用舊的虛擬硬碟（VHD）檔案，或執行實體對虛擬轉換（P2V 轉換）時，不會確保實體機器在轉換後永久保持離線狀態。 請採取下列預防措施，以確保不會進行 USN 復原：

   - 當不執行 Windows Server 2012 或更新版本時，請勿採用或使用網域控制站虛擬機器的快照集。
   - 請勿複製網域控制站 VHD 檔案。  
   - 當未執行 Windows Server 2012 或更新版本時，請勿匯出正在執行網域控制站的虛擬機器。  
   - 請勿還原網域控制站，或嘗試透過任何其他方式回復 Active Directory 資料庫的內容，而不是支援的備份解決方案，例如 Windows Server Backup。  

在某些情況下，可能會無法偵測到 USN 回復。 在其他情況下，可能會造成其他複寫錯誤。 在這些情況下，您必須找出問題的範圍，並及時加以處理。 如需如何移除因 USN 復原而可能發生之延遲物件的詳細資訊，請參閱 Microsoft 知識庫中的已[過期 Active Directory 物件在 Windows Server 2003 中產生事件識別碼 1988](https://go.microsoft.com/fwlink/?linkid=137185) 。

## <a name="usn-rollback-detection"></a>USN 復原偵測

在大部分情況下，系統會偵測到不適當的還原程式所導致的**invocationID**重設，而不會有對應的重新開機。 Windows Server 2008 會在不適當的網域控制站還原作業之後，針對不適當的複寫提供保護。 這些保護是由不適當的還原作業導致較低的 Usn （代表複寫協力電腦已收到的原始變更）所觸發。

在 Windows Server 2008 和 Windows Server 2003 SP1 中，當目的地網域控制站使用先前使用的 USN 來要求變更時，其來源複寫協力電腦的回應會由目的地網域控制站解讀，以表示其複寫中繼資料已過期。 這表示來源網域控制站上的 Active Directory 資料庫已回復為先前的狀態。 例如，虛擬機器的 VHD 檔案已復原為先前的版本。 在此情況下，目的地網域控制站會在已識別為已進行不適當還原的網域控制站上，起始下列隔離措施：

   - AD DS 暫停 Net Logon 服務，這會防止使用者帳戶和電腦帳戶變更帳戶密碼。 此動作會在不適當的還原之後發生這種情況時，防止遺失這類變更。  
   - AD DS 會停用傳入和傳出 Active Directory 複寫。  
   - AD DS 會在目錄服務事件記錄檔中產生事件識別碼2095，以指出條件。  

下圖顯示在 VDC2 （在虛擬機器上執行的目的地網域控制站）上偵測到 USN 回復時所發生的事件順序。 在此圖中，當複寫協力電腦偵測到 VDC2 已傳送目的地網域控制站先前所見的最新 USN 值（表示 VDC2's 資料庫已復原）時，就會在 VDC2 上進行 USN 復原的偵測。時間不正確。

![](media/virtualized-domain-controller-architecture/Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

如果目錄服務事件記錄檔報告事件識別碼2095，請立即完成下列程式。

## <a name="to-resolve-event-id2095"></a>若要解析事件識別碼2095

1. 隔離已從網路記錄錯誤的虛擬機器。
2. 嘗試判斷是否有任何變更源自此網域控制站，並傳播至其他網域控制站。 如果事件是啟動虛擬機器快照集或複本的結果，請嘗試判斷 USN 復原的時間。 接著，您可以檢查該網域控制站的複寫協力電腦，以判斷從那時起是否發生複寫。

   您可以使用 Repadmin 工具來進行這項判斷。 如需如何使用 Repadmin 的詳細資訊，請參閱[使用 Repadmin 監視和疑難排解 Active Directory](https://go.microsoft.com/fwlink/?linkid=122830)複寫。 如果您無法自行判斷，請洽詢[Microsoft 支援服務](https://support.microsoft.com)尋求協助。

3. 強制降級網域控制站。 這包括清理網域控制站的中繼資料，以及佔用操作主機（也稱為彈性單一主機操作或 FSMO）角色。 如需詳細資訊，請參閱 Microsoft 知識庫中的「從 USN 復原復原」一節，其中[說明如何在 Windows server 2003、Windows server 2008 和 Windows server 2008 R2 中從 usn 復原偵測和復原](https://go.microsoft.com/fwlink/?linkid=137182)。
4. 刪除網域控制站的所有先前的 VHD 檔案。

## <a name="undetected-usn-rollback"></a>未偵測到 USN 回復

在下列兩種情況的其中一種情況下，可能無法偵測到 USN 復原：

1. VHD 檔案會附加到同時在多個位置執行的不同虛擬機器。  
2. 還原的網域控制站上的 USN 已超過另一個網域控制站已收到的最後一個 USN。  

在第一種情況下，其他網域控制站可能會使用其中一部虛擬機器進行複寫，而任何一部虛擬機器上可能會發生變更，而不會複寫到另一部。 此樹系的分歧很難以偵測，而且會導致無法預期的目錄回應。 如果實體和虛擬機器都是在相同的網路上執行，則在 P2V 遷移之後可能會發生這種情況。 如果從相同的實體網域控制站建立多個虛擬網域控制站，然後在相同的網路上執行，也可能會發生這種情況。

在第二種情況下，Usn 範圍會套用至兩個不同的變更集。 這可以在不偵測到的情況下繼續進行長時間。 每當修改在該時間建立的物件時，就會在事件檢視器中偵測到延遲物件並回報為事件識別碼1988。 下圖顯示在這種情況下，可能無法偵測到 USN 復原的方式。

![](media/virtualized-domain-controller-architecture/Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>唯讀網域控制站

Rodc 是在 Active Directory 資料庫中裝載資料分割之唯讀複本的網域控制站。 Rodc 會避免大部分的 USN 復原問題，因為它們不會將任何變更複寫到其他網域控制站。 不過，如果 RODC 從已受到 USN 回復影響的可寫入網域控制站進行複寫，RODC 也會受到影響。

不建議使用快照集還原 RODC。 使用與 Active Directory 相容的備份應用程式來還原 RODC。 此外，與可寫入的網域控制站一樣，必須小心不要讓 RODC 離線超過標記存留期。 這種狀況可能會導致 RODC 上的物件延遲。

如需 Rodc 的詳細資訊，請參閱[唯讀網域控制站規劃和部署指南](../../deploy/rodc/read-only-domain-controller-updates.md)。
