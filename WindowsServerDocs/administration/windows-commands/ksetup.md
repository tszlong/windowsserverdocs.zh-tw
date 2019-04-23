---
title: ksetup
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0194cf81d069d7a5c1223f0a514d593e4870d397
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868839"
---
# <a name="ksetup"></a>ksetup



執行工作的相關設定和維護 Kerberos 通訊協定和的 「 金鑰發佈中心 (KDC) 」，以支援 Kerberos 領域，也不是 Windows 網域。 如需如何使用此命令的範例，請參閱 < 範例 > 一節中每個相關的子主題。

## <a name="syntax"></a>語法

```
ksetup 
[/setrealm <DNSDomainName>] 
[/mapuser <Principal> <Account>] 
[/addkdc <RealmName> <KDCName>] 
[/delkdc <RealmName> <KDCName>]
[/addkpasswd <RealmName> <KDCPasswordName>] 
[/delkpasswd <RealmName> <KDCPasswordName>]
[/server <ServerName>] 
[/setcomputerpassword <Password>]
[/removerealm <RealmName>]  
[/domain <DomainName>] 
[/changepassword <OldPassword> <NewPassword>] 
[/listrealmflags] 
[/setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/dumpstate]
[/addhosttorealmmap] <HostName> <RealmName>]  
[/delhosttorealmmap] <HostName> <RealmName>]  
[/setenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <DomainName>
[/addenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <DomainName>

```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|讓這部電腦的 Kerberos 領域的成員。|
|[Ksetup:mapuser](ksetup-mapuser.md)|對應至帳戶的 Kerberos 主體。|
|[Ksetup:addkdc](ksetup-addkdc.md)|定義指定領域的 KDC 項目。|
|[Ksetup:delkdc](ksetup-delkdc.md)|刪除領域的 KDC 項目。|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|新增領域 Kpasswd 伺服器位址。|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|刪除領域 Kpasswd 伺服器位址。|
|[Ksetup:server](ksetup-server.md)|可讓您指定要套用變更的 Windows 電腦的名稱。|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|設定電腦的網域帳戶 （或主機主體） 的密碼。|
|[Ksetup:removerealm](ksetup-removerealm.md)|從登錄中刪除指定的領域的所有資訊。|
|[Ksetup:domain](ksetup-domain.md)|可讓您指定網域 (如果\<DomainName > 未設定使用 **/domain**)。|
|[Ksetup:changepassword](ksetup-changepassword.md)|可讓您使用 Kpasswd 變更登入的使用者密碼。|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|列出可用領域旗標， **ksetup**可以偵測。|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|設定特定領域的領域旗標。|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|加入領域中的其他領域旗標。|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|刪除領域中的領域旗標。|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|分析指定的電腦上的 Kerberos 設定。 將登錄中的領域對應的主機。|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|將主機對應到 Kerberos 領域的登錄值。|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|刪除對應的主機電腦加入 Kerberos 領域的登錄值。|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|信任的網域屬性會設定一或多個加密類型。|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|取得網域的加密類型信任屬性。|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|加入網域的加密類型信任屬性中的加密類型。|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|刪除網域的加密類型信任屬性。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

**Ksetup**用來變更電腦設定，以尋找 Kerberos 領域。 在非 Microsoft Kerberos 為基礎的實作中，這項資訊通常會保留在 Krb5.conf 檔案中。 Windows Server 作業系統，它都會保持在登錄中。 您可以使用這項工具來修改這些設定。 若要尋找 Kerberos 領域的工作站和網域控制站，以找出跨領域信任關係的 Kerberos 領域，則會使用這些設定。

**Ksetup**初始化 Kerberos 安全性支援提供者 (SSP) 用來找出 KDC Kerberos 領域，如果電腦執行 Windows Server 2003、 Windows Server 2008 或 Windows Server 2008 R2，而且不是成員的 Windows 登錄機碼網域。 完成設定後，用戶端電腦執行 Windows 作業系統可以登入的使用者帳戶 Kerberos 領域中。

Kerberos 版本 5 通訊協定是在執行 Windows XP Professional、 Windows Vista 和 Windows 7 的電腦上的網路驗證的預設值。 Kerberos SSP 搜尋使用者的領域的網域名稱的登錄，並藉由查詢 DNS 伺服器，然後會將名稱解析為 IP 位址。 Kerberos 通訊協定可以使用 DNS 來找出 Kdc 只使用領域名稱，但它必須經過特別設定若要這樣做。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)