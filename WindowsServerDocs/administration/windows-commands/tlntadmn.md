---
title: tlntadmn
description: Tlntadmn 的參考文章，可管理本機或遠端電腦，並執行 telnet 伺服器服務。
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dee63c0233bab5341dae724ccd9779c3917054c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87897150"
---
# <a name="tlntadmn"></a>tlntadmn

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理執行 telnet 伺服器服務的本機或遠端電腦。

## <a name="syntax"></a>語法
```
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]
```
#### <a name="parameters"></a>參數

|                   參數                    |                                                                                                                                                       描述                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                \<computerName>                 |                                                                                                                    指定要連接的伺服器名稱。 預設是本機電腦。                                                                                                                    |
|         -u \<UserName> -p\<Password>          |                                                指定您要管理之遠端伺服器的系統管理認證。 如果您想要管理您未使用系統管理認證登入的遠端伺服器，則需要此參數。                                                |
|                     開始                      |                                                                                                                                            啟動 telnet 伺服器服務。                                                                                                                                             |
|                      stop                      |                                                                                                                                             停止 telnet 伺服器服務                                                                                                                                              |
|                     pause                      |                                                                                                                          暫停 telnet 伺服器服務。 不接受任何新的連接。                                                                                                                          |
|                    continue                    |                                                                                                                                            繼續 telnet 伺服器服務。                                                                                                                                            |
|          -s { \<SessionID> &#124; 全部}          |                                                                                                                                             顯示作用中的 telnet 會話。                                                                                                                                             |
|          -k { \<SessionID> &#124; 全部}          |                                                                                                        結束 telnet 會話。 輸入會話識別碼以結束特定會話，或輸入 all 以結束所有會話。                                                                                                         |
|    -m { \<SessionID> &#124; 全部}<Message>     |                                                   將訊息傳送至一或多個會話。 輸入會話識別碼以傳送訊息至特定會話，或輸入 all 以傳送訊息至所有會話。 輸入您想要在引號之間傳送的訊息。                                                   |
|             設定 dom =\<Domain>             |                                                                                                                                      設定伺服器的預設網域。                                                                                                                                       |
|      config ctrlakeymap = {yes &#124; 否}      |                                                                                     指定您是否要讓 telnet 伺服器解讀 CTRL + A 做為 ALT。 輸入 **[是]** 以對應快速鍵，或輸入 [**否**] 以防止對應。                                                                                     |
|       config timeout = \<hh> ： \<mm> ：\<ss>       |                                                                                                                                 設定以小時、分鐘和秒為單位的超時時間。                                                                                                                                 |
|     config timeoutactive = {yes &#124; 否      |                                                                                                                                            啟用閒置會話超時。                                                                                                                                             |
|          config maxfail =\<attempts>          |                                                                                                                          設定中斷連線前的失敗登入嘗試次數上限。                                                                                                                          |
|        config maxconn =\<Connections>         |                                                                                                                                         設定連接的最大數目。                                                                                                                                          |
|            config port = < \Number of>             |                                                                                                                    設定 telnet 通訊埠。 您必須使用小於1024的整數來指定埠。                                                                                                                    |
| config sec {+ &#124;-} NTLM {+ &#124;-} passwd | 指定您要使用 NTLM 還是密碼，或兩者都用來驗證登入嘗試。 若要使用特定類型的驗證，請在 **+** 該類型的驗證之前輸入加號 () 。 若要避免使用特定類型的驗證，請在 **-** 該類型的驗證之前輸入減號 () 。 |
|     config mode = {console &#124; stream}      |                                                                                                                                             指定操作的模式。                                                                                                                                             |
|                       -?                       |                                                                                                                                           在命令提示字元顯示說明。                                                                                                                                           |

## <a name="remarks"></a>備註
-   若要顯示伺服器設定，請輸入**tlntadmn** ，而不需要任何參數。
-   若要使用**tlntadmn**命令，您必須使用系統管理認證登入本機電腦。 若要管理遠端電腦，您也必須提供遠端電腦的系統管理認證。 若要這麼做，您可以使用同時具有本機電腦和遠端電腦系統管理認證的帳戶登入本機電腦。 如果您無法使用這個方法，您可以使用 **-u**和 **-p**參數來提供遠端電腦的系統管理認證。

## <a name="examples"></a>範例
將閒置會話超時設定為30分鐘。
```
tlntadmn config timeout=0:30:0
```
顯示作用中的 telnet 會話。
```
tlntadmn -s
```

## <a name="additional-references"></a>其他參考資料
-   [telnet 操作指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753164(v=ws.10))
- [命令列語法關鍵](command-line-syntax-key.md)
