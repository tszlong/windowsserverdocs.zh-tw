---
title: ksetup delrealmflags
description: Ksetup delrealmflags 命令的參考文章，它會從指定的領域移除領域旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d3c81d1b034f6c53c33271c1c9e61a0fc5d4893
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929170"
---
# <a name="ksetup-delrealmflags"></a>ksetup delrealmflags

從指定的領域移除領域旗標。

## <a name="syntax"></a>語法

```
ksetup /delrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，且在執行**ksetup**時列為預設領域或**領域 =** 。 |

#### <a name="remarks"></a>備註

- 領域旗標會指定不是以 Windows 伺服器作業系統為基礎的 Kerberos 領域的其他功能。 執行 Windows Server 的電腦可以使用 Kerberos 伺服器來管理 Kerberos 領域中的驗證，而不是使用執行 Windows Server 作業系統的網域。 此專案會建立領域的功能，如下所示：

| 值 | 領域旗標 | Description |
| ----- | ---------- | ----------- |
| 0xF | 全部 | 所有領域旗標都已設定。 |
| 0x00 | None | 未設定領域旗標，且未啟用任何其他功能。 |
| 0x01 | sendaddress | IP 位址會包含在票證授權票證中。 |
| 0x02 | tcpsupported | 此領域支援傳輸控制通訊協定（TCP）和使用者資料包協定（UDP）。 |
| 0x04 | Delegate - 委派 | 此領域中的每個人都受信任，可進行委派。 |
| 0x08 | ncsupported | 此領域支援名稱標準化，其允許 DNS 和領域命名標準。 |
| 0x80 | rc4 | 此領域支援 RC4 加密來啟用跨領域信任，以允許使用 TLS。 |

- 領域旗標會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` 。 此專案預設不存在於登錄中。 您可以使用[ksetup addrealmflags 命令](ksetup-addrealmflags.md)來填入登錄。

- 您可以查看**ksetup**或的輸出，以查看可用和設定領域旗標 `ksetup /dumpstate` 。

### <a name="examples"></a>範例

若要列出領域 CONTOSO 的可用領域旗標，請輸入：

```
ksetup /listrealmflags
```

若要移除目前在集合中的兩個旗標，請輸入：

```
ksetup /delrealmflags CONTOSO ncsupported delegate
```

若要確認已移除領域旗標，請輸入， `ksetup` 然後再查看輸出，尋找 [**領域旗標 =**] 文字。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup listrealmflags 命令](ksetup-listrealmflags.md)

- [ksetup setrealmflags 命令](ksetup-setrealmflags.md)

- [ksetup addrealmflags 命令](ksetup-addrealmflags.md)

- [ksetup dumpstate 命令](ksetup-dumpstate.md)
