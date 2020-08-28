---
title: wbadmin disable backup
description: Wbadmin disable backup 的參考文章，會停止執行現有的已排程每日備份。
ms.topic: reference
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 685e74d0c5e6b18e0929aa97361eb5ff0bea5b94
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032103"
---
# <a name="wbadmin-disable-backup"></a>wbadmin disable backup



停止執行現有的已排程每日備份。

若要停用排程的每日備份，您必須是 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中執行 **wbadmin** 。  (開啟提升許可權的命令提示字元，以滑鼠右鍵按一下 **命令提示** 字元，然後按一下 [以 **系統管理員身分執行**]。 ) 

## <a name="syntax"></a>語法

```
wbadmin disable backup
[-quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-quiet|執行子命令，而不提示使用者。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Backup](wbadmin.md)