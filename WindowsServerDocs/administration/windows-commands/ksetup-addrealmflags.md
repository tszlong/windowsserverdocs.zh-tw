---
title: ksetup： addrealmflags
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21ab18620f0ed25cfb0d50bb1ab1a5d92caa9c54
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841811"
---
# <a name="ksetupaddrealmflags"></a>ksetup： addrealmflags



將其他領域旗標新增至指定的領域。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|領域名稱|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>備註

領域旗標會指定不是以 Windows 伺服器作業系統為基礎的 Kerberos 領域的其他功能。 執行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的電腦可以使用 Kerberos 伺服器來管理驗證，而不是使用執行 Windows Server 作業系統的網域，而這些系統會參與 Kerberos 領域。 此專案會建立領域的功能。 下表描述每個。

|值|領域旗標|描述|
|-----|----------|-----------|
|0xF|全部|所有領域旗標都已設定。|
|0x00|無|未設定領域旗標，且未啟用任何其他功能。|
|0x01|SendAddress|IP 位址會包含在票證授權票證中。|
|0x02|TcpSupported|此領域支援傳輸控制通訊協定（TCP）和使用者資料包協定（UDP）。|
|0x04|委派|此領域中的每個人都受信任，可進行委派。|
|0x08|NcSupported|此領域支援名稱標準化，其允許 DNS 和領域命名標準。|
|0x80|RC4|此領域支援 RC4 加密來啟用跨領域信任，以允許使用 TLS。|

領域旗標會儲存在登錄中的**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>領域名稱</em>底下。 此項目依預設不存在於登錄中。 您可以使用[Ksetup： addrealmflags](ksetup-addrealmflags.md)命令來填入登錄。

您可以查看 ksetup 或 ksetup/dumpstate. 的輸出，以查看可用和設定的領域旗標

## <a name="examples"></a><a name=BKMK_Examples></a>典型

列出領域 CONTOSO 的可用領域旗標：
```
Ksetup /listrealmflags
```
將兩個旗標設定為 CONTOSO 領域：
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
新增另一個目前不在此集合中的旗標：
```
ksetup /addrealmflags CONTOSO SendAddress
```
執行**ksetup**命令，以確認是否已透過查看輸出並尋找**領域旗標 =** 來設定領域旗標。

## <a name="additional-references"></a>其他參考資料

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)