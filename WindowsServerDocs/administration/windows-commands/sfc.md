---
title: sfc
description: Sfc 命令的參考文章，它會掃描並確認所有受保護系統檔案的完整性，並以正確的版本取代不正確的版本。
ms.topic: reference
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 829c6e328ad0ea993e11cb5eb5d96d99f0d52476
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388938"
---
# <a name="sfc"></a>sfc

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

掃描並驗證所有受保護系統檔案的完整性，並以正確的版本取代不正確的版本。 如果這個命令發現已覆寫受保護的檔案，它會從 **systemroot\system32\dllcache** 資料夾中抓取正確的檔案版本，然後取代不正確的檔案。

> [!IMPORTANT]
> 您必須以 Administrators 群組成員的身分登入，才能執行此命令。

## <a name="syntax"></a>語法

```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /scannow | 掃描所有受保護系統檔案的完整性，並盡可能修復有問題的檔案。 |
| /verifyonly | 掃描所有受保護系統檔案的完整性，而不執行修復。 |
| /scanfile `<file>` | 掃描指定檔案的完整性 (完整路徑和檔案名) 並在偵測到任何問題時嘗試修復。 |
| /verifyfile `<file>` | 確認指定檔案的完整性 (完整路徑和檔案名) ，而不執行修復。 |
| /offwindir `<offline windows directory>` | 指定離線修復的離線 windows 目錄位置。 |
| /offbootdir `<offline boot directory>` | 指定離線修復的離線開機目錄位置。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要驗證 *kernel32.dll*檔案，請輸入：

```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```

若要設定離線修復 *kernel32.dll* 檔，並將離線開機目錄設定為 * D：並將 \* 離線 windows 目錄設定為 *D:\windows*，請輸入：

```
sfc /scanfile=D:\windows\system32\kernel32.dll /offbootdir=D:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
