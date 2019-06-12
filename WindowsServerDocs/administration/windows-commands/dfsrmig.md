---
title: dfsrmig
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0fb68fc962602969a18145c67ad6c361515db6a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436148"
---
# <a name="dfsrmig"></a>dfsrmig

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

`dfsrmig`命令將 SYSvol 複寫從檔案複寫服務 (FRS) 移轉至分散式檔案系統 (DFS) 複寫、 提供移轉時，進度資訊，並修改 active directory 網域服務 (AD DS) 物件支援移轉。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)本文件稍後的章節。
## <a name="syntax"></a>語法
```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```
## <a name="parameters"></a>參數

|                         參數                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  /SetGlobalState <state>                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     設定網域的所需的通用移轉狀態，對應至所指定的值表示*狀態*。<br /><br />若要繼續透過移轉或復原程序，請使用此命令有效的狀態間循環。 此選項可讓您起始，並藉由設定 AD DS 中的全域的移轉狀態，PDC 模擬器上控制移轉程序。 如果找不到 PDC 模擬器，此命令將會失敗。<br /><br /> 您只可以設定全域的移轉狀態穩定的狀態。 有效值*狀態*，因此， **0**開始狀態，如**1**備妥狀態**2**重新導向狀態和**3**已排除的狀態。<br /><br />移轉已排除狀態無法復原，且不能從該狀態復原，因此，請使用值為**3** for*狀態*只時完全 committd 使用 sysvol 的 DFS 複寫複寫。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                      /GetGlobalState                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       從 AD DS 資料庫、 在 PDC 模擬器上執行時的本機副本擷取網域的目前全域的移轉狀態。<br /><br />使用此選項，以確認您設定正確的全域移轉狀態。 只有穩定的移轉狀態可以是全域的移轉狀態，因此結果的**drsrmig**命令使用報表 **/GetGlobalState**選項對應至的狀態，您可以設定具有 **/SetGlobalState**選項。<br /><br />您應該執行**drsrmig**命令搭配 **/GetGlobalState**只能在 PDC 模擬器上的選項。 active directory 複寫會將全域狀態複寫至其他網域控制站在網域中，但複寫延遲可能會導致不一致的情況，如果您執行**drsrmig**命令搭配 **/GetGlobalState**以外 PDC 模擬器網域控制站上的選項。 若要檢查網域控制站以外的 PDC 模擬器的本機的移轉狀態，請使用 **/GetMigrationState**選項。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|                    /GetMigrationState                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     擷取在網域中的所有網域控制站的目前本機的移轉狀態，並判斷這些本機狀態是否符合目前的全域的移轉狀態。<br /><br />使用此選項，來判斷是否所有網域控制站已都達到全域的移轉狀態。 輸出**dsfrmig**命令，當您使用 **/GetMigrationState**選項指出是否為目前的全域狀態移轉已完成，而且它會列出任何本機的移轉狀態尚未到達目前的全域的移轉狀態的網域控制站。 網域控制站的本機的移轉狀態可以包含尚未到達目前的全域的移轉狀態的網域控制站的轉換狀態。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                   /createGlobalObjects                    | DFS 複寫會使用的 AD DS 中建立的全域物件和設定。<br /><br />您應該不需要使用此選項，在一般的移轉過程中，因為 DFS 複寫服務會自動建立下列的 AD DS 物件及設定期間從 start 狀態移轉至已準備就緒的狀態。 若要手動建立這些物件和設定，在下列情況下使用此選項：<br /><br />-   **新的唯讀網域控制站升級在移轉期間**。 DFS 複寫服務會自動建立的 AD DS 物件和設定 DFS 複寫期間從 start 狀態移轉至已準備就緒的狀態。 如果新的唯讀網域控制站升級網域中完成這項轉換之後，但移轉前已排除的狀態，然後對應至新啟動的唯讀網域控制站的物件不會建立在 AD DS 中導致複寫和移轉失敗。<br />-在此情況下，您可以執行**drsrmig**命令與 **/createGlobalObjects**以手動方式在還沒有它們的任何唯讀網域控制站上建立物件的選項。 執行此命令不會影響已有的物件和設定 DFS 複寫服務的網域控制站。<br />-   **DFS 複寫服務的全域設定已遺失或已刪除**。 如果這些設定已遺失特定網域控制站，備妥狀態從 start 狀態移轉會停止在網域控制站正在準備轉換狀態。 在此情況下，您可以使用**drsrmig**命令搭配 **/createGlobalObjects**來手動建立設定的選項。 **注意：** PDC 模擬器上建立的全域的 AD DS 設定 DFS 複寫服務的唯讀網域控制站，因為這些設定都需要從 PDC 模擬器，DFS 複寫服務之前複寫到唯讀網域控制站上唯讀網域控制站可以使用這些設定。 作用中的目錄複寫延遲，因為此複寫可能需要一些時間進行。 |
| /deleteRoNtfrsMember [<read_only_domain_controller_name>] |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    刪除全域 AD DS FRS 複寫設定對應至指定的唯讀網域控制站，或刪除全域 AD DS 設定的所有唯讀網域控制站的 FRS 複寫，如果未不指定任何值，如*read_only_domain_controller_ip_address*。<br /><br />您應該不需要使用此選項，在一般的移轉過程中，因為 DFS 複寫服務會自動刪除這些 AD DS 設定期間從重新導向狀態移轉至已排除的狀態。 唯讀網域控制站無法從 AD DS 中刪除這些設定，因為 PDC 模擬器執行這項作業，並變更最後會複寫到唯讀網域控制站之後適用的 active directory 複寫延遲。<br /><br />您可以使用此選項以手動方式刪除 AD DS 設定，只有在自動刪除無法在唯讀網域控制站上，並從重新導向狀態已排除狀態在移轉期間停止長的輸入法的唯讀網域控制站時。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| /deleteRoDfsrMember [<read_only_domain_controller_name>]  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          刪除全域 AD DS 設定 DFS 複寫會對應至指定的唯讀網域控制站，或刪除所有的唯讀網域控制站的 DFS 複寫全域 AD DS 設定，如果未不指定任何值，如*read_only_domain_controller_ip_address*。<br /><br />使用此選項，以手動方式刪除 AD DS 設定，自動刪除失敗的唯讀網域控制站上，並停止長時間的唯讀網域控制站時，只回復移轉已準備就緒的狀態從 start 狀態的時間。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                            /?                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              在命令提示字元顯示說明。 相當於執行**drsrmig**不使用任何選項。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

## <a name="remarks"></a>備註
- dfsrmig.exe，DFS 複寫服務，移轉工具會安裝 DFS 複寫服務。
  適用於新的 Windows Server 2008 伺服器，Dcpromo.exe 會安裝並啟動 DFS 複寫服務，當您將升級為網域控制站的電腦。 當您將伺服器從 Windows Server 2003 升級到 Windows Server 2008，升級程序會安裝並啟動 DFS 複寫服務。 您不需要安裝 DFS 複寫角色服務安裝和啟動 DFS 複寫服務。
- **Drsrmig**工具僅適用於執行 Windows Server 2008 網域功能等級，在網域控制站因為從 FRS 到 DFS 複寫的 SYSvol 移轉時才可運作的網域控制站上 Windows Server 2008 網域功能等級。
- 您可以執行**drsrmig**任何網域控制站，但建立或操作 AD DS 的作業上的命令物件才允許 （不在唯讀網域控制站） 上的讀寫支援的網域控制站上。
- 執行**drsrmig**不使用任何選項，請在命令提示字元中顯示說明。
  ## <a name="BKMK_examples"></a>範例
  將全域的移轉狀態設定為已備妥 (**1**)，並起始移轉至或從備妥狀態，也就是類型的復原：
  ```
  dfsrmig /SetGlobalState 1
  ```
  若要設定全域的移轉狀態，才能開始 (**0**)，並起始回復開始狀態，而型別：
  ```
  dfsrmig /SetGlobalState 0
  ```
  若要顯示的通用的移轉狀態，請輸入：
  ```
  dfsrmig /GetGlobalState
  ```
  此範例示範典型輸出**drsrmig /GetGlobalState**命令。
  ```
  Current DFSR global state:  Prepared 
  Succeeded.
  ```
  若要顯示所有網域控制站上的本機的移轉狀態是否符合全球的移轉狀態和本機的移轉狀態的任何網域控制站的本機狀態不符合全域狀態的相關資訊，請輸入：
  ```
  dfsrmig /GetMigrationState
  ```
  此範例示範典型輸出**drsrmig /GetMigrationState**命令時的所有網域控制站上的本機的移轉狀態符合全域的移轉狀態。
  ```
  All Domain Controllers have migrated successfully to Global state ( Prepared ).
  Migration has reached a consistent state on all Domain Controllers.
  Succeeded.
  ```
  此範例示範典型輸出**drsrmig /GetMigrationState**命令時在部分網域控制站上的本機的移轉狀態不相符的通用的移轉狀態。
  ```
  The following Domain Controllers are not in sync with Global state ( Prepared ):
Domain Controller (Local Migration State)   DC type
=========
CONTOSO-DC2 ( start )   ReadOnly DC
CONTOSO-DC3 ( Preparing )   Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```
其中這些設定已不會自動移轉期間建立的網域控制站上的 AD DS 中建立的全域物件和設定 DFS 複寫使用，或符合這些設定時遺失，請輸入：
```
dfsrmig /createGlobalObjects
```
若要刪除名為 contoso-dc2，如果移轉程序會自動刪除未刪除這些設定的唯讀網域控制站的 FRS 複寫的全域 AD DS 設定，請輸入：
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
若要刪除全域 AD DS 設定的所有唯讀網域控制站的 FRS 複寫，如果這些設定不會自動刪除的移轉程序，請輸入：
```
dfsrmig /deleteRoNtfrsMember
```
若要刪除 DFS 複寫的全域 AD DS 設定名為 contoso-dc2，如果這些設定不會自動刪除的移轉程序的唯讀網域控制站，請輸入：
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
若要刪除所有的唯讀網域控制站的 DFS 複寫全域 AD DS 設定，如果這些設定不會自動刪除的移轉程序，請輸入：
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](https://go.microsoft.com/fwlink/?LinkId=122056)

[SYSvol 移轉系列：第 2 部分 dfsrmig.exe:SYSvol 移轉工具](https://go.microsoft.com/fwlink/?LinkID=121757)
