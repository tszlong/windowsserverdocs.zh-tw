---
title: tlntadmn
description: Tlntadmn 命令的參考文章，此命令會管理本機或遠端電腦，以執行 telnet 伺服器服務。
ms.topic: reference
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9a4de6903d5a9979f45677176d023c694e20cdfd
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156444"
---
# <a name="tlntadmn"></a>tlntadmn

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理正在執行 telnet 伺服器服務的本機或遠端電腦。 如果使用時不含參數， **tlntadmn** 會顯示目前的伺服器設定。

此命令會要求您使用系統管理認證登入本機電腦。 若要管理遠端電腦，您也必須提供遠端電腦的系統管理認證。 若要這麼做，您可以使用具有本機電腦和遠端電腦的系統管理認證的帳戶登入本機電腦。 如果您無法使用這個方法，您可以使用 **-u** 和 **-p** 參數來提供遠端電腦的系統管理認證。

## <a name="syntax"></a>語法

```
tlntadmn [<computername>] [-u <username>] [-p <password>] [{start | stop | pause | continue}] [-s {<sessionID> | all}] [-k {<sessionID> | all}] [-m {<sessionID> | all}  <message>] [config [dom = <domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <connections>] [port = <number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<computername>` | 指定要連接的伺服器名稱。 預設是本機電腦。 |
| -u `<username> -p <password>` | 指定您想要管理的遠端伺服器的系統管理認證。 如果您想要管理您未使用系統管理認證登入的遠端伺服器，則需要此參數。 |
| start | 啟動 telnet 伺服器服務。 |
| stop | 停止 telnet 伺服器服務 |
| pause | 暫停 telnet 伺服器服務。 將不會接受任何新的連接。 |
| continue | 繼續 telnet 伺服器服務。 |
| -s `{<sessionID> | all}` | 顯示主動式 telnet 會話。 |
| -k `{<sessionID> | all}` | 結束 telnet 會話。 輸入會話識別碼以結束特定會話，或輸入全部來結束所有會話。 |
| -m `{<sessionID> | all}  <message>` | 將訊息傳送至一或多個會話。 輸入要傳送訊息至特定會話的會話識別碼，或輸入 all 以將訊息傳送到所有會話。 輸入您想要在引號之間傳送的訊息。 |
| config dom = `<domain>` | 設定伺服器的預設網域。 |
| config ctrlakeymap = `{yes | no}` | 指定您是否希望 telnet 伺服器解讀 CTRL + A 做為 ALT。 輸入 **yes** 以對應快速鍵，或輸入 **no** 以防止對應。 |
| config timeout = `<hh>:<mm>:<ss>` | 設定以小時、分鐘和秒為單位的超時時間。 |
| config timeoutactive = `{yes | no}` | 啟用閒置會話超時。 |
| config maxfail = `<attempts>` | 設定中斷連線前的失敗登入嘗試次數上限。 |
| config maxconn = `<connections>` | 設定連接的最大數目。 |
| config 埠 = `<number>` | 設定 telnet 埠。 您必須使用小於1024的整數來指定埠。 |
| config sec `{+ | -}NTLM {+ | -}passwd` | 指定是否要使用 NTLM、密碼或兩者來驗證登入嘗試。 若要使用特定類型的驗證，請在 **+** 驗證類型之前輸入加號 () 。 若要避免使用特定類型的驗證，請在 **-** 該類型的驗證之前輸入負號 () 。 |
| 配置模式 = `{console | stream}` | 指定作業的模式。 |
| -? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要將閒置會話超時設定為30分鐘，請輸入：

```
tlntadmn config timeout=0:30:0
```

若要顯示主動式 telnet 會話，請輸入：

```
tlntadmn -s
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [telnet 操作指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753164(v=ws.10))
