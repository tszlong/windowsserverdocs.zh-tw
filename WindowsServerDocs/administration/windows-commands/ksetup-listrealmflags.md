---
title: ksetup listrealmflags
description: Ksetup listrealmflags 命令的參考主題，其中列出可由 ksetup 報告的可用領域旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c5ebbee7f937733286e0354eca1cf5524459e86
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817718"
---
# <a name="ksetup-listrealmflags"></a>ksetup listrealmflags

列出可由**ksetup**報告的可用領域旗標。

## <a name="syntax"></a>語法

```
ksetup /listrealmflags
```

### <a name="remarks"></a>備註

- 領域旗標會指定不是以 Windows 伺服器作業系統為基礎的 Kerberos 領域的其他功能。 執行 Windows Server 的電腦可以使用 Kerberos 伺服器來管理 Kerberos 領域中的驗證，而不是使用執行 Windows Server 作業系統的網域。 此專案會建立領域的功能，如下所示：

| 值 | 領域旗標 | 說明 |
| ----- | ---------- | ----------- |
| 0xF | 全部 | 所有領域旗標都已設定。 |
| 0x00 | None | 未設定領域旗標，且未啟用任何其他功能。 |
| 0x01 | sendaddress | IP 位址會包含在票證授權票證中。 |
| 0x02 | tcpsupported | 此領域支援傳輸控制通訊協定（TCP）和使用者資料包協定（UDP）。 |
| 0x04 | Delegate - 委派 | 此領域中的每個人都受信任，可進行委派。 |
| 0x08 | ncsupported | 此領域支援名稱標準化，其允許 DNS 和領域命名標準。 |
| 0x80 | rc4 | 此領域支援 RC4 加密來啟用跨領域信任，以允許使用 TLS。 |

- 領域旗標會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` 。 此專案預設不存在於登錄中。 您可以使用[ksetup addrealmflags 命令](ksetup-addrealmflags.md)來填入登錄。

## <a name="examples"></a>範例

若要列出這部電腦上的已知領域旗標，請輸入：

```
ksetup /listrealmflags
```

若要設定**ksetup**不知道的可用領域旗標，請輸入：

```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```

**或**

```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup addrealmflags 命令](ksetup-addrealmflags.md)

- [ksetup setrealmflags 命令](ksetup-setrealmflags.md)

- [ksetup delrealmflags 命令](ksetup-delrealmflags.md)
