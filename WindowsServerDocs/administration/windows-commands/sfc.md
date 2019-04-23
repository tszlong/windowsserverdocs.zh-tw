---
title: sfc
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1db0ab81c9469c88ddb64a367a9dc98a1fd9b70c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832389"
---
# <a name="sfc"></a>sfc

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

掃描，並確認所有受保護的系統完整性檔，並使用正確的版本取代不正確的版本。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/scannow|掃描所有受保護的系統檔案的完整性，並修復問題時可能的檔案。|
|/verifyonly|掃描所有受保護的系統檔案的完整性。 修復會不執行任何作業。|
|/scanfile|掃描指定的檔案的完整性，並偵測到問題時，可能的話，修復的檔案。|
|\<file>|指定完整路徑和檔案名稱|
|/verifyfile|驗證指定的檔案完整性。 修復會不執行任何作業。|
|/offwindir|指定離線 windows 目錄的 離線修復的位置。|
|/offbootdir|離線瀏覽指定離線開機目錄的位置|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您必須執行的系統管理員群組的成員身分登入**sfc.exe**。
-   如果**sfc**探索已覆寫受保護的檔案，它會擷取從檔案的正確版本**systemroot\system32\dllcache**資料夾，然後再取代不正確的檔案。
-   功能之間的差異**sfc** Windows Server 2003、 Windows Server 2008 和 Windows Server 2008 R2:
-   如需詳細資訊**sfc** Windows Server 2003，請參閱[文章 310747](https://go.microsoft.com/fwlink/?LinkId=227069) Microsoft 知識庫中。
-   如需詳細資訊**sfc**在 Windows Server 2008 和 Windows Server 2008 R2，請參閱[系統檔案檢查程式](https://go.microsoft.com/fwlink/?LinkId=227071)。

## <a name="BKMK_examples"></a>範例
若要確認**kernel32.dll 檔案**，型別：
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
安裝程式的離線修復的**kernel32.dll**與設為離線開機目錄的檔案**d:** ] 和 [設為離線 windows 目錄**d:\windows**，型別：
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)

