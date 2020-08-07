---
title: prnport
description: Prnport 命令的參考文章，它會建立、刪除和列出標準 TCP/IP 印表機埠，以及顯示和變更埠設定。
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8aa1729c84b54393a6154dd5fc4ba5a5e6834fc
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884699"
---
# <a name="prnport"></a>prnport

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

除了顯示和變更埠設定之外，還會建立、刪除及列出標準 TCP/IP 印表機埠。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入**cscript** ，後面接著 prnport 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如：`cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport`。

## <a name="syntax"></a>語法

```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <portname>] [-s <Servername>] [-u <Username>] [-w <password>] [-o {raw | lpr}] [-h <Hostaddress>] [-q <Queuename>] [-n <portnumber>] -m{e | d} [-i <SNMPindex>] [-y <communityname>] -2{e | -d}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -a | 建立標準 TCP/IP 印表機埠。 |
| -d | 刪除標準 TCP/IP 印表機埠。 |
| -l | 列出 **-s**參數所指定之電腦上的所有標準 tcp/ip 印表機埠。 |
| -g | 顯示標準 TCP/IP 印表機埠的設定。 |
| -t | 設定標準 TCP/IP 印表機埠的通訊埠設定。 |
| -r`<portname>` | 指定印表機所連接的埠。 |
| -s`<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -u `<Username>` -w`<password>` | 指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。 |
| -o`{raw|lpr}` | 指定埠使用的通訊協定： TCP 原始或 TCP lpr。 TCP 原始通訊協定在 Windows 上是比 lpr 通訊協定更高的效能通訊協定。 如果您使用 TCP raw，您可以選擇使用 **-n**參數來指定埠號碼。 預設埠號碼為9100。 |
| -h`<Hostaddress>` | 指定要設定埠的印表機)  (的 IP 位址。 |
| -q`<Queuename>` | 指定 TCP 原始埠的佇列名稱。 |
| -n`<portnumber>` | 指定 TCP 原始埠的通訊埠編號。 預設埠號碼為9100。 |
| -m`{e|d}` | 指定是否啟用 SNMP。 參數**e**會啟用 SNMP。 參數**d**會停用 SNMP。 |
| -i`<SNMPindex` | 指定 snmp 索引（如果已啟用 SNMP）。 如需詳細資訊，請參閱[rfc editor 網站](https://www.ietf.org/rfc/rfc1759.txt?number=1759)的**rfc 1759** 。 |
| -y`<communityname>` | 指定 snmp 群體名稱（如果已啟用 SNMP）。 |
| -2`{e|-d}` | 指定是否針對 TCP lpr 埠啟用雙緩衝緩衝 (也稱為 respooling) 。 因為 TCP lpr 必須在傳送至印表機的控制檔案中包含正確的位元組計數，但通訊協定無法從本機列印提供者取得計數，所以需要雙重後臺緩衝。 因此，當檔案以多工緩衝處理至 TCP lpr 列印佇列時，它也會在 system32 目錄中做為暫存檔的暫存檔案。 TCP lpr 會決定暫存檔案的大小，並將大小傳送到執行 LPD 的伺服器。 參數**e**會啟用雙重後臺緩衝。 參數**d**會停用雙重線軸。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字 (例如「電腦名稱稱」 ) 。

### <a name="examples"></a>範例

若要顯示伺服器 Server1 上的所有標準 TCP/IP 列印埠 \\ ，請輸入：

```
cscript prnport -l -s Server1
```

若要在連接到網路印表機的伺服器 Server1 上刪除標準 TCP/IP 列印埠 \\ ，請在10.2.3.4 上輸入：

```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```

若要在伺服器 Server1 上新增標準 TCP/IP 列印埠， \\ 並在10.2.3.4 連線到網路印表機，並在埠9100上使用 TCP 原始通訊協定，請輸入：

```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```

若要啟用 SNMP，請指定「公用」群體名稱，並在伺服器 Server1 共用的10.2.3.4 網路印表機上，將 SNMP 索引設定為 1 \\ ，並輸入：

```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```

若要在10.2.3.4 連線到網路印表機的本機電腦上新增標準 TCP/IP 列印埠，並自動從印表機取得裝置設定，請輸入：

```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
