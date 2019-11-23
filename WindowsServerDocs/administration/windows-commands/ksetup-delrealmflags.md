---
title: ksetup： delrealmflags
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8e2e67d7af4fdd31b79ad633c9df844483bb1ea3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375099"
---
# <a name="ksetupdelrealmflags"></a>ksetup： delrealmflags



從指定的領域移除領域旗標。  如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM，並在執行**Ksetup**時列為預設領域。|

## <a name="remarks"></a>備註

領域旗標會指定不是以 Windows 伺服器作業系統為基礎的 Kerberos 領域的其他功能。 執行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的電腦可以使用 Kerberos 伺服器來管理驗證，而不是使用執行 Windows Server 作業系統的網域，而這些系統會參與Kerberos 領域。 此專案會建立領域的功能。 下表描述每個。

|值|領域旗標|描述|
|-----|----------|-----------|
|0xF|全部|所有領域旗標都已設定。|
|0x00|無|未設定領域旗標，且未啟用任何其他功能。|
|0x01|SendAddress|IP 位址會包含在票證授權票證中。|
|0x02|TcpSupported|此領域支援傳輸控制通訊協定（TCP）和使用者資料包協定（UDP）。|
|0x04|委派|此領域中的每個人都受信任，可進行委派。|
|0x08|NcSupported|此領域支援名稱標準化，其允許 DNS 和領域命名標準。|
|0x80|RC4|此領域支援 RC4 加密來啟用跨領域信任，以允許使用 TLS。|

領域旗標會儲存在登錄中的**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>RealmName</em>中。 此項目依預設不存在於登錄中。 您可以使用[Ksetup： addrealmflags](ksetup-addrealmflags.md)命令來填入登錄。

您可以查看**ksetup**或**ksetup/dumpstate**的輸出，以查看有哪些領域旗標可供使用和設定。

## <a name="BKMK_Examples"></a>典型

列出領域 CONTOSO 的可用領域旗標：
```
Ksetup /listrealmflags
```
移除目前在集合中的兩個旗標：
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
執行**ksetup**命令，以確認是否已透過查看輸出並尋找**領域旗標 =** 來設定領域旗標。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [命令列語法關鍵](command-line-syntax-key.md)