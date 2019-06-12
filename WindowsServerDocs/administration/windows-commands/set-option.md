---
title: Set 選項
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4756627d19d296d02fa11ac67ef80080ddf318
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441362"
---
# <a name="set-option"></a>Set 選項



設定好建立陰影複製的選項。 如果未指定參數，使用**設定選項**在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>參數

|     參數     |                                                                                                  描述                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [差異   |                                                                                                     plex]                                                                                                     |
|  [transportable]  |                       指定陰影複製是不尚未匯入。 中繼資料.cab 檔案稍後可用來匯入陰影複製到相同或不同的電腦。                       |
| [rollbackrecover] |                     表示要使用的寫入器*n>* 期間**PostSnapshot**事件。 這非常有用，如果陰影複製將用於復原 （例如，使用資料採礦）。                      |
|   [txfrecover]    |                                                               請在建立期間的交易上一致的陰影複製要求 VSS。                                                                |
|  [noautorecover]  | 停駐點寫入器和檔案系統執行陰影複製到交易一致狀態的任何復原變更。 **Noautorecover**不能搭配**txfrecover**或是**rollbackrecover**。 |

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)