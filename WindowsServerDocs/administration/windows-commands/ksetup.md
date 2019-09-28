---
title: ksetup
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 265f67bff65794938485472a41064837551c7699
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374803"
---
# <a name="ksetup"></a>ksetup



執行與設定和維護 Kerberos 通訊協定和金鑰發佈中心（KDC）相關的工作，以支援不是 Windows 網域的 Kerberos 領域。 如需如何使用此命令的範例，請參閱每個相關子主題中的範例一節。

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
|[Ksetup:setrealm](ksetup-setrealm.md)|使此電腦成為 Kerberos 領域的成員。|
|[Ksetup:mapuser](ksetup-mapuser.md)|將 Kerberos 主體對應至帳戶。|
|[Ksetup:addkdc](ksetup-addkdc.md)|定義給定領域的 KDC 專案。|
|[Ksetup:delkdc](ksetup-delkdc.md)|刪除領域的 KDC 專案。|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|新增領域的 Kpasswd 伺服器位址。|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|刪除領域的 Kpasswd 伺服器位址。|
|[Ksetup:server](ksetup-server.md)|可讓您指定要套用變更的 Windows 電腦名稱稱。|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|設定電腦網域帳戶（或主機主體）的密碼。|
|[Ksetup:removerealm](ksetup-removerealm.md)|從登錄中刪除指定領域的所有資訊。|
|[Ksetup:domain](ksetup-domain.md)|可讓您指定網域（如果尚未使用 [ **/domain**] 設定 @no__t 0DomainName >）。|
|[Ksetup:changepassword](ksetup-changepassword.md)|可讓您使用 Kpasswd 來變更登入使用者的密碼。|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|列出**ksetup**可以偵測到的可用領域旗標。|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|設定特定領域的領域旗標。|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|將其他領域旗標新增至領域。|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|從領域刪除領域旗標。|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|分析指定電腦上的 Kerberos 設定。 將主機新增至登錄的領域對應。|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|新增登錄值，以將主機對應至 Kerberos 領域。|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|刪除將主機電腦對應到 Kerberos 領域的登錄值。|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|設定網域的一或多個加密類型信任屬性。|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|取得網域的加密類型信任屬性。|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|將加密類型新增至網域的加密類型信任屬性。|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|刪除網域的加密類型信任屬性。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

**Ksetup**是用來變更用來尋找 Kerberos 領域的電腦設定。 在以非 Microsoft Kerberos 為基礎的內部部署中，這項資訊通常會保留在 Krb5 檔案中。 在 Windows Server 作業系統中，它會保留在登錄中。 您可以使用此工具來修改這些設定。 工作站會使用這些設定來尋找 Kerberos 領域和網域控制站，以找出跨領域信任關係的 Kerberos 領域。

如果電腦執行的是 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2，而且不是 Windows 的成員，則**Ksetup**會初始化 Kerberos 安全性支援提供者（SSP）用來尋找 KERBEROS 領域 KDC 的登錄機碼domain. 設定之後，執行 Windows 作業系統的用戶端電腦使用者就可以登入 Kerberos 領域中的帳戶。

Kerberos 第5版通訊協定是執行 Windows XP Professional、Windows Vista 和 Windows 7 之電腦上的網路驗證預設值。 Kerberos SSP 會在登錄中搜尋使用者領域的功能變數名稱，然後藉由查詢 DNS 伺服器，將名稱解析成 IP 位址。 Kerberos 通訊協定可以使用 DNS 來尋找 Kdc，方法是只使用領域名稱，但必須特別設定才能執行這項操作。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)