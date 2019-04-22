---
title: bitsadmin complete
description: 適用於 Windows 命令主題**bitsadmin 完整**-完成作業。 直到您使用這個參數，不供您下載的檔案。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 561585da370f7e69aa3b83b3ddd7579bfc658a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817319"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

完成的工作。 直到您使用這個參數，不供您下載的檔案。 作業會移至傳輸的狀態之後，請使用此參數。 否則，只有在已成功傳送的檔案使用。

## <a name="syntax"></a>語法

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

當傳送作業的狀態時，位元已成功轉移作業中的所有檔案。 不過，不會提供檔案直到您使用 **/ 完成**切換。 如果有多個作業使用*myDownloadJob*做為其名稱中，您必須取代*myDownloadJob*來唯一識別作業的作業的 guid。
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)