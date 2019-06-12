---
title: 登出
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88b3cbbbd52a965fd0a0de3c164e8dbc1f90d053
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437521"
---
# <a name="logoff"></a>登出

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將使用者登出遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上的工作階段記錄，並從伺服器刪除的工作階段。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>參數

|      參數       |                                                                             描述                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  指定工作階段的名稱。                                                                  |
|     <SessionID>      |                                                 指定用來識別伺服器工作階段的數值識別碼。                                                 |
| /server:<ServerName> | 指定包含您想要登出的使用者工作階段的 rd 工作階段主機伺服器。 如果未指定，則會使用您擔任目前作用中的伺服器。 |
|          /v          |                                                       顯示已執行之動作的相關資訊。                                                        |
|          /?          |                                                                 在命令提示字元顯示說明。                                                                 |

## <a name="remarks"></a>備註
- 您可以一律從登出工作階段，您目前登入。 您，不過，必須從其他工作階段的使用者登出的完整控制權權限。
- 將使用者登出工作階段，而不發出警告記錄可能會導致資料遺失在使用者的工作階段。 您應該向使用者傳送一則訊息，使用**msg**警告使用者採取此動作之前的命令。
- 如果 <*SessionID*> 或 <*SessionName*> 未指定，則**登出**目前工作階段使用者登出。 如果您指定 <*SessionName*>，它必須是作用中的一個。
- 當您登出使用者時，所有處理序結束和工作階段從伺服器中刪除。
- 您無法登出使用者，以從主控台工作階段。
  ## <a name="BKMK_examples"></a>範例
- 若要登出目前工作階段的使用者，請輸入：
  ```
  logoff
  ```
- 若要使用工作階段的識別碼，例如工作階段 12 登出使用者登出工作階段，請輸入：
  ```
  logoff 12
  ```
- 若要使用的名稱，在工作階段的伺服器，例如工作階段 TERM04 在 Server1 上登出使用者登出工作階段，請輸入：
  ```
  logoff TERM04 /server:Server1
  ```

#### <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
-   [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
