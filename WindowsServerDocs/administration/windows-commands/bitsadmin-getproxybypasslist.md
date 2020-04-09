---
title: bitsadmin getproxybypasslist
description: 適用于**bitsadmin getproxybypasslist**的 Windows 命令主題，它會抓取指定工作的 proxy 略過清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cd81aaef22c4173f198b765246b78b3d3bae136
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850531"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

抓取指定之作業的 proxy 略過清單。

## <a name="syntax"></a>語法

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="remarks"></a>備註

「略過清單」包含不會透過 proxy 路由傳送的主機名稱或 IP 位址（或兩者）。 清單可以包含 `<local>`，以參照相同 LAN 上的所有伺服器。 清單可以是分號（;)或以空格分隔。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取 proxy 略過清單。

```
C:\>bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)