---
title: prnport
description: 瞭解如何建立、刪除和列出印表機埠。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9c162cef2a3ae2f3de1e891691572130ae68f93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372549"
---
# <a name="prnport"></a>prnport

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

除了顯示和變更埠設定之外，還會建立、刪除及列出標準 TCP/IP 印表機埠。

## <a name="syntax"></a>語法
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

## <a name="parameters"></a>Parameters

|          參數           |                                                                                                                                                                                                                                                                                                     描述                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a              |                                                                                                                                                                                                                                                                                       建立標準 TCP/IP 印表機埠。                                                                                                                                                                                                                                                                                        |
|              -d.ddd...e              |                                                                                                                                                                                                                                                                                       刪除標準 TCP/IP 印表機埠。                                                                                                                                                                                                                                                                                        |
|              -l              |                                                                                                                                                                                                                                                             列出電腦上使用 **-s**參數指定的所有標準 tcp/ip 印表機埠。                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            顯示標準 TCP/IP 印表機埠的設定。                                                                                                                                                                                                                                                                             |
|              -t              |                                                                                                                                                                                                                                                                           設定標準 TCP/IP 印表機埠的通訊埠設定。                                                                                                                                                                                                                                                                           |
|        -r \<PortName >        |                                                                                                                                                                                                                                                                                指定印表機所連接的埠。                                                                                                                                                                                                                                                                                 |
|       -s \<ServerName >       |                                                                                                                                                                                                                               指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。                                                                                                                                                                                                                                |
| -u \<UserName >-w <Password> |                                                                                                              指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。                                                                                                               |
|     -o {原始&#124; lpr}      |                                                                                                                                                                                                              指定埠使用的通訊協定： TCP 原始或 TCP lpr。 如果您使用 TCP raw，您可以選擇使用 **-n**參數來指定埠號碼。 預設埠號碼為9100。                                                                                                                                                                                                              |
|      -h \<Hostaddress >       |                                                                                                                                                                                                                                                                   指定您要設定埠的印表機（依 IP 位址）。                                                                                                                                                                                                                                                                    |
|       -q \<QueueName >        |                                                                                                                                                                                                                                                                                     指定 TCP 原始埠的佇列名稱。                                                                                                                                                                                                                                                                                     |
|       -n \<PortNumber >       |                                                                                                                                                                                                                                                                    指定 TCP 原始埠的通訊埠編號。 預設埠號碼為9100。                                                                                                                                                                                                                                                                    |
|        -m {e &#124; d}        |                                                                                                                                                                                                                                                       指定是否啟用 SNMP。 參數**e**會啟用 SNMP。 參數**d**會停用 SNMP。                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             指定 snmp 索引（如果已啟用 SNMP）。 如需詳細資訊，請參閱[rfc editor](https://go.microsoft.com/fwlink/?LinkId=569)網站的 rfc 1759。                                                                                                                                                                                                                              |
|     -y \<CommunityName >      |                                                                                                                                                                                                                                                                                指定 snmp 群體名稱（如果已啟用 SNMP）。                                                                                                                                                                                                                                                                                |
|       -2 {e &#124; -d}        | 指定是否針對 TCP lpr 埠啟用雙重線軸（也稱為 respooling）。 因為 TCP lpr 必須在傳送至印表機的控制檔案中包含正確的位元組計數，但通訊協定無法從本機列印提供者取得計數，所以需要雙重後臺緩衝。 因此，當檔案以多工緩衝處理至 TCP lpr 列印佇列時，它也會在 system32 目錄中做為暫存檔的暫存檔案。 TCP lpr 會決定暫存檔案的大小，並將大小傳送到執行 LPD 的伺服器。 參數**e**會啟用雙重後臺緩衝。 參數**d**會停用雙重線軸。 |
|              /?              |                                                                                                                                                                                                                                                                                         在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>備註
-   **Prnport**命令是位於%windir%\system32\ printing_Admin_Scripts\\<language> 目錄中的 Visual Basic 腳本。 若要使用此命令，請在命令提示字元中輸入**cscript** ，後面接著 prnport 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   如果您提供的資訊包含空格，請使用引號括住文字（例如 `"computer Name"`）。
-   TCP 原始通訊協定在 Windows 上是比 lpr 通訊協定更高的效能通訊協定。

## <a name="BKMK_examples"></a>典型
若要顯示伺服器上的所有標準 TCP/IP 列印埠 \\\Server1，請輸入：
```
cscript prnport -l -s Server1
```
若要刪除伺服器上的標準 TCP/IP 列印埠 \\\Server1 在10.2.3.4 連接到網路印表機，請輸入：
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
若要在伺服器上新增標準 TCP/IP 列印埠 \\\Server1 在10.2.3.4 連接到網路印表機，並在埠9100上使用 TCP 原始通訊協定，請輸入：
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
若要啟用 SNMP，請指定「公用」群體名稱，並在 伺服器 \\\Server1 所共用的網路印表機上，將 SNMP 索引 設定為1，然後輸入：
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
若要在10.2.3.4 連線到網路印表機的本機電腦上新增標準 TCP/IP 列印埠，並自動從印表機取得裝置設定，請輸入：
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[列印命令參考](print-command-reference.md)
