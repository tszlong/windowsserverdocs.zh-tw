---
title: ksetup:listrealmflags
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7db6caf4e63ea59fa40892679d3de0cfaca661e9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438022"
---
# <a name="ksetuplistrealmflags"></a>ksetup:listrealmflags



列出可以報告的可用領域旗標**ksetup**。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /listrealmflags
```

### <a name="parameters"></a>參數

None

## <a name="remarks"></a>備註

領域旗標會指定非 Windows 型的 Kerberos 領域的其他功能。 執行 Windows Server 2003、 Windows Server 2008 或 Windows Server 2008 R2 的電腦可用的非 Windows 型 Kerberos 伺服器來管理驗證，而不是使用執行 Windows Server 作業系統的網域。 這些系統加入 Kerberos 領域，而不是 Windows 網域。 此項目會建立領域的功能。 下表說明每個。

|值|領域旗標|描述|
|-----|----------|-----------|
|0xF|全部|設定所有領域旗標。|
|0x00|None|任何領域旗標的設定，以及任何其他的功能已啟用。|
|0x01|SendAddress|IP 位址會包含在 「 票證授權票證。|
|0x02|TcpSupported|在這個領域中支援傳輸控制通訊協定 (TCP) 和使用者資料包通訊協定 (UDP)。|
|0x04|委派|在這個領域中的每個人都是受信任可以委派。|
|0x08|NcSupported|此領域支援名稱標準化，可讓 DNS 和領域命名標準。|
|0x80|RC4|此領域支援 RC4 加密，以啟用跨領域信任，允許使用 TLS。|

在登錄中儲存領域旗標**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\** <em>領域名稱</em>。 此項目依預設不存在於登錄中。 您可以使用[Ksetup:addrealmflags](ksetup-addrealmflags.md)來填入登錄的命令。

## <a name="BKMK_Examples"></a>範例

列出此電腦上的已知的領域旗標：
```
ksetup /listrealmflags
```
設定可用的領域旗標**Ksetup**不知道在命令列輸入下列命令之一：
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)