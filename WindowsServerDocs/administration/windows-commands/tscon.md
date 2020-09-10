---
title: tscon
description: Tscon 的參考文章，可連接到遠端桌面工作階段主機 (rd 工作階段主機) server 上的另一個會話。
ms.topic: reference
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2ba6df3a8878c42ac8ce8ac88671bb645bfedf86
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624604"
---
# <a name="tscon"></a>tscon

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到遠端桌面工作階段主機伺服器上的另一個會話。



> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 Windows server TechNet Library 中的 [Windows server 2012 遠端桌面服務的新功能](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) 。

## <a name="syntax"></a>語法
```
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]
```
### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|\<SessionID>|指定您要連接之會話的識別碼。 如果您使用選擇性的 **/dest：** < *SessionName*> 參數，這就是您要連接的會話識別碼。|
|\<SessionName>|指定您要連接之會話的名稱。|
|/dest\<SessionName>|指定目前會話的名稱。 當您連接到新的會話時，此會話將會中斷連線。|
|/password\<pw>|指定擁有您要連接之會話的使用者密碼。 當連接的使用者未擁有會話時，需要此密碼。|
|/password： *|提示您輸入擁有您要連接之會話的使用者密碼。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您必須擁有 [完全控制] 存取權限或 [連接特殊存取權限]，才能連接到另一個會話。
-   **/Dest：** < *SessionName*> 參數可讓您將另一位使用者的會話連線到不同的會話。
-   如果您未在 <*密碼*> 參數中指定密碼，且目標會話屬於目前的使用者，則 **tscon** 會失敗。
-   您無法連接到主控台會話。

## <a name="examples"></a>範例
- 若要在目前的 rd 工作階段主機伺服器上連接到會話12，並中斷目前會話的連線，請輸入：
  ```
  tscon 12
  ```
- 若要在目前的 rd 工作階段主機伺服器上連接到會話23，請使用密碼 mypass 並中斷目前會話的連線，然後輸入：
  ```
  tscon 23 /password:mypass
  ```
- 若要將名為 TERM03 的會話連線到名為 TERM05 的會話，然後中斷連接會話 TERM05 （如果已連線），請輸入：
  ```
  tscon TERM03 /v /dest:TERM05
  ```
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md) 
  [遠端桌面服務 (終端機服務) 命令參考](remote-desktop-services-terminal-services-command-reference.md)
