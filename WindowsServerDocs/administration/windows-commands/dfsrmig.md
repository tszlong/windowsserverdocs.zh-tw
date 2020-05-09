---
title: dfsrmig
description: Drsrmig 命令的參考主題，可將 SYSvol 複寫從 FRS 遷移至 DFS 複寫、提供有關遷移進度的資訊，以及修改 AD DS 物件以支援遷移。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0329bd5829941595f9e1938f68eefb17036c132
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992723"
---
# <a name="dfsrmig"></a>dfsrmig

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

DFS 複寫服務（drsrmig）的遷移工具會隨 DFS 複寫服務一起安裝。 此工具會將 SYSvol 複寫從檔案複寫服務（FRS）遷移至分散式檔案系統（DFS）複寫。 它也會提供有關遷移進度的資訊，並修改 Active Directory Domain Services （AD DS）物件以支援遷移。

## <a name="syntax"></a>語法

```
dfsrmig [/setglobalstate <state> | /getglobalstate | /getmigrationstate | /createglobalobjects |
/deleterontfrsmember [<read_only_domain_controller_name>] | /deleterodfsrmember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/setglobalstate <state>` | 將網域的全域遷移狀態設定為對應至*state*所指定之值的域。 您只能將全域遷移狀態設定為穩定狀態。 *狀態值*包括：<ul><li>**0** -開始狀態</li><li>**1** -備妥狀態</li><li>**2** -重新導向狀態</li><li>**3** -排除狀態</li></ul> |
| /getglobalstate | 在 PDC 模擬器上執行時，從 AD DS 資料庫的本機複本中，抓取網域的目前全域遷移狀態。 使用此選項來確認您已設定正確的全域遷移狀態。<p>**重要事項：** 您應該只在 PDC 模擬器上執行這個命令。 |
| /getmigrationstate | 抓取網域中所有網域控制站的目前本機遷移狀態，並判斷這些本機狀態是否符合目前的全域遷移狀態。 使用此選項來判斷所有網域控制站是否已達到全域遷移狀態。 |
| /createglobalobjects | 在 DFS 複寫使用的 AD DS 中，建立全域物件和設定。 您應該使用此選項手動建立物件和設定的情況如下：<ul><li>在**遷移期間，會升級新的唯讀網域控制站**。 如果新的唯讀網域控制站在移至**備**妥的狀態之後，但在遷移至 [已**刪除**] 狀態之前升級，則不會建立對應至新網域控制站的物件，因而導致複寫和遷移失敗。</li><li>**DFS 複寫服務的全域設定遺失或已刪除**。 如果網域控制站缺少這些設定，從**啟動**狀態到**備**妥狀態的遷移將會在**準備**轉換狀態時停止。 **注意：** 因為唯讀網域控制站的 DFS 複寫服務的全域 AD DS 設定是在 PDC 模擬器上建立的，所以這些設定必須先從 PDC 模擬器複寫到唯讀網域控制站，唯讀網域控制站上的 DFS 複寫服務才能使用這些設定。 由於 Active Directory 複寫延遲，因此此複寫可能需要一些時間才能執行。 |
| `/deleterontfrsmember [<read_only_domain_controller_name>]` | 刪除對應至指定唯讀網域控制站之 FRS 複寫的全域 AD DS 設定，或刪除所有唯讀網域控制站之 FRS 複寫的全域 AD DS 設定（如果未指定任何值） `<read_only_domain_controller_name>`。<p>在正常的遷移程式期間，您應該不需要使用此選項，因為 DFS 複寫服務會在從重新**導向**狀態遷移至 [已**刪除**] 狀態期間，自動刪除這些 AD DS 設定。 只有在唯讀網域控制站上的自動刪除失敗時，才使用此選項手動刪除 AD DS 設定，並在從重新**導向**狀態遷移至 [已**刪除**] 狀態期間，從 [重新輸入] 的長時間停止唯讀網域控制站。 |
| `/deleterodfsrmember [<read_only_domain_controller_name>]` | 刪除對應至指定唯讀網域控制站之 DFS 複寫的全域 AD DS 設定，或如果未指定的`<read_only_domain_controller_name>`值，則刪除所有唯讀網域控制站之 DFS 複寫的全域 AD DS 設定。<p>使用此選項只會在唯讀網域控制站上自動刪除失敗時，手動刪除 AD DS 設定，並在從備妥狀態復原至啟動狀態時，長時間停止唯讀網域控制站。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 使用`/setglobalstate <state>`命令，在 PDC 模擬器的 AD DS 中設定全域遷移狀態，以起始及控制遷移程式。 如果 PDC 模擬器無法使用，則此命令會失敗。

- 遷移至 [已**排除**] 狀態是無法復原的，而且無法復原，因此，只有在您完全認可使用 DFS 複寫進行 SYSvol 複寫時，才需要使用值**3**來表示*狀態*。

- 全域遷移狀態必須是穩定的遷移狀態。

- Active Directory 複寫會將全域狀態複寫到網域中的其他網域控制站，但因為複寫延遲，如果您在 PDC 模擬器以外的`dfsrmig /getglobalstate`網域控制站上執行，可能會出現不一致的情況。

- 的輸出`dsfrmig /getmigrationstate`指出是否已完成遷移至目前的全域狀態，並列出尚未達到目前全域遷移狀態的任何網域控制站的本機遷移狀態。 網域控制站的本機遷移狀態也可以包含尚未達到目前全域遷移狀態之網域控制站的轉換狀態。

- 唯讀網域控制站無法從 AD DS 刪除設定，PDC 模擬器會執行此作業，而變更最終會在 active directory 複寫適用的延遲之後，複寫到唯讀網域控制站。

- 只有在執行 Windows Server 網域功能等級的網域控制站上才支援**drsrmig**命令，因為從 FRS 到 DFS 複寫的 SYSvol 遷移僅適用于在該層級運作的網域控制站。

- 您可以在任何網域控制站上執行**drsrmig**命令，但建立或操作 AD DS 物件的作業只能在可讀寫功能的網域控制站（不是在唯讀網域控制站上）上使用。

## <a name="examples"></a>範例

若要將全域遷移狀態設定為 [備妥] （**1**），並起始遷移或從備妥的狀態復原，請輸入：

```
dfsrmig /setglobalstate 1
```

若要將全域遷移狀態設定為 [啟動] （**0**），並起始復原至 [啟動] 狀態，請輸入：

```
dfsrmig /setglobalstate 0
```

若要顯示全域遷移狀態，請輸入：

```
dfsrmig /getglobalstate
```

命令的`dfsrmig /getglobalstate`輸出：

```
Current DFSR global state: Prepared
Succeeded.
```

若要顯示所有網域控制站上的本機遷移狀態是否符合全域遷移狀態的相關資訊，以及本機狀態是否與全域狀態不相符的任何本機遷移狀態，請輸入：

```
dfsrmig /GetMigrationState
```

當所有域`dfsrmig /getmigrationstate`控制器上的本機遷移狀態符合全域遷移狀態時，從命令輸出：

```
All Domain Controllers have migrated successfully to Global state (Prepared).
Migration has reached a consistent state on all Domain Controllers.
Succeeded.
```

當某些域`dfsrmig /getmigrationstate`控制器上的本機遷移狀態不符合全域遷移狀態時，從命令輸出。

```
The following Domain Controllers are not in sync with Global state (Prepared):
Domain Controller (Local Migration State) DC type
=========
CONTOSO-DC2 (start) ReadOnly DC
CONTOSO-DC3 (Preparing) Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```

若要建立全域物件和設定 DFS 複寫，以在未于遷移期間自動建立這些設定的網域控制站上，或在遺失這些設定的位置 AD DS 中使用，請輸入：

```
dfsrmig /createglobalobjects
```

若要刪除名為 contoso-dc2 的唯讀網域控制站之 FRS 複寫的全域 AD DS 設定，如果這些設定並未被遷移程式自動刪除，請輸入：

```
dfsrmig /deleterontfrsmember contoso-dc2
```

若要刪除所有唯讀網域控制站之 FRS 複寫的全域 AD DS 設定，如果這些設定未由遷移程式自動刪除，請輸入：

```
dfsrmig /deleterontfrsmember
```

若要刪除名為 contoso-dc2 的唯讀網域控制站之 DFS 複寫的全域 AD DS 設定，如果這些設定未由遷移程式自動刪除，請輸入：

```
dfsrmig /deleterodfsrmember contoso-dc2
```

若要刪除所有唯讀網域控制站之 DFS 複寫的全域 AD DS 設定，如果遷移程式未自動刪除這些設定，請輸入：

```
dfsrmig /deleterodfsrmember
```

若要在命令提示字元中顯示說明：

```
dfsrmig
```

```
dfsrmig /?
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](https://go.microsoft.com/fwlink/?LinkId=122056)

- [SYSvol 遷移系列：第2部分 drsrmig： SYSvol 遷移工具](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-2-8211-dfsrmig-exe-the-sysvol/ba-p/423470)

- [Active Directory Domain Services](../../identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview.md)
