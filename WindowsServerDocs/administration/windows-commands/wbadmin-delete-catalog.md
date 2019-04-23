---
title: Wbadmin delete 類別目錄
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9f60cf79fcd856fa972d8f43f195c5823e5d81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841409"
---
# <a name="wbadmin-delete-catalog"></a>Wbadmin delete 類別目錄



刪除備份資料目錄儲存在本機電腦上。 使用此命令，當備份類別目錄已損毀，且您無法還原使用**wbadmin 還原目錄**。

若要刪除此子命令與備份類別目錄，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

## <a name="syntax"></a>語法

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-quiet|子命令會以不執行任何提示給使用者。|

## <a name="remarks"></a>備註

如果您刪除備份資料目錄的電腦，您將無法存取的 Windows Server Backup 嵌入式管理單元中使用該電腦建立的備份。 在此案例中，如果您可以存取另一個備份的位置，使用**wbadmin 還原目錄**從該位置中還原備份類別目錄。 刪除備份類別目錄之後，您應該建立新的備份。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)