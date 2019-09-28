---
title: bitsadmin 對等和 getconfigurationflags
description: Bitsadmin 對等互連**和 getconfigurationflags**的 Windows 命令主題-取得設定旗標，以判斷電腦是否將內容提供給對等，並且可以從對等下載內容。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94c7eb1a115fe9152b149b8cf65765b179080cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381099"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin 對等和 getconfigurationflags



取得設定旗標，判斷電腦是否將內容提供給對等，並且可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會取得名為*myJob*之作業的設定旗標。
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)