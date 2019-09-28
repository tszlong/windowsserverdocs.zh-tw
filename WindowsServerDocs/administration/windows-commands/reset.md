---
title: 啟動
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8903c300d12a019f8fb4aef6d367131a195d034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371456"
---
# <a name="reset"></a>啟動



將 DiskShadow 重設為預設狀態。 [**重設**] 特別適合用來分隔複合 DiskShadow 作業，例如**建立**、匯**入**、**備份**或**還原**。

## <a name="syntax"></a>語法

```
reset
```

## <a name="remarks"></a>備註

-   當您使用 [**重設**] 命令時，您會從 [**新增**]、[**設定**]、[**載入**] 或 [**寫入器**] 等命令遺失狀態 **Reset**也會釋放 IVssBackupComponent 介面，並失去非持續性陰影複製。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)