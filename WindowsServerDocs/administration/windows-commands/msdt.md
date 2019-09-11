---
title: msdt
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7bec16ab3f716148bb009dd56be475fcd058a897
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868904"
---
# <a name="msdt"></a>msdt



在命令列或自動腳本的過程中叫用疑難排解套件，並啟用其他選項而不需要使用者輸入。

## <a name="syntax"></a>語法

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>參數

下表包含 msdt 支援的參數和選項。


|      參數      |                                                                                            描述                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /id \<封裝名稱 > |        指定要執行的診斷封裝。 如需可用套件的清單，請參閱本主題稍後的「可用的疑難排解套件」一節中的疑難排解套件識別碼。         |
|  /path \<目錄  |                                                                                           diagpkg 檔案                                                                                            |
|   /dci \<密碼 >   |                                        會預先填入在 msdt 中的密碼欄位。 只有在支援提供者提供通行金鑰時，才會使用這個參數。                                         |
|  /dt \<目錄 >   | 在指定的目錄中顯示疑難排解記錄。 診斷結果會儲存在使用者的 **%LOCALAPPDATA%\Diagnostics**或 **%LOCALAPPDATA%\ElevatedDiagnostics**目錄中。 |
| /af \<回應檔案 >  |                                               指定 XML 格式的回應檔案，其中包含一個或多個診斷互動的回應。                                               |

