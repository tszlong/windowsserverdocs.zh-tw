---
title: winrs
description: Winrs 的參考文章，可讓您從遠端系統管理和執行程式。
ms.topic: reference
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a2a92a20a924e36686fb555b90da4fa7d6ef67fd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641119"
---
# <a name="winrs"></a>winrs

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows 遠端系統管理可讓您從遠端系統管理和執行程式。
## <a name="syntax"></a>語法
```
winrs [/<parameter>[:<value>]] <command>
```
#### <a name="parameters"></a>參數

|           參數            |                                                                                                                                                                                    描述                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /remote\<endpoint>       |                                                                                          使用 NetBIOS 名稱或標準連接來指定目標端點：<p>-   <url>: [\<transport>://]\<target>[:\<port>]<p>如果未指定，則會使用 **/r： localhost** 。                                                                                          |
|          /unencrypted          | 指定將不會加密遠端 shell 的訊息。 當您使用 **ipsec**或強制執行實體安全性時，這項功能很有用。<p>根據預設，訊息會使用 Kerberos 或 NTLM 金鑰進行加密。<p>選取 HTTPS 傳輸時，會忽略這個命令列選項。 |
|     /username\<username>      |                                                                                在命令列上指定使用者名稱。<p>如果未指定，此工具將會使用協商驗證或提示輸入名稱。<p>如果指定了 **/username** ，也必須指定 **/password** 。                                                                                 |
|     /password\<password>      |                                                                           指定命令列上的密碼。<p>如果未指定 **/password** ，但 **/username** 為，則工具會提示您輸入密碼。<p>如果指定 **/password** ，也必須指定 **/username** 。                                                                            |
|      /timeout\<seconds>       |                                                                                                                                                                             這個選項已被取代。                                                                                                                                                                             |
|       /目錄\<path>       |                                                                                            指定遠端 shell 的起始目錄。<p>如果未指定，則會在環境變數 **% USERPROFILE%** 所定義的使用者主目錄中啟動遠端 shell。                                                                                             |
| 環境\<string>=<value> |                                                                          指定當 shell 啟動時要設定的單一環境變數，這可讓您變更 shell 的預設環境。<p>必須使用這個參數的多個來指定多個環境變數。                                                                          |
|            /noecho             |                                                                                                    指定應停用 echo。 這可能是必要的，以確保使用者不會在本機顯示遠端提示的回應。<p>預設會開啟 echo。                                                                                                    |
|           /noprofile           |                                              指定不應該載入使用者的設定檔。<p>根據預設，伺服器會嘗試載入使用者設定檔。<p>如果遠端使用者並非目標系統上的本機系統管理員，則需要此選項 (預設會導致) 錯誤。                                               |
|         /allowdelegate         |                                                                                                                  指定使用者的認證可以用來存取遠端共用，例如，在與目標端點不同的電腦上找到。                                                                                                                   |
|          /compression          |                                                                           開啟壓縮。  遠端電腦上的舊版安裝可能不支援壓縮，因此預設為關閉。<p>預設設定為關閉，因為遠端電腦上的舊版安裝可能不支援壓縮。                                                                           |
|            /usessl             |                                                                                                               使用遠端端點時，請使用 SSL 連線。  指定此項，而不是傳輸 **HTTPs：** 將會使用預設的 **WinRM** 預設通訊埠。                                                                                                                |
|               /?               |                                                                                                                                                                        在命令提示字元顯示說明。                                                                                                                                                                        |

## <a name="remarks"></a>備註
-   所有命令列選項都接受簡短形式或完整格式。 例如， **/r** 和 **/remote** 都是有效的。
-   若要終止 **/remote** 命令，使用者可以輸入 **ctrl-c** 或 **ctrl-break**，這會傳送至遠端 shell。 第二個 **ctrl-c** 會強制終止 **winrs.exe**。
-   若要管理主動遠端 shell 或 winrs 設定，請使用 WinRM 工具。  管理使用中 shell 的 URI 別名是 **shell/cmd**。  Winrs 設定的 URI 別名是 **winrm/config/winrs**。

## <a name="examples"></a>範例
```
winrs /r:https://contoso.com command
```
```
winrs /r:contoso.com /usessl command
```
```
winrs /r:myserver command
```
```
winrs /r:http://127.0.0.1 command
```
```
winrs /r:http://169.51.2.101:80 /unencrypted command
```
```
winrs /r:https://[::FFFF:129.144.52.38] command
```
```
winrs /r:http://[1080:0:0:0:8:800:200C:417A]:80 command
```
```
winrs /r:https://contoso.com /t:600 /u:administrator /p:$%fgh7 ipconfig
```
```
winrs /r:myserver /env:path=^%path^%;c:\tools /env:TEMP=d:\temp config.cmd
```
```
winrs /r:myserver netdom join myserver /domain:testdomain /userd:johns /passwordd:$%fgh789
```
```
winrs /r:myserver /ad /u:administrator /p:$%fgh7 dir \\anotherserver\share
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

