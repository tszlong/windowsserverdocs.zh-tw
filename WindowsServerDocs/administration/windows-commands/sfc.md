---
title: sfc
description: Sfc 的參考文章，會掃描並確認所有受保護系統檔案的完整性，並以正確的版本取代不正確的版本。
ms.topic: reference
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 18c7457b7f51449796374930d6232045be443c85
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640973"
---
# <a name="sfc"></a>sfc

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

掃描並驗證所有受保護系統檔案的完整性，並以正確的版本取代不正確的版本。


## <a name="syntax"></a>語法
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/scannow|掃描所有受保護系統檔案的完整性，並盡可能修復有問題的檔案。|
|/verifyonly|掃描所有受保護系統檔案的完整性。 未執行任何修復操作。|
|/scanfile|盡可能掃描指定檔案的完整性，並在偵測到問題時修復檔案。|
|\<file>|指定的完整路徑和檔案名|
|/verifyfile|確認指定檔案的完整性。 未執行任何修復操作。|
|/offwindir|指定離線修復的離線 windows 目錄位置。|
|/offbootdir|指定離線開機目錄離線的位置|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您必須以 Administrators 群組成員的身分登入，才能執行 **sfc.exe**。
-   如果 **sfc** 發現已覆寫受保護的檔案，它會從 **systemroot\system32\dllcache** 資料夾中抓取正確的檔案版本，然後取代不正確的檔案。
-   Windows Server 2003、Windows Server 2008 和 Windows Server 2008 R2 上的 **sfc** 有一些功能上的差異：
-   如需有關 Windows Server 2003 上 **sfc** 的詳細資訊，請參閱 Microsoft 知識庫中的 [文章 310747](https://go.microsoft.com/fwlink/?LinkId=227069) 。
-   如需有關 Windows Server 2008 和 Windows Server 2008 R2 上的 **sfc** 的詳細資訊，請參閱 [系統檔案檢查工具](https://go.microsoft.com/fwlink/?LinkId=227071)。

## <a name="examples"></a>範例
若要驗證 **kernel32.dll**檔案，請輸入：
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
若要設定離線修復 **kernel32.dll** 檔，並將離線開機目錄設定為 **d：** 和離線 windows 目錄設為 **d:\windows**，請輸入：
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

