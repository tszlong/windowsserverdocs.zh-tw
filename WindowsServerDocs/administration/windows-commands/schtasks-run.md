---
title: schtasks 執行
description: Schtasks run 命令的參考文章，
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 9304e41c21899ce49bd6d4d4409ae3b47d3746aa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390705"
---
# <a name="schtasks-run"></a>schtasks 執行

立即啟動排程工作。 執行作業會忽略排程，但會使用在工作中儲存的程式檔位置、使用者帳戶和密碼來立即執行工作。 執行工作並不會影響工作排程，也不會變更下次排程工作的執行時間。

## <a name="syntax"></a>語法

```
schtasks /run /tn <taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /tn `<taskname>` | 識別要啟動的工作。 這是必要參數。 |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。 |
| u `[<domain>]` | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您在沒有 **/p**參數或 password 引數的情況下使用 **/u**參數，則 schtasks.exe 會提示您輸入密碼。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 使用此作業來測試您的工作。 如果工作未執行，請檢查工作排程器服務交易記錄檔， `<Systemroot>\SchedLgU.txt` 以取得錯誤。

- 若要從遠端執行工作，必須在遠端電腦上排程工作。 當您執行工作時，它只會在遠端電腦上執行。 若要確認工作是在遠端電腦上執行，請使用工作管理員或工作排程器服務交易記錄檔 `<Systemroot>\SchedLgU.txt` 。

## <a name="examples"></a>範例

若要啟動 *安全性腳本* 工作，請輸入：

```
schtasks /run /tn Security Script
```

若要在遠端電腦上啟動 *更新* 工作，請 >woodgrove-svr01，輸入：

```
schtasks /run /tn Update /s Svr01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [schtasks 變更命令](schtasks-change.md)

- [schtasks create 命令](schtasks-create.md)

- [schtasks delete 命令](schtasks-delete.md)

- [schtasks end 命令](schtasks-end.md)

- [schtasks 查詢命令](schtasks-query.md)