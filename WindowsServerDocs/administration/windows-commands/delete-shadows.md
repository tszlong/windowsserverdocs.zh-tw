---
title: 刪除陰影
description: 刪除陰影的 Windows 命令主題，會刪除陰影複製。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd109f7ddc0365d03737eddba31a1a4b7f34915b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846561"
---
# <a name="delete-shadows"></a>刪除陰影

刪除陰影複製。

## <a name="syntax"></a>語法

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---- | ---- |
| 全部 | 刪除所有陰影複製。 |
| 磁片區 \<磁片區 > | 刪除指定磁片區的所有陰影複製。 |
| 最舊的 \<磁片區 > | 刪除指定磁片區最舊的陰影複製。 |
| 設定 \<SetID > | 刪除指定識別碼之陰影複製組中的陰影複製。 如果別名存在於目前的環境中，您可以使用 **%** 符號來指定別名。 |
| 識別碼 \<ShadowID > | 刪除指定識別碼的陰影複本。 如果別名存在於目前的環境中，您可以使用 **%** 符號來指定別名。 |
| 公開 {\<磁片磁碟機 > | <MountPoint>} |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)