---
title: dfsrmig
description: Drsrmig 命令的參考文章，可將 SYSvol 複寫從 FRS 遷移至 DFS 複寫、提供遷移進度的相關資訊，以及修改 AD DS 物件以支援遷移。
ms.topic: reference
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5c97132a078f1459890062fa1f99a58b6718bd84
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635028"
---
# <a name="dfsrmig"></a>dfsrmig

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

DFS 複寫服務 dfsrmig.exe 的遷移工具會與 DFS 複寫服務一起安裝。 此工具會將 SYSvol 複寫從) 的檔案複寫服務 (FRS 複製到分散式檔案系統 (DFS) 複寫。 它也提供遷移進度的相關資訊，並修改 Active Directory Domain Services (AD DS) 物件來支援遷移。

## <a name="syntax"></a>語法

```
dfsrmig [/setglobalstate <state> | /getglobalstate | /getmigrationstate | /createglobalobjects |
/deleterontfrsmember [<read_only_domain_controller_name>] | /deleterodfsrmember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/setglobalstate <state>` | 將網域的全域遷移狀態設定為對應至 [ *狀態*] 所指定之值的一個。 您只能將全域遷移狀態設定為穩定的狀態。 這些 *狀態值* 包括：<ul><li>**0** -開始狀態</li><li>**1** -備妥狀態</li><li>**2** -重新導向狀態</li><li>**3** -已消除狀態</li></ul> |
| /getglobalstate | 在 PDC 模擬器上執行時，從 AD DS 資料庫的本機複本中，抓取網域的目前全域遷移狀態。 您可以使用此選項來確認您已設定正確的全域遷移狀態。<p>**重要事項：** 您應該只在 PDC 模擬器上執行此命令。 |
| /getmigrationstate | 抓取網域中所有網域控制站目前的本機遷移狀態，並判斷這些本機狀態是否符合目前的全域遷移狀態。 您可以使用此選項來判斷所有網域控制站是否已達到全域遷移狀態。 |
| /createglobalobjects | 在 DFS 複寫使用的 AD DS 中建立全域物件和設定。 您應該使用此選項來手動建立物件和設定的情況如下：<ul><li>在**遷移期間，會升級新的唯讀網域控制站**。 如果在移至已 **備** 妥的狀態之後，網域中的新唯讀網域控制站已升級，但在遷移至 [已 **刪除** ] 狀態之前，則不會建立對應至新網域控制站的物件，因而導致複寫失敗。</li><li>**DFS 複寫服務的全域設定遺失或已刪除**。 如果網域控制站缺少這些設定，從 [ **開始** ] 狀態遷移至 [ **備** 妥] 狀態將會在 **準備** 轉換狀態時停止。 **注意：** 因為唯讀網域控制站的 DFS 複寫服務的全域 AD DS 設定是在 PDC 模擬器上建立的，所以這些設定必須先從 PDC 模擬器複寫到唯讀網域控制站，才能讓唯讀網域控制站上的 DFS 複寫服務可以使用這些設定。 由於 Active Directory 複寫延遲，因此可能需要一些時間才能進行此複寫。 |
| `/deleterontfrsmember [<read_only_domain_controller_name>]` | 刪除對應至指定唯讀網域控制站之 FRS 複寫的全域 AD DS 設定，或者，如果未指定任何值，則會刪除所有唯讀網域控制站的 FRS 複寫全域 AD DS 設定 `<read_only_domain_controller_name>` 。<p>在正常的遷移程式期間，您不應該使用這個選項，因為 DFS 複寫服務會在從重新 **導向** 的狀態遷移至已 **刪除的狀態** 時，自動刪除這些 AD DS 設定。 只有在唯讀網域控制站上的自動刪除失敗時，才使用此選項手動刪除 AD DS 設定，並在從重新 **導向** 的狀態遷移至已 **刪除** 的狀態時，讓長輸入的唯讀網域控制站停止執行。 |
| `/deleterodfsrmember [<read_only_domain_controller_name>]` | 刪除對應至指定唯讀網域控制站之 DFS 複寫的全域 AD DS 設定，如果未指定任何值，則刪除所有唯讀網域控制站 DFS 複寫的全域 AD DS 設定 `<read_only_domain_controller_name>` 。<p>只有在唯讀網域控制站上的自動刪除失敗時，您可以使用此選項手動刪除 AD DS 設定，並在從備妥的狀態復原到啟動狀態時，讓唯讀網域控制站的時間更長。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 使用 `/setglobalstate <state>` 命令來設定 PDC 模擬器 AD DS 中的全域遷移狀態，以起始及控制遷移程式。 如果 PDC 模擬器無法使用，則此命令會失敗。

- 遷移至已 **刪除** 的狀態是無法回復的動作，因此無法復原，因此只有當您完全認可為使用 DFS 複寫進行 SYSvol 複寫時，才將值設為 **3** 的 *狀態* 。

- 全域遷移狀態必須是穩定的遷移狀態。

- Active Directory 複寫會將全域狀態複寫到網域中的其他網域控制站，但由於複寫延遲的不同，如果您在 `dfsrmig /getglobalstate` PDC 模擬器以外的網域控制站上執行，就會發生不一致的情況。

- 的輸出 `dsfrmig /getmigrationstate` 會指出是否已完成遷移至目前的全域狀態，並列出尚未到達目前全域遷移狀態之任何網域控制站的本機遷移狀態。 網域控制站的本機遷移狀態也可以包含尚未達到目前全域遷移狀態之網域控制站的轉換狀態。

- 唯讀網域控制站無法從 AD DS 刪除設定，PDC 模擬器會執行此操作，而且變更最後會在適用于 active directory 複寫的延遲後複寫到唯讀網域控制站。

- 只有在執行于 Windows Server 網域功能等級的網域控制站上，才支援 **drsrmig** 命令，因為只有在該層級運作的網域控制站可以從 FRS 遷移至 DFS 複寫。

- 您可以在任何網域控制站上執行 **drsrmig** 命令，但建立或操作 AD DS 物件的作業只允許在具有讀寫功能的網域控制站上， (不能使用唯讀網域控制站) 。

## <a name="examples"></a>範例

若要將全域遷移狀態設定為準備 (**1**) 並起始遷移或從備妥狀態復原，請輸入：

```
dfsrmig /setglobalstate 1
```

若要將全域遷移狀態設定為啟動 (**0**) 並起始復原至開始狀態，請輸入：

```
dfsrmig /setglobalstate 0
```

若要顯示全域遷移狀態，請輸入：

```
dfsrmig /getglobalstate
```

命令的輸出 `dfsrmig /getglobalstate` ：

```
Current DFSR global state: Prepared
Succeeded.
```

若要顯示所有網域控制站上的本機遷移狀態是否符合全域遷移狀態的相關資訊，以及本機狀態不符合全域狀態的本機遷移狀態，請輸入：

```
dfsrmig /GetMigrationState
```

`dfsrmig /getmigrationstate`當所有網域控制站上的本機遷移狀態符合全域遷移狀態時，命令的輸出：

```
All Domain Controllers have migrated successfully to Global state (Prepared).
Migration has reached a consistent state on all Domain Controllers.
Succeeded.
```

`dfsrmig /getmigrationstate`當某些網域控制站上的本機遷移狀態不符合全域遷移狀態時，命令的輸出。

```
The following Domain Controllers are not in sync with Global state (Prepared):
Domain Controller (Local Migration State) DC type
=========
CONTOSO-DC2 (start) ReadOnly DC
CONTOSO-DC3 (Preparing) Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```

若要建立全域物件和設定，讓 DFS 複寫使用於未在遷移期間自動建立這些設定的網域控制站上 AD DS，或遺漏這些設定，請輸入：

```
dfsrmig /createglobalobjects
```

若要針對名為 contoso-dc2 的唯讀網域控制站刪除適用于 FRS 複寫的全域 AD DS 設定，如果這些設定未被遷移程式自動刪除，請輸入：

```
dfsrmig /deleterontfrsmember contoso-dc2
```

若要刪除所有唯讀網域控制站的 FRS 複寫全域 AD DS 設定，如果遷移程式未自動刪除這些設定，請輸入：

```
dfsrmig /deleterontfrsmember
```

若要刪除名為 contoso-dc2 之唯讀網域控制站的全域 AD DS DFS 複寫設定，如果遷移程式未自動刪除這些設定，請輸入：

```
dfsrmig /deleterodfsrmember contoso-dc2
```

若要刪除所有唯讀網域控制站的全域 AD DS 設定 DFS 複寫如果這些設定未由遷移程式自動刪除，請輸入：

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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](https://go.microsoft.com/fwlink/?LinkId=122056)

- [SYSvol 遷移系列：第2部分 dfsrmig.exe： SYSvol 遷移工具](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-2-8211-dfsrmig-exe-the-sysvol/ba-p/423470)

- [Active Directory Domain Services](../../identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview.md)
