---
title: bitsadmin create
description: Bitsadmin create 命令的參考文章，它會使用指定的顯示名稱來建立傳送工作。
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca651705955826ab3b65bd95aa90c98170bdb935
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894591"
---
# <a name="bitsadmin-create"></a>bitsadmin create

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立具有指定之顯示名稱的傳送工作。

> [!NOTE]
> **/Upload**   BITS 1.2 和更早版本不支援/Upload 和 **/Upload-Reply**參數類型。

## <a name="syntax"></a>語法

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| type | 有三種類型的作業：<ul><li>**下載.** 將資料從伺服器傳送到本機檔案。</li><li>**上傳.** 將資料從本機檔案傳送到伺服器。</li><li>**/Upload-Reply.** 將資料從本機檔案傳送到伺服器，並從伺服器接收回複檔案。</li></ul>如果未指定，此參數的預設值為 **/Download** 。 |
| displayname | 指派給新建立之作業的顯示名稱。 |

## <a name="examples"></a>範例

若要建立名為*myDownloadJob*的下載作業：

```
bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin resume 命令](bitsadmin-resume.md)

- [bitsadmin 命令](bitsadmin.md)
