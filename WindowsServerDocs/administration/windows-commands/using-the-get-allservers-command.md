---
title: AllServers
description: AllServers 的 Windows 命令主題，它會抓取所有 Windows 部署服務伺服器的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b400d5a2be69e8e89a05b233cc2e8f29bec848f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831211"
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

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要查看所有伺服器的相關資訊，請輸入：
```
WDSUTIL /Get-AllServers /Show:Config
```
若要查看所有伺服器的詳細資訊，請輸入：
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)