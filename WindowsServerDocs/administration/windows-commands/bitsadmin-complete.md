---
title: bitsadmin complete
description: Bitsadmin 的 Windows 命令主題**完成**-完成作業。 您必須先使用此交換器，才可取得下載的檔案。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d5a1dc5dbbf2d5b3207b5423f338e0caf4412599
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381823"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

完成作業。 您必須先使用此交換器，才可取得下載的檔案。 在作業移至已轉移狀態之後，使用此參數。 否則，只有已成功傳輸的檔案才可使用。

## <a name="syntax"></a>語法

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

當作業的狀態為「已傳輸」時，BITS 已成功傳送作業中的所有檔案。 不過，除非您使用 **/complete**參數，否則無法使用這些檔案。 如果多個作業使用*myDownloadJob*作為其名稱，您必須將*myDownloadJob*取代為作業的 GUID，以唯一識別作業。
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)