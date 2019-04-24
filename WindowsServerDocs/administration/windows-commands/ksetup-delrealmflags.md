---
title: ksetup:delrealmflags
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9522d384c4f6996534b47e98dc8d8003bda504cd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877139"
---
# <a name="ksetupdelrealmflags"></a>ksetup:delrealmflags



移除指定的領域的領域旗標。  如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM，並列為預設領域時**Ksetup**執行。|

## <a name="remarks"></a>備註

領域旗標會指定不以 Windows Server 作業系統為基礎的 Kerberos 領域的其他功能。 執行 Windows Server 2003、 Windows Server 2008 或 Windows Server 2008 R2 的電腦可以使用 Kerberos 伺服器管理驗證，而不是使用執行 Windows Server 作業系統的網域，以及這些系統參與Kerberos 領域。 此項目會建立領域的功能。 下表說明每個。

|值|領域旗標|描述|
|-----|----------|-----------|
|0xF|全部|設定所有領域旗標。|
|0x00|None|任何領域旗標的設定，以及任何其他的功能已啟用。|
|0x01|SendAddress|IP 位址會包含在票證授與-票證。|
|0x02|TcpSupported|在這個領域中支援傳輸控制通訊協定 (TCP) 和使用者資料包通訊協定 (UDP)。|
|0x04|委派|在這個領域中的每個人都是受信任可以委派。|
|0x08|NcSupported|此領域支援名稱標準化，可讓 DNS 和領域命名標準。|
|0x80|RC4|此領域支援 RC4 加密，以啟用跨領域信任，允許使用 TLS。|

在登錄中儲存領域旗標**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** RealmName *。 此項目依預設不存在於登錄中。 您可以使用[Ksetup:addrealmflags](ksetup-addrealmflags.md)來填入登錄的命令。

您可以看到哪些領域旗標可供使用且設定檢視的輸出**ksetup**或是**ksetup /dumpstate**。

## <a name="BKMK_Examples"></a>範例

列出可用的領域旗標，為 CONTOSO 的領域：
```
Ksetup /listrealmflags
```
移除目前在集合中的兩個旗標：
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
執行**ksetup**命令來確認領域旗標設定檢視輸出，並尋找**領域的旗標 =**。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [命令列語法關鍵](command-line-syntax-key.md)