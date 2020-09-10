---
title: bitsadmin create
description: Bitsadmin create 命令的參考文章，此命令會建立具有指定顯示名稱的傳送工作。
ms.topic: reference
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7d621f3c03f2b002c88792bc2cf2b8f2c70351c2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632360"
---
# <a name="bitsadmin-create"></a>bitsadmin create

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立具有指定顯示名稱的傳送工作。

> [!NOTE]
> **/Upload**   BITS 1.2 及更早版本不支援/Upload 和 **/Upload-Reply**參數類型。

## <a name="syntax"></a>語法

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| type | 有三種類型的作業：<ul><li>**內容.** 將資料從伺服器傳送至本機檔案。</li><li>**上.** 將資料從本機檔案傳送至伺服器。</li><li>**/Upload-Reply.** 將資料從本機檔案傳送到伺服器，並從伺服器接收回複檔。</li></ul>如果未指定此參數，這個參數會預設為 **/Download** 。 |
| displayname | 指派給新建立作業的顯示名稱。 |

## <a name="examples"></a>範例

若要建立名為 *myDownloadJob*的下載作業：

```
bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin resume 命令](bitsadmin-resume.md)

- [bitsadmin 命令](bitsadmin.md)
