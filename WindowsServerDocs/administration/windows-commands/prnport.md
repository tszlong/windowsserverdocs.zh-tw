---
title: prnport
description: 了解如何建立、 刪除和列出印表機連接埠。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5a7c48dcfa4db548d27722ee316eda797fd1ef2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442137"
---
# <a name="prnport"></a>prnport

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立、 刪除和列出標準的 TCP/IP 印表機連接埠，除了顯示和變更連接埠組態。

## <a name="syntax"></a>語法
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

## <a name="parameters"></a>參數

|          參數           |                                                                                                                                                                                                                                                                                                     描述                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a              |                                                                                                                                                                                                                                                                                       建立標準 TCP/IP 印表機連接埠。                                                                                                                                                                                                                                                                                        |
|              -d              |                                                                                                                                                                                                                                                                                       刪除標準 TCP/IP 印表機連接埠。                                                                                                                                                                                                                                                                                        |
|              -l              |                                                                                                                                                                                                                                                             列出所有標準的 TCP/IP 印表機連接埠，使用指定的電腦上 **-s**參數。                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            顯示標準 TCP/IP 印表機連接埠的組態。                                                                                                                                                                                                                                                                             |
|              -t              |                                                                                                                                                                                                                                                                           設定標準 TCP/IP 印表機連接埠的連接埠設定。                                                                                                                                                                                                                                                                           |
|        -r \<PortName >        |                                                                                                                                                                                                                                                                                指定印表機所連接的連接埠。                                                                                                                                                                                                                                                                                 |
|       -s\<伺服器名稱 >       |                                                                                                                                                                                                                               指定裝載的印表機，您想要管理遠端電腦的名稱。 如果您沒有指定的電腦，則會使用本機電腦。                                                                                                                                                                                                                                |
| -u \<UserName> -w <Password> |                                                                                                              指定具有連線至裝載的印表機，您想要管理之電腦的權限的帳戶。 目標電腦的本機 Administrators 群組的所有成員都有這些權限，但可以也對其他使用者授與權限。 如果您未指定帳戶，您必須登入具有可使用該指令的這些權限的帳戶。                                                                                                               |
|     -o {raw &#124; lpr}      |                                                                                                                                                                                                              指定的通訊協定連接埠所使用：未經處理的 TCP 或 TCP lpr。 如果您使用原始的 TCP，您可以選擇性地使用指定的連接埠號碼 **-n**參數。 預設連接埠號碼是 9100。                                                                                                                                                                                                              |
|      -h\<主機位址 >       |                                                                                                                                                                                                                                                                   指定您要設定的連接埠的印表機 （藉由 IP 位址）。                                                                                                                                                                                                                                                                    |
|       -q \<QueueName>        |                                                                                                                                                                                                                                                                                     指定 TCP 原始通訊埠的佇列名稱。                                                                                                                                                                                                                                                                                     |
|       -n\<連接埠號碼 >       |                                                                                                                                                                                                                                                                    指定 TCP 原始通訊埠的連接埠號碼。 預設連接埠號碼是 9100。                                                                                                                                                                                                                                                                    |
|        -m{e &#124; d}        |                                                                                                                                                                                                                                                       指定是否啟用 （snmp）。 參數**e**啟用 SNMP。 參數**d** SNMP 會停用。                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             指定的 SNMP 索引，如果已啟用 SNMP。 如需詳細資訊，請參閱在 Rfc 1759 [Rfc editor 網站](https://go.microsoft.com/fwlink/?LinkId=569)。                                                                                                                                                                                                                              |
|     -y \<CommunityName>      |                                                                                                                                                                                                                                                                                如果在啟用 （snmp），請指定 SNMP 群體名稱。                                                                                                                                                                                                                                                                                |
|       -2{e &#124; -d}        | 指定是否啟用雙精度浮點多工緩衝處理 （也稱為重複），以供 TCP lpr 連接埠。 雙精度浮點多工緩衝處理是必要的因為 TCP lpr 必須傳送至印表機，控制檔案中包含正確的位元組計數，但是通訊協定無法從本機列印提供者取得的計數。 因此，當檔案多工緩衝處理至 TCP lpr 列印佇列時，它也多工緩衝處理的 system32 目錄中的暫存檔案。 TCP lpr 決定暫存檔案的大小，並將大小傳送至執行 LPD 的伺服器。 參數**e**可讓雙精度浮點多工緩衝處理。 參數**d**停用雙精度浮點多工緩衝處理。 |
|              /?              |                                                                                                                                                                                                                                                                                         在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>備註
-   **Prnport**命令是 Visual Basic 指令碼位於 %WINdir%\System32\printing_Admin_Scripts\\ <language>目錄。 若要使用這個命令中，在命令提示字元中，輸入**cscript**後面 prnport 檔案或將目錄變更為適當的資料夾完整路徑。 例如:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   如果您提供的資訊包含空格，請使用引號括住的文字 (例如`"computer Name"`)。
-   TCP 原始通訊協定是較高的效能與通訊協定在 Windows 上 lpr 通訊協定。

## <a name="BKMK_examples"></a>範例
若要顯示伺服器上的所有標準 TCP/IP 列印通訊埠\\\Server1 中，輸入：
```
cscript prnport -l -s Server1
```
若要刪除標準 TCP/IP 列印伺服器的連接埠\\\Server1 連接到網路印表機在 10.2.3.4，型別：
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
若要在伺服器上新增標準 TCP/IP 列印連接埠\\\Server1 可連接至在 10.2.3.4 網路印表機，以及連接埠 9100，型別上使用 TCP 原始通訊協定：
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
若要啟用 SNMP，指定的 「 公用 」 的群體名稱，然後在 10.2.3.4 伺服器共用網路印表機上，設定為 1 的 SNMP 索引\\\Server1 中，輸入：
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
若要連接到網路印表機在 10.2.3.4 並自動從印表機取得裝置設定的本機電腦上新增標準 TCP/IP 列印連接埠，請輸入：
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[列印命令參考資料](print-command-reference.md)
