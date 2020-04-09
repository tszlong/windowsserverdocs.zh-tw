---
title: bitsadmin 對等和 setconfigurationflags
description: 適用于**bitsadmin**對等互連和**Setconfigurationflags**的 Windows 命令主題，其會設定決定電腦是否可以將內容提供給對等，以及是否可以從對等下載內容的設定旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebaa09da2d4594d2762e67dc5884dd15cf4d1da8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850131"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin 對等和 setconfigurationflags

設定決定電腦是否可以將內容提供給對等，以及是否可以從對等下載內容的設定旗標。

## <a name="syntax"></a>語法

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| 值 | 不帶正負號的整數，具有二進位標記法中的位的下列解讀：<ul><li> 若要允許從對等體下載作業的資料，請設定最不重要的位。</li><li>若要允許將作業的資料提供給對等，請從右邊設定第二個位。</li></ul>|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業，指定要從對等下載的作業資料。

```
C:\> bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)