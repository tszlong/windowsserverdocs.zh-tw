---
title: 重設
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0bd9b6735697cbcefdcf68dc3d4a53a6870a7a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850959"
---
# <a name="reset"></a>重設



DiskShadow.exe 重設為預設狀態。 **重設**特別有用，例如來分隔複合 DiskShadow operations**建立**，**匯入**，**備份**，或**還原**.

## <a name="syntax"></a>語法

```
reset
```

## <a name="remarks"></a>備註

-   當您使用**重設**命令時，您會遺失狀態命令這類**新增**，**設定**，**載入**，或**寫入器**. **重設**也釋出 IVssBackupComponent 介面，而且會失去非持續性的陰影複製。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)