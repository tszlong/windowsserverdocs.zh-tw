---
title: getmac
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c770f5da5159e0037af479f90fadb4cd83464c77
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375815"
---
# <a name="getmac"></a>getmac

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

針對每部電腦中的所有網路卡，在本機或網路上，傳回媒體存取控制（MAC）位址和與每個位址相關聯的網路通訊協定清單。 
## <a name="syntax"></a>語法
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>參數

|             參數              |                                                                                          描述                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s <computer>            |                                      指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                       |
|        /u <Domain> @ no__t-1 @ no__t-2         | 以 User 或 Domain\user 所指定使用者的帳戶許可權來執行命令 預設為發出命令之電腦上目前登入使用者的許可權。 |
|           /p <Password>            |                                                     指定 **/u**參數中指定之使用者帳戶的密碼。                                                     |
| /fo {資料表&#124;清單&#124; CSV} |                       指定要用於查詢輸出的格式。 有效值為**TABLE**、 **list**和**CSV**。 輸出的預設格式為**TABLE**。                        |
|                /nh                 |                                             隱藏輸出中的資料行標頭。 當 **/fo**參數設定為**TABLE**或**CSV**時有效。                                              |
|                 /v                 |                                                                    指定輸出顯示詳細資訊。                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>備註
當您想要在網路分析器中輸入 MAC 位址，或當您需要知道電腦中的每個網路介面卡目前正在使用哪些通訊協定時， **getmac**可能會很有用。
## <a name="BKMK_Examples"></a>典型
下列範例會示範如何使用**getmac**命令：
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>其他參考
-   [命令列語法關鍵](command-line-syntax-key.md)
