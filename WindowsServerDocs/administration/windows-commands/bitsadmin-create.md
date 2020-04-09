---
title: bitsadmin create
description: 適用于**bitsadmin create**的 Windows 命令主題，它會建立具有指定顯示名稱的傳送工作。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a922d9f15aff0a9bd064a7e987920adf3a9107d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850811"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立具有指定之顯示名稱的傳送工作。 下載作業會將資料從伺服器傳送到本機檔案。 上傳作業會將資料從本機檔案傳送到伺服器。 上傳-回復作業會將資料從本機檔案傳送到伺服器，並從伺服器接收回複檔案。

使用[bitsadmin resume](bitsadmin-resume.md)參數來啟動傳送佇列中的作業。

## <a name="syntax"></a>語法

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| 類型 | -  **/Download**會將資料從伺服器傳送到本機檔案。<p>-  **/Upload**會將資料從本機檔案傳送到伺服器。<p>-  **/Upload-Reply**會將資料從本機檔案傳送到伺服器，並從伺服器接收回複檔案。<p>在命令列上未指定時，此參數的預設值為 **/Download** 。 此外， **/Upload** 和 **/Upload-Reply** 類型在 BITS 1.2 和更早版本中無法使用。 |
| displayname | 指派給新建立之作業的顯示名稱。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

建立名為*myDownloadJob*的下載作業。

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
