---
title: getmac
description: Getmac 命令的參考文章，此命令會傳回媒體存取控制 (MAC) 位址，以及與每個在本機或網路上相關聯的網路通訊協定清單。
ms.topic: reference
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 901777744b98095e4e19ff39d9965d144ee1f1c8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034076"
---
# <a name="getmac"></a>getmac

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

傳回媒體存取控制 (MAC) 位址，以及與每一部電腦上所有網路卡相關聯的網路通訊協定清單（不論是本機或網路）。 當您想要將 MAC 位址輸入網路分析器，或是當您需要知道電腦上的每個網路介面卡目前正在使用哪些通訊協定時，此命令特別有用。

## <a name="syntax"></a>語法

```
getmac[.exe][/s <computer> [/u <domain\<user> [/p <password>]]][/fo {table | list | csv}][/nh][/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 |
| u `<domain>\<user>` | 使用 *user* 或 *domain\user*所指定使用者的帳戶許可權來執行此命令。 預設值是發出命令的電腦上目前登入之使用者的許可權。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| /fo {table | list | csv | 指定用於查詢輸出的格式。 有效的值為 **table**、 **list**和 **csv**。 輸出的預設格式為 **table**。 |
| /nh | 隱藏輸出中的資料行標頭。 當 **/fo** 參數設定為 **資料表** 或 **csv**時有效。 |
| /v | 指定輸出顯示詳細資訊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

下列範例會示範如何使用 **getmac** 命令：

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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
