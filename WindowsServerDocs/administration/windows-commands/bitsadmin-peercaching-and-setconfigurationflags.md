---
title: bitsadmin 對等快取和 setconfigurationflags
description: 適用於 Windows 命令主題**bitsadmin 對等快取和 setconfigurationflags** -設定判斷電腦是否可以提供內容給對等，以及可從對等下載內容的組態旗標。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22408d4aab7f5ea374511bc16751d911a84644f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813329"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin 對等快取和 setconfigurationflags



設定判斷電腦是否可以提供內容給對等，以及可從對等下載內容的組態旗標。

## <a name="syntax"></a>語法

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|值|其值為不帶正負號的整數，下列中的二進位表示法的位元的解譯：</br>-允許從對等電腦下載作業的資料：設定最小顯著性的位元</br>-允許作業的資料提供給對等：設定從右邊的第 2 位元。|

## <a name="BKMK_examples"></a>範例

下列範例會指定作業的資料，從名為作業的對等下載*myJob*。
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)