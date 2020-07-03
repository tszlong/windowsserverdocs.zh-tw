---
title: bitsadmin peercaching and setconfigurationflags
description: Bitsadmin 對等互連和 setconfigurationflags 命令的參考文章，其會設定決定電腦是否可以將內容提供給對等，以及是否可以從對等下載內容的設定旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 868ef39104f1d16c760d91eee401c0d48b27ea1f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928125"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin peercaching and setconfigurationflags

設定決定電腦是否可以將內容提供給對等，以及是否可以從對等下載內容的設定旗標。

## <a name="syntax"></a>語法

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| value | 不帶正負號的整數，具有二進位標記法中的位的下列解讀：<ul><li>若要允許從對等體下載作業的資料，請設定最不重要的位。</li><li>若要允許將作業的資料提供給對等，請從右邊設定第二個位。</li></ul>|

## <a name="examples"></a>範例

針對名為*myDownloadJob*的作業，指定要從對等電腦下載的作業資料：

```
bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

- [bitsadmin 對等命令](bitsadmin-peercaching.md)
