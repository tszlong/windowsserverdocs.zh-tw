---
title: bitsadmin peercaching and setconfigurationflags
description: Bitsadmin 對等互連和 setconfigurationflags 命令的參考文章，此命令會設定設定旗標，以判斷電腦是否可以將內容提供給對等，以及是否可以從對等下載內容。
ms.topic: reference
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73daad6a915ee39f166d54efd79290ce92df60db
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631339"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin peercaching and setconfigurationflags

設定旗標，以判斷電腦是否可以將內容提供給對等，以及是否可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| value | 針對二進位標記法中的位，具有下列解釋的不帶正負號的整數：<ul><li>若要允許從對等下載作業的資料，請設定最不重要的位。</li><li>若要允許將作業的資料提供給對等，請設定右邊的第二位。</li></ul>|

## <a name="examples"></a>範例

若要針對名為 *myDownloadJob*的作業指定要從對等下載的作業資料：

```
bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

- [bitsadmin 對等的命令](bitsadmin-peercaching.md)
