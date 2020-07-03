---
title: reset
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a412eff7bdf432608a999edb4531074ed5e8f26
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933093"
---
# <a name="reset"></a>reset



將 DiskShadow.exe 重設為預設狀態。 [**重設**] 特別適合用來分隔複合 DiskShadow 作業，例如**建立**、匯**入**、**備份**或**還原**。

## <a name="syntax"></a>語法

```
reset
```

## <a name="remarks"></a>備註

-   當您使用 [**重設**] 命令時，您會從 [**新增**]、[**設定**]、[**載入**] 或 [**寫入器**] 等命令遺失狀態 **Reset**也會釋放 IVssBackupComponent 介面，並失去非持續性陰影複製。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)