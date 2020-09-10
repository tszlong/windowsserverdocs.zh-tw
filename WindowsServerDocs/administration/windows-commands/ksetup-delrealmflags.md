---
title: ksetup delrealmflags
description: '>ksetup delrealmflags 命令的參考文章，此命令會從指定的領域移除領域旗標。'
ms.topic: reference
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 90ebf697ae19cd31b45dc7744ba29f5dc0b9f597
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640051"
---
# <a name="ksetup-delrealmflags"></a>ksetup delrealmflags

從指定的領域移除領域旗標。

## <a name="syntax"></a>語法

```
ksetup /delrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，並在 **>ksetup**執行時列為預設領域或**領域 =** 。 |

#### <a name="remarks"></a>備註

- 領域旗標會指定 Kerberos 領域的其他功能，而不是以 Windows Server 作業系統為基礎。 執行 Windows Server 的電腦可以使用 Kerberos 伺服器來管理 Kerberos 領域中的驗證，而不是使用執行 Windows Server 作業系統的網域。 此專案會建立領域的功能，如下所示：

| 值 | 領域旗標 | 描述 |
| ----- | ---------- | ----------- |
| 0xF | 全部 | 所有領域旗標都已設定。 |
| 0x00 | 無 | 未設定任何領域旗標，也未啟用任何其他功能。 |
| 0x01 | sendaddress | IP 位址將包含在票證授權票證內。 |
| 0x02 | tcpsupported | 此領域支援傳輸控制通訊協定 (TCP) 和使用者資料包協定 (UDP) 。 |
| 0x04 | Delegate - 委派 | 此領域中的每個人都受信任，可進行委派。 |
| 0x08 | ncsupported | 此領域支援名稱標準化，可允許 DNS 和領域命名標準。 |
| 0x80 | rc4 | 此領域支援 RC4 加密來啟用跨領域信任，以允許使用 TLS。 |

- 領域旗標儲存在登錄下的登錄中 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` 。 依預設，此專案不存在於登錄中。 您可以使用 [>ksetup addrealmflags 命令](ksetup-addrealmflags.md) 來填入登錄。

- 您可以藉由觀看 **>ksetup** 或的輸出來查看可用的和設定領域旗標 `ksetup /dumpstate` 。

### <a name="examples"></a>範例

若要列出適用于領域 CONTOSO 的可用領域旗標，請輸入：

```
ksetup /listrealmflags
```

若要移除目前在集合中的兩個旗標，請輸入：

```
ksetup /delrealmflags CONTOSO ncsupported delegate
```

若要確認已移除領域旗標，請輸入 `ksetup` 並查看輸出，並尋找文字、 **領域旗標 =**。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup listrealmflags 命令](ksetup-listrealmflags.md)

- [>ksetup setrealmflags 命令](ksetup-setrealmflags.md)

- [>ksetup addrealmflags 命令](ksetup-addrealmflags.md)

- [>ksetup dumpstate 命令](ksetup-dumpstate.md)
