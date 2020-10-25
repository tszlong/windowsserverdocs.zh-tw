---
title: AllServers
description: AllServers 的參考文章，可取得所有 Windows 部署服務伺服器的相關資訊。
ms.topic: reference
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 450c864bef3b3f17f3912a06aa72aa56ce6e529a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524443"
---
# <a name="get-allservers"></a>AllServers

抓取所有 Windows 部署服務伺服器的相關資訊。

> [!NOTE]
> 如果您的環境中有許多 Windows 部署服務伺服器，或連結伺服器的網路連接速度很慢，此命令可能需要很長的時間才能完成。

## <a name="syntax"></a>語法

```
wdsutil [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

### <a name="parameters"></a>參數

|   參數   |                                                                                                                 說明                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show： {Config |                                                                                                                    影像                                                                                                                    |
|  [/Detailed]  | 搭配使用 **/show： Images** 或 **/show： all**時，會傳回每個影像的所有影像中繼資料。 如果未指定 **/Detailed** 選項，則預設行為是傳回映射名稱、描述和檔案名。 |
| [/Forest： {Yes |                                                                                                                     否}]                                                                                                                     |

## <a name="examples"></a>範例

若要查看所有伺服器的相關資訊，請輸入：
```
wdsutil /Get-AllServers /Show:Config
```
若要查看所有伺服器的詳細資訊，請輸入：
```
wdsutil /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)