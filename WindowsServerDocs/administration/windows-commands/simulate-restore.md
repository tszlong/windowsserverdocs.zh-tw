---
title: 模擬還原
description: 適用于模擬還原的 Windows 命令主題，其會測試作者參與電腦上還原會話的工作，而不會對寫入器發出 PreRestore 或 PostRestore 事件。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 024d654864c000e44bccb9ddb167c6147444cc00
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834101"
---
# <a name="simulate-restore"></a>模擬還原

測試 writer 會參與電腦上的還原會話，而不會對寫入器發出**PreRestore**或**PostRestore**事件。

## <a name="syntax"></a>語法

```
simulate restore
```

## <a name="remarks"></a>備註

-   [**模擬還原**] 用來測試寫入器是否可以成功進行還原。
-   在您可以使用 [**模擬還原**] 之前，您必須使用 [**載入中繼資料**] 命令來載入 DiskShadow 中繼資料檔案。 這會載入選取的寫入器和還原的元件。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)