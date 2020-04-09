---
title: bitsadmin 對等和 getconfigurationflags
description: 適用于**bitsadmin**對等互連和**Getconfigurationflags**的 Windows 命令主題，可取得設定旗標來判斷電腦是否將內容提供給對等，以及是否可以從對等下載內容。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be8d6a719d63c8e9c6250320560b6ce21274c680
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850171"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin 對等和 getconfigurationflags

取得設定旗標，以判斷電腦是否將內容提供給對等，以及是否可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會取得名為*myDownloadJob*之作業的設定旗標。

```
C:\> bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)