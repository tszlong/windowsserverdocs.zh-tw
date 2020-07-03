---
title: sfc
description: Sfc 的參考文章，它會掃描並確認所有受保護系統檔案的完整性，並以正確的版本取代不正確的版本。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f4b0798f9c0e3e1c70ca701de1ea2246bddf7b9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931614"
---
# <a name="sfc"></a>sfc

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

會掃描並確認所有受保護系統檔案的完整性，並以正確的版本取代不正確的版本。


## <a name="syntax"></a>語法
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

#### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|/scannow|會掃描所有受保護系統檔案的完整性，並在可能時修復有問題的檔案。|
|/verifyonly|掃描所有受保護系統檔案的完整性。 不會執行任何修復操作。|
|/scanfile|如果偵測到問題，請掃描指定檔案的完整性，並修復檔案（如果可能的話）。|
|\<file>|指定的完整路徑和檔案名|
|/verifyfile|驗證指定檔案的完整性。 不會執行任何修復操作。|
|/offwindir|針對離線修復指定離線 windows 目錄的位置。|
|/offbootdir|指定離線開機目錄的位置以供離線|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您必須以 Administrators 群組成員的身分登入，才能執行**sfc.exe**。
-   如果**sfc**發現受保護的檔案已遭到覆寫，則會從**systemroot\system32\dllcache**資料夾抓取檔案的正確版本，然後取代不正確的檔案。
-   Windows Server 2003、Windows Server 2008 和 Windows Server 2008 R2 上的**sfc**之間有功能上的差異：
-   如需有關 Windows Server 2003 上**sfc**的詳細資訊，請參閱 Microsoft 知識庫中的[文章 310747](https://go.microsoft.com/fwlink/?LinkId=227069) 。
-   如需有關 Windows Server 2008 和 Windows Server 2008 R2 上**sfc**的詳細資訊，請參閱[系統檔案檢查](https://go.microsoft.com/fwlink/?LinkId=227071)程式。

## <a name="examples"></a>範例
若要確認**kernel32.dll**檔案，請輸入：
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
若要設定離線修復**kernel32.dll**檔案，並將離線開機目錄設為**d：** ，並將離線 windows 目錄設為**d:\windows**，請輸入：
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

