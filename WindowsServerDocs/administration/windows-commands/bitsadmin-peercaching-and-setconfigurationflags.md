---
title: bitsadmin 對等和 setconfigurationflags
description: Bitsadmin 對等互連**和 setconfigurationflags**的 Windows 命令主題-設定判斷電腦是否可以將內容提供給對等，並且可以從對等下載內容的設定旗標。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a65d54bcaa2bce26eb2b7c98250837ab09c7a423
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381110"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin 對等和 setconfigurationflags



設定判斷電腦是否可以將內容提供給對等，並且可以從對等下載內容的設定旗標。

## <a name="syntax"></a>語法

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|值|值為不帶正負號的整數，其在二進位標記法中具有下列對位的解讀：</br>-允許從對等下載作業的資料：設定最不重要的位</br>-允許將作業的資料提供給對等：從右邊設定第二個位。|

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myJob*的作業，指定要從對等下載的作業資料。
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)