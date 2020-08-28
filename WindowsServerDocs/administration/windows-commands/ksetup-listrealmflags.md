---
title: ksetup listrealmflags
description: '>ksetup listrealmflags 命令的參考文章，其中列出可由 >ksetup 報告的可用領域旗標。'
ms.topic: reference
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7c522449053a18cdd1e2a9e533dbce5d6e9f17c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025472"
---
# <a name="ksetup-listrealmflags"></a>ksetup listrealmflags

列出 **>ksetup**可以報告的可用領域旗標。

## <a name="syntax"></a>語法

```
ksetup /listrealmflags
```

### <a name="remarks"></a>備註

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

## <a name="examples"></a>範例

若要列出這部電腦上的已知領域旗標，請輸入：

```
ksetup /listrealmflags
```

若要設定 **>ksetup** 不知道的可用領域旗標，請輸入：

```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```

**等於**

```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup addrealmflags 命令](ksetup-addrealmflags.md)

- [>ksetup setrealmflags 命令](ksetup-setrealmflags.md)

- [>ksetup delrealmflags 命令](ksetup-delrealmflags.md)
