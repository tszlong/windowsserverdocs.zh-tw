---
title: tlntadmn
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b4423e35d0c26819188001dea8d3d8497add7f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440951"
---
# <a name="tlntadmn"></a>tlntadmn

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

管理本機或遠端電腦執行 telnet 伺服器服務。   
## <a name="syntax"></a>語法  
```  
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]  
```  
### <a name="parameters"></a>參數  

|                   參數                    |                                                                                                                                                       描述                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                \<computerName>                 |                                                                                                                    指定要連接到伺服器的名稱。 預設是本機電腦。                                                                                                                    |
|         -u \<UserName> -p \<Password>          |                                                指定您想要管理遠端伺服器的系統管理認證。 如果您想要管理遠端伺服器，您不登入系統管理認證，則需要此參數。                                                |
|                     start                      |                                                                                                                                            啟動 telnet 伺服器服務。                                                                                                                                             |
|                      stop                      |                                                                                                                                             停止 telnet 伺服器服務                                                                                                                                              |
|                     pause                      |                                                                                                                          telnet 伺服器服務就會暫停。 將會不接受任何新的連線。                                                                                                                          |
|                    繼續                    |                                                                                                                                            Telnet 伺服器服務就會繼續。                                                                                                                                            |
|          -s {\<SessionID >&#124;所有}          |                                                                                                                                             顯示作用中 telnet 工作階段。                                                                                                                                             |
|          -k {\<SessionID> &#124; all}          |                                                                                                        結束 telnet 工作階段。 輸入工作階段識別碼，來結束特定的工作階段中，或輸入所有結束所有工作階段。                                                                                                         |
|    -m {\<SessionID> &#124; all}  <Message>     |                                                   將訊息傳送至一或多個工作階段。 輸入工作階段識別碼，將訊息傳送到特定的工作階段中，或輸入所有訊息傳送至所有工作階段。 輸入您想要將引號之間傳送的訊息。                                                   |
|             設定 dom =\<網域 >             |                                                                                                                                      設定伺服器的預設網域。                                                                                                                                       |
|      config ctrlakeymap = {yes &#124; no}      |                                                                                     指定是否您想要將 CTRL + A 以 ALT telnet 伺服器。 型別 **[是]** 對應的快速鍵，或型別**沒有**以防止對應。                                                                                     |
|       設定逾時 = \<hh >:\<公釐 >:\<ss >       |                                                                                                                                 您可以設定逾時期限中小時、 分鐘和秒。                                                                                                                                 |
|     config timeoutactive = {yes &#124; no      |                                                                                                                                            可讓閒置的工作階段逾時。                                                                                                                                             |
|          config maxfail = \<attempts>          |                                                                                                                          設定中斷連線前的嘗試登入失敗的數目上限。                                                                                                                          |
|        設定 maxconn =\<連接 >         |                                                                                                                                         設定連線的數目上限。                                                                                                                                          |
|            config port = <\Number>             |                                                                                                                    設定 telnet 連接埠。 您必須使用小於 1024年的整數來指定連接埠。                                                                                                                    |
| config sec {+ &#124; -}NTLM {+ &#124; -}passwd | 指定是否要使用 NTLM、 密碼，或兩者來驗證登入嘗試。 若要使用特定類型的驗證，請輸入一個加號 ( **+** ) 驗證該型別前面。 若要避免使用特定類型的驗證，輸入減號 ( **-** ) 驗證該型別前面。 |
|     config mode = {console &#124; stream}      |                                                                                                                                             指定作業的模式。                                                                                                                                             |
|                       -?                       |                                                                                                                                           在命令提示字元顯示說明。                                                                                                                                           |

## <a name="remarks"></a>備註  
-   若要顯示的伺服器設定，請輸入**tlntadmn**不含任何參數。  
-   若要使用**tlntadmn**命令時，您必須登入本機電腦系統管理認證。 若要管理遠端電腦，您也必須提供系統管理認證的遠端電腦。 您可以藉由登入本機電腦帳戶具有本機電腦和遠端電腦的系統管理認證來這麼做。 如果您無法使用這個方法，您可以使用 **-u**並 **-p**參數提供的遠端電腦的系統管理認證。  

## <a name="BKMK_Examples"></a>範例  
設定為 30 分鐘的閒置工作階段逾時。  
```  
tlntadmn config timeout=0:30:0  
```  
顯示作用中 telnet 工作階段。  
```  
tlntadmn -s  
```  

## <a name="additional-references"></a>其他參考資料  
-   [telnet 操作指南](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
