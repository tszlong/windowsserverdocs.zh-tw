---
title: 使用 get AllServers 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbccb834f9058f2c3cca097cdf998455f2a6892e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440491"
---
# <a name="using-the-get-allservers-command"></a>使用 get AllServers 命令



擷取所有的 Windows 部署服務伺服器的相關資訊。

> [!NOTE]
> 此命令可能需要長時間才能完成，如果您的環境中有多個 Windows 部署服務伺服器，或連結伺服器的網路連線速度很慢。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>參數

|   參數   |                                                                                                                 描述                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / 顯示: {組態 |                                                                                                                    映像                                                                                                                    |
|  [/ 詳細]  | 當搭配 **/Show:Images**或是 **/Show:All**，傳回所有影像中每個映像的中繼資料。 如果 **/詳細**選項未指定，預設行為是傳回映像名稱、 描述和檔案名稱。 |
| [/ 樹系: {是 |                                                                                                                     No}]                                                                                                                     |

## <a name="BKMK_examples"></a>範例

若要檢視所有伺服器的相關資訊，請輸入：
```
WDSUTIL /Get-AllServers /Show:Config
```
若要檢視所有伺服器的詳細的資訊，請輸入：
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)