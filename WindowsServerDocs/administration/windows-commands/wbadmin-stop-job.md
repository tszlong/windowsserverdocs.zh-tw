---
title: wbadmin stop job
description: Wbadmin stop job 的參考文章，可取消目前正在執行的備份或復原作業。 無法重新開機已取消的作業，您必須從一開始重新執行已取消的備份或復原作業。
ms.topic: reference
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0ca6fca8dda7c741a9ad9c52a7b6ec0f5e75dea
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031826"
---
# <a name="wbadmin-stop-job"></a>wbadmin stop job



取消目前正在執行的備份或復原操作。 無法重新開機已取消的作業，您必須從一開始重新執行已取消的備份或復原作業。

若要使用此子命令停止備份或復原操作，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中執行 **wbadmin** 。  (開啟提升許可權的命令提示字元，以滑鼠右鍵按一下 **命令提示** 字元，然後按一下 [以 **系統管理員身分執行**]。 ) 

## <a name="syntax"></a>語法

```
wbadmin stop job
[-quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-quiet|執行子命令，而不提示使用者。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Backup](wbadmin.md)