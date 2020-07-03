---
title: bitsadmin reset
description: Bitsadmin reset 命令的參考文章，它會取消目前使用者擁有之傳送佇列中的所有工作。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc8faf10f991f06609d653c8cb7a1dc89de2fa8a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926399"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

取消目前使用者擁有之傳送佇列中的所有工作。 您無法重設本機系統所建立的工作。 相反地，您必須是系統管理員，並使用工作排程器，使用本機系統認證將此命令排定為工作。

> [!NOTE]
> 如果您在 BITSAdmin 1.5 和更早版本中具有系統管理員許可權，/reset 參數將會取消佇列中的所有作業。 此外，不支援/allusers 選項。

## <a name="syntax"></a>語法

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| /allusers | 選擇性。 取消目前使用者擁有之佇列中的所有工作。 您必須具有系統管理員許可權，才能使用此參數。 |

## <a name="examples"></a>範例

取消目前使用者的傳輸佇列中的所有工作。

```
bitsadmin /reset
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
