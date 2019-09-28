---
title: wbadmin delete catalog
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 58b8bc6043437755675af28c084257ba0d8b176d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362535"
---
# <a name="wbadmin-delete-catalog"></a>wbadmin delete catalog



刪除儲存在本機電腦上的備份類別目錄。 當備份類別目錄已損毀，而且您無法使用**wbadmin restore catalog**進行還原時，請使用此命令。

若要使用這個子命令刪除備份類別目錄，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）。

## <a name="syntax"></a>語法

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-quiet|執行子命令，而不提示使用者。|

## <a name="remarks"></a>備註

如果您刪除電腦的備份類別目錄，就無法使用 [Windows Server Backup] 嵌入式管理單元來存取該電腦所建立的備份。 在此情況下，如果您可以存取其他備份位置，請使用**wbadmin restore catalog**從該位置還原備份類別目錄。 刪除備份類別目錄後，您應該建立新的備份。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [移除-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)