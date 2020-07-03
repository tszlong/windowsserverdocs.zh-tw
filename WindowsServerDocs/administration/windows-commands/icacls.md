---
title: icacls
description: Icacls 命令的參考文章，它會顯示或修改指定檔案上的任意存取控制清單（DACL），並將預存的 Dacl 套用到指定目錄中的檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 386e008ef7095cbef8d84b33682b494d8d6c9c52
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924521"
---
# <a name="icacls"></a>icacls

顯示或修改指定檔案上的判別存取控制清單 (DACL)，及套用預存的 DACL 到指定目錄中的檔案。

> [!NOTE]
> 此命令會取代已被取代的[cacls 命令](cacls.md)。

## <a name="syntax"></a>語法

```
icacls <filename> [/grant[:r] <sid>:<perm>[...]] [/deny <sid>:<perm>[...]] [/remove[:g|:d]] <sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<policy>[...]]
icacls <directory> [/substitute <sidold> <sidnew> [...]] [/restore <aclfile> [/c] [/l] [/q]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<filename>` | 指定要顯示其 Dacl 的檔案。 |
| `<directory>` | 指定要顯示其 Dacl 的目錄。 |
| /t | 對目前目錄及其子目錄中的所有指定檔案執行作業。 |
| /C | 即使有任何檔案錯誤，仍繼續操作。 仍然會顯示錯誤訊息。 |
| /l | 在符號連結（而不是其目的地）上執行作業。 |
| /q | 隱藏成功訊息。 |
| [/save `<ACLfile>`一起/c/l[/q]] | 將所有相符檔案的 Dacl 儲存至*ACLfile* ，以供稍後用於 **/restore**。 |
| [/setowner `<username>`一起/c/l[/q]] | 將所有相符檔案的擁有者變更為指定的使用者。 |
| [/findsid `<sid>`一起/c/l[/q]] | 尋找包含 DACL 的所有相符檔案，明確提及指定的安全識別碼（SID）。 |
| [/verify [/t] [/c] [/l] [/q]] | 尋找 Acl 不是標準的所有檔案，或長度與 ACE 不一致的檔案（存取控制專案）計數。 |
| [/reset [/t] [/c] [/l] [/q]] | 以預設繼承的 Acl 取代所有相符檔案的 Acl。 |
| [/grant [： r] \<sid> ： <perm> [...]] | 授與指定的使用者存取權限。 許可權會取代先前授與的明確許可權。<p>未新增 **： r**，表示許可權會加入至任何先前授與的明確許可權。 |
| [/deny \<sid> ： <perm> [...]] | 明確拒絕指定的使用者存取權限。 系統會為指定的許可權新增明確的拒絕 ACE，並移除任何明確授與中的相同許可權。 |
| [/remove `[:g | :d]]` `<sid>`[...]一起/c/l一起 | 從 DACL 中移除所有出現的指定 SID。 此命令也可以使用：<ul><li>**： g** -移除所有已授與指定 SID 的許可權。</li><li>**:d** -移除指定之 SID 的所有已拒絕許可權。 |
| [/setintegritylevel [（CI）（OI）] `<Level>:<Policy>`[...]] | 將完整性 ACE 明確新增至所有相符的檔案。 層級可以指定為：<ul><li>**l** -低</li><li>**m**-中型</li><li>**h** -高</li></ul>完整性 ACE 的繼承選項可能在層級之前，而且僅適用于目錄。 |
| [/substitute `<sidold> <sidnew>`[...]] | 以新的 SID （*sidnew*）取代現有的 sid （*sidold*）。 需要搭配參數使用 `<directory>` 。 |
| /restore `<ACLfile>` [/c] [/l] [/q] | 將儲存的 Dacl 從套用 `<ACLfile>` 至指定目錄中的檔案。 需要搭配參數使用 `<directory>` 。 |
| /inheritancelevel:`[e | d | r]` | 設定繼承層級，可以是：<ul><li>**e** -啟用繼承</li><li>**d** -停用繼承並複製 ace</li><li>**r** -移除所有繼承的 ace</li></ul> |

## <a name="remarks"></a>備註

- Sid 可以是數位或易記名稱格式。 如果您使用數值格式，請將萬用字元 **&#42;** 貼到 SID 的開頭。

- 此命令會保留 ACE 專案的標準順序，如下所示：

    - 明確拒絕

    -  明確授與

    - 繼承拒絕

    - 繼承的授與

- 此 `<perm>` 選項是可使用下列其中一種形式指定的許可權遮罩：

    - 一系列的簡單許可權：

      - **F** -完整存取

      - **M**-修改存取權

      - **RX** -讀取和執行存取

      - **R** -唯讀存取

      - **W** -僅限寫入的存取

    - 以括弧括住的特定許可權清單（以逗號分隔）：

      - **D** -刪除

      - **RC** -讀取控制

      - **WDAC** -寫入 DAC

      - **WO** -寫入擁有者

      - **S** -同步處理

      - **AS**存取系統安全性

      - **MA** -允許的最大值

      - **GR** -一般讀取

      - **GW** -一般寫入

      - **GE** -一般執行

      - **GA** -全部通用

      - **RD** -讀取資料/清單目錄

      - **WD** -寫入資料/新增檔案

      - **AD** -附加資料/新增子目錄

      - **反應**-讀取擴充屬性

      - **WEA** -寫入擴充屬性

      - **X** -執行/遍歷

      - **DC** -刪除子系

      - **RA** -讀取屬性

      - **WA** -寫入屬性

  - 繼承許可權可能會在任一 `<perm>` 形式之前，而且只會套用至目錄：

      - **（OI）** -物件繼承

      - **（CI）** -容器繼承

      - **（IO）** -僅繼承

      - **（NP）** -不要傳播繼承

## <a name="examples"></a>範例

若要將 C：\Windows 目錄及其子目錄中所有檔案的 Dacl 儲存至 ACLFile 檔案，請輸入：

```
icacls c:\windows\* /save aclfile /t
```

若要還原 ACLFile 中存在於 C：\Windows 目錄及其子目錄中的每個檔案的 Dacl，請輸入：

```
icacls c:\windows\ /restore aclfile
```

若要將使用者 User1 刪除和寫入 DAC 許可權授與名為 Test1 的檔案，請輸入：

```
icacls test1 /grant User1:(d,wdac)
```

若要將 SID S-1-1-0 Delete 和 Write DAC 許可權所定義的使用者授與名為 Test2 的檔案，請輸入：

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
