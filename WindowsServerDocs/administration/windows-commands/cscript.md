---
title: cscript
description: Cscript 命令的參考文章，它會啟動腳本，使其在命令列環境中執行。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7f6731c264fc5a22bee2d94b41a555431e48b42
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928836"
---
# <a name="cscript"></a>cscript

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動要在命令列環境中執行的腳本。

>[!IMPORTANT]
> 執行此工作不必有系統管理認證， 因此基於安全性最佳做法，請考慮以不具系統管理認證的使用者身分執行此工作。

## <a name="syntax"></a>語法

```
cscript <scriptname.extension> [/b] [/d] [/e:<engine>] [{/h:cscript | /h:wscript}] [/i] [/job:<identifier>] [{/logo | /nologo}] [/s] [/t:<seconds>] [x] [/u] [/?] [<scriptarguments>]
```

#### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| scriptname 擴充功能 | 指定具有選擇性副檔名之腳本檔案的路徑和檔案名。 |
| /b | 指定不會顯示警示、腳本錯誤或輸入提示的批次模式。 |
| /d | 啟動偵錯工具。 |
| /e:`<engine>` | 指定用來執行腳本的引擎。 |
| /h： cscript | 註冊 cscript.exe 做為執行腳本的預設腳本主機。 |
| /h： wscript.echo | 註冊 wscript.exe 做為執行腳本的預設腳本主機。 此為預設值。 |
| /i | 指定互動模式，以顯示警示、腳本錯誤和輸入提示。 這是的預設值，相反的 `/b` 。 |
| /工作 x<identifier> | 執行 manage-bde.wsf 腳本檔案中的*識別碼*所識別的作業。 |
| /logo | 指定在執行腳本之前，主控台中顯示 Windows 腳本主機橫幅。 這是的預設值，相反的 `/nologo` 。 |
| /nologo | 指定在腳本執行之前，不顯示 Windows Script 主機橫幅。 |
| /s | 儲存目前使用者目前的命令提示字元選項。 |
| /t:<seconds> | 指定腳本可執行檔時間上限（以秒為單位）。 您最多可以指定32767秒。 預設值為 [無時間限制]。 |
| /U | 針對從主控台重新導向的輸入和輸出，指定 Unicode。 |
| /x | 啟動偵錯工具中的腳本。 |
| /? | 顯示可用的命令參數，並提供使用它們的協助。 這等同于輸入沒有參數的**cscript.exe** ，而且沒有任何腳本。 |
| scriptarguments | 指定傳遞至腳本的引數。 每個腳本引數前面必須加上斜線（ **/** ）。 |

#### <a name="remarks"></a>備註

- 每個參數都是選擇性的;不過，您不能指定腳本引數，而不指定腳本。 如果您未指定腳本或任何腳本引數，cscript.exe 會顯示 cscript.exe 語法和有效的主機選項。

- **/T**參數會藉由設定計時器來防止過多的腳本執行。 當執行時間超過指定的值時，cscript 會中斷腳本引擎並結束進程。

- Windows 腳本檔案通常會有下列其中一個副檔名： manage-bde.wsf、.vbs、.js。 Windows Script Host 可以使用 manage-bde.wsf 腳本檔案。 每個 manage-bde.wsf 檔案都可以使用多個腳本引擎並執行多個作業。

- 如果您按兩下副檔名沒有關聯的腳本檔案，[**開啟方式**] 對話方塊隨即出現。 選取 [wscript.echo] 或 [cscript]，然後選取 [**一律使用此程式] 來開啟此檔案類型**。 這會針對此檔案類型的檔案，註冊 wscript.exe 或 cscript 做為預設的腳本主機。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
