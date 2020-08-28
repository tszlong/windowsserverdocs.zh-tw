---
title: bitsadmin reset
description: Bitsadmin reset 命令的參考文章，此命令會取消目前使用者所擁有之傳送佇列中的所有工作。
ms.topic: reference
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ab8c1e81fa2ede43897e05778b974b0ff094311
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026316"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

取消目前使用者所擁有之傳送佇列中的所有工作。 您無法重設本機系統建立的工作。 相反地，您必須是系統管理員，並使用 [工作排程器]，將此命令排程為使用本機系統認證的工作。

> [!NOTE]
> 如果您在 BITSAdmin 1.5 和更早版本中有系統管理員許可權，/reset 參數將會取消佇列中的所有作業。 此外，不支援/allusers 選項。

## <a name="syntax"></a>語法

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| /allusers | 選擇性。 取消目前使用者所擁有之佇列中的所有作業。 您必須具有系統管理員許可權，才能使用此參數。 |

## <a name="examples"></a>範例

取消目前使用者的傳送佇列中的所有作業。

```
bitsadmin /reset
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
