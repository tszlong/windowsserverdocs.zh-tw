---
title: bitsadmin getsecurityflags
description: 適用于**bitsadmin getsecurityflags**的 Windows 命令主題，它會報告 URL 重新導向的 HTTP 安全性旗標，以及在傳送期間在伺服器憑證上執行的檢查。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 360f8d5514e5251dd9e4a6a6b60335dc34fe4415
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850471"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

報告用於 URL 重新導向的 HTTP 安全性旗標，以及在傳送期間在伺服器憑證上執行的檢查。

## <a name="syntax"></a>語法

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會從名為*myDownloadJob*的作業中，捕獲安全性旗標。

```
C:\>bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)