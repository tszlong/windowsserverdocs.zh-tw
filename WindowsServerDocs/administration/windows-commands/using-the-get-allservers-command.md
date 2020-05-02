---
title: AllServers
description: AllServers 的參考主題，它會抓取所有 Windows 部署服務伺服器的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b623b5e95e2a57147b7d9d191d42556191dd8e4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719976"
---
# <a name="get-allservers"></a>AllServers

抓取所有 Windows 部署服務伺服器的相關資訊。

> [!NOTE]
> 如果您的環境中有許多 Windows 部署服務伺服器，或連結伺服器的網路連線速度緩慢，此命令可能需要相當長的時間才能完成。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

### <a name="parameters"></a>參數

|   參數   |                                                                                                                 描述                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show： {Config |                                                                                                                    映像                                                                                                                    |
|  [/Detailed]  | 與 **/show： Images**或 **/show： All**一起使用時，會傳回每個影像中的所有影像中繼資料。 如果未指定 **/Detailed**選項，則預設行為是傳回映射名稱、描述和檔案名。 |
| [/Forest： {Yes |                                                                                                                     否}]                                                                                                                     |

## <a name="examples"></a>範例

若要查看所有伺服器的相關資訊，請輸入：
```
WDSUTIL /Get-AllServers /Show:Config
```
若要查看所有伺服器的詳細資訊，請輸入：
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)