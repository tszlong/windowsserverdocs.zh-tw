---
title: ksetup： listrealmflags
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 265f988d85deb7602e91677626d207bc3a7873ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841491"
---
# <a name="ksetuplistrealmflags"></a>ksetup： listrealmflags



列出可由**ksetup**報告的可用領域旗標。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /listrealmflags
```

#### <a name="parameters"></a>參數

無

## <a name="remarks"></a>備註

領域旗標會指定非以 Windows 為基礎的 Kerberos 領域的其他功能。 執行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的電腦可以使用非 Windows 型 Kerberos 伺服器來管理驗證，而不是使用執行 Windows Server 作業系統的網域。 這些系統會參與 Kerberos 領域，而不是 Windows 網域。 此專案會建立領域的功能。 下表描述每個。

|值|領域旗標|描述|
|-----|----------|-----------|
|0xF|全部|所有領域旗標都已設定。|
|0x00|無|未設定領域旗標，且未啟用任何其他功能。|
|0x01|SendAddress|IP 位址會包含在票證授權票證中。|
|0x02|TcpSupported|此領域支援「傳輸控制通訊協定」（TCP）和「使用者資料包協定」（UDP）。|
|0x04|委派|此領域中的每個人都受信任，可進行委派。|
|0x08|NcSupported|此領域支援名稱標準化，其允許 DNS 和領域命名標準。|
|0x80|RC4|此領域支援 RC4 加密來啟用跨領域信任，以允許使用 TLS。|

領域旗標會儲存在登錄中， **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>領域名稱</em>。 此項目依預設不存在於登錄中。 您可以使用[Ksetup： addrealmflags](ksetup-addrealmflags.md)命令來填入登錄。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

列出此電腦上的已知領域旗標：
```
ksetup /listrealmflags
```
在命令列中輸入下列任一命令，以設定**Ksetup**不知道的可用領域旗標：
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>其他參考資料

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)