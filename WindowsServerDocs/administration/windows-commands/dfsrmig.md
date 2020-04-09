---
title: dfsrmig
description: 適用于 drsrmig 的 Windows 命令主題，可將 SYSvol 複寫從檔案複寫服務（FRS）遷移至分散式檔案系統（DFS）複寫、提供有關遷移進度的資訊，以及修改 active directory 網域服務（AD DS）物件以支援遷移。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d688832169cf216e628fe761f85d708a78103d89
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846161"
---
# <a name="dfsrmig"></a>dfsrmig

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將 SYSvol 複寫從檔案複寫服務（FRS）遷移至分散式檔案系統（DFS）複寫、提供有關遷移進度的資訊，以及修改 active directory 網域服務（AD DS）物件以支援遷移。

如需如何使用此命令的範例，請參閱本檔稍後的[範例](#BKMK_examples)一節。

## <a name="syntax"></a>語法

```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>參數

|參數 |描述 | |-----_ |------ | |/SetGlobalState <state> |將網域所需的全域遷移狀態設定為對應至*state*所指定之值的狀態。<p>若要繼續進行遷移或復原程式，請使用此命令來迴圈處理有效的狀態。 此選項可讓您藉由在 PDC 模擬器的 AD DS 中設定全域遷移狀態，來起始和控制遷移程式。 如果 PDC 模擬器無法使用，則此命令會失敗。<p>您只能將全域遷移狀態設定為穩定狀態。 適用于*state*的有效值為**0** （代表開始狀態）、 **1**代表備妥狀態、 **2**表示重新導向狀態，而**3**表示已刪除狀態。<p>遷移至 [已排除] 狀態是無法復原的，而且不可能從該狀態回復，因此，只有當您完全 committd 使用 DFS 複寫進行 SYSvol 複寫時，才會使用值**3**來表示*狀態*。 ||/GetGlobalState |在 PDC 模擬器上執行時，從 AD DS 資料庫的本機複本中，抓取網域的目前全域遷移狀態。<p>使用此選項來確認您已設定正確的全域遷移狀態。 只有穩定的遷移狀態可以是全域遷移狀態，因此**drsrmig**命令以 **/GetGlobalState**選項回報的結果會對應至您可以使用 **/SetGlobalState**選項設定的狀態。<p>您應該只在 PDC 模擬器上使用 **/GetGlobalState**選項來執行**drsrmig**命令。 active directory 複寫會將全域狀態複寫到網域中的其他網域控制站，但如果您在 PDC 模擬器以外的網域控制站上使用 **/GetGlobalState**選項來執行**drsrmig**命令，則複寫延遲可能會造成不一致的情況。 若要檢查 PDC 模擬器以外之網域控制站的本機遷移狀態，請改用 **/GetMigrationState**選項。 | |/GetMigrationState |抓取網域中所有網域控制站的目前本機遷移狀態，並判斷這些本機狀態是否符合目前的全域遷移狀態。<p>使用此選項來判斷所有網域控制站是否已達到全域遷移狀態。 當您使用 **/GetMigrationState**選項時， **dsfrmig**命令的輸出會指出是否已完成遷移至目前的全域狀態，而且它會列出尚未達到目前全域遷移狀態的任何網域控制站的本機遷移狀態。 網域控制站的本機遷移狀態可以包含尚未達到目前全域遷移狀態之網域控制站的轉換狀態。 | |/createGlobalObjects |在 DFS 複寫使用的 AD DS 中，建立全域物件和設定。<p>在正常的遷移程式期間，您應該不需要使用此選項，因為 DFS 複寫服務會在從啟動狀態遷移到備妥狀態期間，自動建立這些 AD DS 物件和設定。 在下列情況下，請使用此選項來手動建立這些物件和設定：<p>-  在**遷移期間升級新的唯讀網域控制站**。 DFS 複寫服務會在從啟動狀態遷移到備妥狀態期間，自動建立 DFS 複寫的 AD DS 物件和設定。 如果在此轉換後於網域中升級新的唯讀網域控制站，但在遷移至 [已刪除] 狀態之前，則不會在 AD DS 中建立對應至新啟用之唯讀網域控制站的物件，而導致複寫和遷移失敗。<br />-在此情況下，您可以執行**drsrmig**命令問題 **/createGlobalObjects**選項，以在任何尚未擁有的唯讀網域控制站上手動建立物件。 執行此命令不會影響已經有 DFS 複寫服務之物件和設定的網域控制站。<p>- **DFS 複寫服務的全域設定遺失或已刪除**。 如果特定網域控制站缺少這些設定，從啟動狀態到備妥狀態的遷移會停止到網域控制站的準備轉換狀態。 在此情況下，您可以使用**drsrmig**命令搭配 **/createGlobalObjects**選項來手動建立設定。 **注意：** 因為唯讀網域控制站的 DFS 複寫服務的全域 AD DS 設定是在 PDC 模擬器上建立的，所以這些設定必須先從 PDC 模擬器複寫到唯讀網域控制站，唯讀網域控制站上的 DFS 複寫服務才能使用這些設定。 由於作用中的目錄複寫延遲，這項複寫可能需要一些時間才會發生。 | |/deleteRoNtfrsMember [< read_only_domain_controller_name >] |刪除對應至指定唯讀網域控制站之 FRS 複寫的全域 AD DS 設定，或刪除所有唯讀網域控制站之 FRS 複寫的全域 AD DS 設定（如果未指定*read_only_domain_controller_name*的值）。<p>在正常的遷移程式期間，您應該不需要使用此選項，因為 DFS 複寫服務會在從重新導向狀態遷移至 [已刪除] 狀態期間，自動刪除這些 AD DS 設定。 因為唯讀網域控制站無法從 AD DS 刪除這些設定，所以 PDC 模擬器會執行這項操作，而變更最終會在 active directory 複寫適用的延遲之後，複寫到唯讀網域控制站。<p>只有在唯讀網域控制站上的自動刪除失敗時，您才可以使用此選項手動刪除 AD DS 設定，並在從重新導向狀態遷移至 [已刪除] 狀態時，停止長輸入的唯讀網域控制站。 | |/deleteRoDfsrMember [< read_only_domain_controller_name >] |刪除對應至指定唯讀網域控制站之 DFS 複寫的全域 AD DS 設定，如果未指定*read_only_domain_controller_name*的值，則刪除所有唯讀網域控制站之 DFS 複寫的全域 AD DS 設定。<p>使用此選項只會在唯讀網域控制站上自動刪除失敗時，手動刪除 AD DS 設定，並在從備妥狀態復原至啟動狀態時，長時間停止唯讀網域控制站。 | |/? |在命令提示字元中顯示說明。 相當於執行沒有任何選項的**drsrmig** 。 |

## <a name="remarks"></a>備註
- drsrmig 是 DFS 複寫服務的遷移工具，會隨 DFS 複寫服務一起安裝。
 針對新的 Windows Server 2008 伺服器，當您將電腦升級為網域控制站時，Dcpromo.exe 會安裝並啟動 DFS 複寫服務。 當您將伺服器從 Windows Server 2003 升級至 Windows Server 2008 時，升級程式會安裝並啟動 DFS 複寫服務。 您不需要安裝 DFS 複寫角色服務，就可以安裝和啟動 DFS 複寫服務。
- 只有在執行 Windows Server 2008 網域功能等級的網域控制站上才支援**drsrmig**工具，因為只有在以 windows server 2008 網域功能等級運作的網域控制站上，才能從 FRS 遷移到 DFS 複寫。
- 您可以在任何網域控制站上執行**drsrmig**命令，但建立或操作 AD DS 物件的作業只能在可讀寫功能的網域控制站（不是在唯讀網域控制站上）上使用。
- 執行**drsrmig**而不需要任何選項時，會在命令提示字元中顯示說明。

## <a name="examples"></a><a name=BKMK_examples></a>典型
若要將全域遷移狀態設定為 [備妥] （**1**），並起始 [遷移至] 或 [從備妥的狀態復原]，請輸入：
 ```
 dfsrmig /SetGlobalState 1
 ```
 若要將全域遷移狀態設定為 [啟動] （**0**），並起始 [復原至啟動狀態]，請輸入：
 ```
 dfsrmig /SetGlobalState 0
 ```
 若要顯示全域遷移狀態，請輸入：
 ```
 dfsrmig /GetGlobalState
 ```
 這個範例會顯示**drsrmig/GetGlobalState**命令的一般輸出。
 ```
 Current DFSR global state: Prepared 
 Succeeded.
 ```
 若要顯示所有網域控制站上的本機遷移狀態是否符合全域遷移狀態的相關資訊，以及本機狀態不符合全域狀態的任何網域控制站的本機遷移狀態，請輸入：
 ```
 dfsrmig /GetMigrationState
 ```
 當所有網域控制站上的本機遷移狀態符合全域遷移狀態時，此範例會顯示**drsrmig/GetMigrationState**命令的一般輸出。
 ```
 All Domain Controllers have migrated successfully to Global state ( Prepared ).
 Migration has reached a consistent state on all Domain Controllers.
 Succeeded.
 ```
 當某些網域控制站上的本機遷移狀態不符合全域遷移狀態時，此範例會顯示**drsrmig/GetMigrationState**命令的一般輸出。
 ```
  The following Domain Controllers are not in sync with Global state ( Prepared ):
  Domain Controller (Local Migration State)  DC type
  =========
  CONTOSO-DC2 ( start )  ReadOnly DC
  CONTOSO-DC3 ( Preparing )  Writable DC
  Migration has not yet reached a consistent state on all domain controllers
  State information might be stale due to AD latency.
 ```
若要建立全域物件和設定 DFS 複寫，以在未于遷移期間自動建立這些設定的網域控制站上，或在遺失這些設定的位置 AD DS 中使用，請輸入：
```
dfsrmig /createGlobalObjects
```
若要刪除名為 contoso-dc2 的唯讀網域控制站之 FRS 複寫的全域 AD DS 設定，如果這些設定並未被遷移程式自動刪除，請輸入：
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
若要刪除所有唯讀網域控制站之 FRS 複寫的全域 AD DS 設定，如果這些設定未由遷移程式自動刪除，請輸入：
```
dfsrmig /deleteRoNtfrsMember
```
若要刪除名為 contoso-dc2 的唯讀網域控制站之 DFS 複寫的全域 AD DS 設定，如果這些設定未由遷移程式自動刪除，請輸入：
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
若要刪除所有唯讀網域控制站之 DFS 複寫的全域 AD DS 設定，如果遷移程式未自動刪除這些設定，請輸入：
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](https://go.microsoft.com/fwlink/?LinkId=122056)

- [SYSvol 遷移系列：第2部分 drsrmig： SYSvol 遷移工具](https://go.microsoft.com/fwlink/?LinkID=121757)
