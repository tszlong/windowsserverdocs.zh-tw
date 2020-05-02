---
title: wbadmin delete catalog
description: Wbadmin delete catalog 的參考主題會刪除儲存在本機電腦上的備份類別目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73f6f44fb343d3347d18cf2c86913aea59613e07
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720206"
---
# <a name="wbadmin-delete-catalog"></a>wbadmin delete catalog



刪除儲存在本機電腦上的備份類別目錄。 當備份類別目錄已損毀，而且您無法使用**wbadmin restore catalog**進行還原時，請使用此命令。

若要使用這個子命令刪除備份類別目錄，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）。

## <a name="syntax"></a>語法

```
wbadmin delete catalog
[-quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-quiet|執行子命令，而不提示使用者。|

## <a name="remarks"></a>備註

如果您刪除電腦的備份類別目錄，就無法使用 [Windows Server Backup] 嵌入式管理單元來存取該電腦所建立的備份。 在此情況下，如果您可以存取其他備份位置，請使用**wbadmin restore catalog**從該位置還原備份類別目錄。 刪除備份類別目錄後，您應該建立新的備份。

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [移除-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)