---
title: wbadmin disable backup
description: Wbadmin disable backup 的參考文章，它會停止執行現有排程的每日備份。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88383975aa3ae8d6821698159e6ee445198301c5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933672"
---
# <a name="wbadmin-disable-backup"></a>wbadmin disable backup



停止執行現有排程的每日備份。

若要停用排程的每日備份，您必須是**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）

## <a name="syntax"></a>語法

```
wbadmin disable backup
[-quiet]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|-quiet|執行子命令，而不提示使用者。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)