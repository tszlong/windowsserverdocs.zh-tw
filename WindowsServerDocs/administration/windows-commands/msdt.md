---
title: msdt
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c1e4e8cd6d9de036b47de590867a6531d0335a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839251"
---
# <a name="msdt"></a>msdt



在命令列或自動腳本的過程中叫用疑難排解套件，並啟用其他選項而不需要使用者輸入。

## <a name="syntax"></a>語法

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>參數

下表包含 msdt 支援的參數和選項。


|      參數      |                                                                                            描述                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /id \<封裝名稱 > |        指定要執行的診斷封裝。 如需可用套件的清單，請參閱本主題稍後的可用疑難排解套件一節中的疑難排解套件識別碼。         |
|  /path \<目錄  |                                                                                           diagpkg 檔案                                                                                            |
|   /dci \<密碼金鑰 >   |                                        會預先填入在 msdt 中的密碼欄位。 只有在支援提供者提供通行金鑰時，才會使用這個參數。                                         |
|  /dt \<directory >   | 在指定的目錄中顯示疑難排解記錄。 診斷結果會儲存在使用者的 **%LOCALAPPDATA%\Diagnostics**或 **%LOCALAPPDATA%\ElevatedDiagnostics**目錄中。 |
| /af \<回應檔案 >  |                                               指定 XML 格式的回應檔案，其中包含一個或多個診斷互動的回應。                                               |

