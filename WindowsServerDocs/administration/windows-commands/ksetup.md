---
title: ksetup
description: '>ksetup 命令的參考文章，此命令會執行與設定和維護 Kerberos 通訊協定和金鑰發佈中心 (KDC) 支援 Kerberos 領域相關的工作。'
ms.topic: reference
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cb748e841854a096a0c0622658e781fd545f6c15
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627614"
---
# <a name="ksetup"></a>ksetup

執行與設定和維護 Kerberos 通訊協定和金鑰發佈中心 (KDC) 以支援 Kerberos 領域相關的工作。 具體而言，此命令是用來：

- 變更用來尋找 Kerberos 領域的電腦設定。 在非 Microsoft 以 Kerberos 為基礎的執行中，這項資訊通常會保留在 Krb5.keytab 的檔案中。 在 Windows Server 作業系統中，它會保留在登錄中。 您可以使用此工具來修改這些設定。 工作站會使用這些設定來尋找 Kerberos 領域和網域控制站，以尋找跨領域信任關係的 Kerberos 領域。

- 如果電腦不是 Windows 網域的成員，請將 Kerberos 安全性支援提供者 (SSP) 用來找出 Kerberos 領域之 KDC 的登錄機碼初始化。 設定之後，執行 Windows 作業系統的用戶端電腦使用者可以登入 Kerberos 領域中的帳戶。

- 搜尋登錄以取得使用者領域的功能變數名稱，然後藉由查詢 DNS 伺服器將名稱解析為 IP 位址。 Kerberos 通訊協定可以使用 DNS 來找出使用領域名稱的 Kdc，但必須特別設定才能執行這項作業。

## <a name="syntax"></a>語法

```
ksetup
[/setrealm <DNSdomainname>]
[/mapuser <principal> <account>]
[/addkdc <realmname> <KDCname>]
[/delkdc <realmname> <KDCname>]
[/addkpasswd <realmname> <KDCPasswordName>]
[/delkpasswd <realmname> <KDCPasswordName>]
[/server <servername>]
[/setcomputerpassword <password>]
[/removerealm <realmname>]
[/domain <domainname>]
[/changepassword <oldpassword> <newpassword>]
[/listrealmflags]
[/setrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/addrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/dumpstate]
[/addhosttorealmmap] <hostname> <realmname>]
[/delhosttorealmmap] <hostname> <realmname>]
[/setenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <domainname>
[/addenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <domainname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [ksetup setrealm](ksetup-setrealm.md) | 讓這部電腦成為 Kerberos 領域的成員。 |
| [ksetup addkdc](ksetup-addkdc.md) | 定義指定領域的 KDC 專案。 |
| [ksetup delkdc](ksetup-delkdc.md) | 刪除領域的 KDC 專案。 |
| [ksetup addkpasswd](ksetup-addkpasswd.md) | 新增領域的 kpasswd 伺服器位址。 |
| [ksetup delkpasswd](ksetup-delkpasswd.md) | 刪除領域的 kpasswd 伺服器位址。 |
| [ksetup server](ksetup-server.md) | 可讓您指定要套用變更的 Windows 電腦名稱稱。 |
| [ksetup setcomputerpassword](ksetup-setcomputerpassword.md) | 設定電腦網域帳戶 (或主機主體) 的密碼。 |
| [ksetup removerealm](ksetup-removerealm.md) | 從登錄中刪除指定領域的所有資訊。 |
| [ksetup domain](ksetup-domain.md) | 可讓您指定網域 (如果 [ `<domainname>` **/domain** ] 參數) 尚未設定該網域。 |
| [ksetup changepassword](ksetup-changepassword.md) | 可讓您使用 kpasswd 來變更登入使用者的密碼。 |
| [ksetup listrealmflags](ksetup-listrealmflags.md) | 列出 **>ksetup** 可以偵測到的可用領域旗標。 |
| [ksetup setrealmflags](ksetup-setrealmflags.md) | 設定特定領域的領域旗標。 |
| [ksetup addrealmflags](ksetup-addrealmflags.md) | 將其他領域旗標新增至領域。 |
| [ksetup delrealmflags](ksetup-delrealmflags.md) | 從領域刪除領域旗標。 |
| [ksetup dumpstate](ksetup-dumpstate.md) | 分析指定電腦上的 Kerberos 設定。 將主機新增至登錄的領域對應。 |
| [ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md) | 新增登錄值以將主機對應至 Kerberos 領域。 |
| [ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md) | 刪除將主機電腦對應至 Kerberos 領域的登錄值。 |
| [ksetup setenctypeattr](ksetup-setenctypeattr.md) | 設定網域的一或多個加密類型信任屬性。 |
| [ksetup getenctypeattr](ksetup-getenctypeattr.md) | 取得網域的加密類型信任屬性。 |
| [ksetup addenctypeattr](ksetup-addenctypeattr.md) | 將加密類型新增至網域的加密類型信任屬性。 |
| [ksetup delenctypeattr](ksetup-delenctypeattr.md) | 刪除網域的加密類型信任屬性。 |
| /? | 在命令提示字元顯示 [說明]。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)