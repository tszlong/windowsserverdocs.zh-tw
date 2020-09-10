---
title: cscript
description: Cscript 命令的參考文章，此命令會啟動腳本，以便在命令列環境中執行。
ms.topic: reference
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e35efeccc219a7e678e2eccab74de5d0c4d6837
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628995"
---
# <a name="cscript"></a>cscript

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動腳本以在命令列環境中執行。

>[!IMPORTANT]
> 執行此工作不必有系統管理認證， 因此基於安全性最佳做法，請考慮以不具系統管理認證的使用者身分執行此工作。

## <a name="syntax"></a>語法

```
cscript <scriptname.extension> [/b] [/d] [/e:<engine>] [{/h:cscript | /h:wscript}] [/i] [/job:<identifier>] [{/logo | /nologo}] [/s] [/t:<seconds>] [x] [/u] [/?] [<scriptarguments>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| scriptname 副檔名 | 使用選擇性的副檔名指定腳本檔案的路徑和檔案名。 |
| /b | 指定不顯示警示、腳本錯誤或輸入提示的批次模式。 |
| /d | 啟動偵錯工具。 |
| /e:`<engine>` | 指定用來執行腳本的引擎。 |
| /h： cscript | 將 cscript.exe 註冊為執行腳本的預設腳本主機。 |
| /h： wscript.echo | 將 wscript.exe 註冊為執行腳本的預設腳本主機。 此為預設值。 |
| /i | 指定可顯示警示、腳本錯誤和輸入提示的互動模式。 這是預設值，相反的 `/b` 。 |
| 工作<identifier> | 執行 w2kmiguser.wsf 腳本檔案中的 *識別碼* 所識別的作業。 |
| /logo | 指定在執行腳本之前，主控台中顯示 Windows Script Host 橫幅。 這是預設值，相反的 `/nologo` 。 |
| /nologo | 指定在執行腳本之前，不會顯示 Windows Script Host 橫幅。 |
| /s | 為目前的使用者儲存目前的命令提示字元選項。 |
| /t:<seconds> | 指定腳本可執行檔時間上限 (（以秒為單位）) 。 您最多可以指定32767秒。 預設值為 [無時間限制]。 |
| /U | 指定從主控台重新導向之輸入和輸出的 Unicode。 |
| /x | 在偵錯工具中啟動腳本。 |
| /? | 顯示可用的命令參數，並提供使用這些參數的說明。 這與輸入不含參數的 **cscript.exe** 和沒有腳本的方式相同。 |
| scriptarguments | 指定傳遞給腳本的引數。 每個腳本引數前面都必須加上斜線 (**/**) 。 |

#### <a name="remarks"></a>備註

- 每個參數都是選擇性的;不過，您不能指定腳本引數，而不需要指定腳本。 如果您未指定腳本或任何腳本引數，cscript.exe 會顯示 cscript.exe 語法和有效的主機選項。

- **/T**參數藉由設定計時器來防止腳本過多執行。 當執行時間超過指定的值時，cscript 會中斷腳本引擎並結束進程。

- Windows 腳本檔案通常會有下列副檔名之一：. w2kmiguser.wsf、.vbs、.js。 Windows Script Host 可以使用. w2kmiguser.wsf 腳本檔案。 每個 w2kmiguser.wsf 檔案都可以使用多個腳本引擎並執行多個作業。

- 如果您按兩下副檔名沒有關聯的指令檔，[ **開啟** 檔案] 對話方塊隨即出現。 選取 [wscript.echo] 或 [cscript]，然後選取 [ **一律使用這個程式] 來開啟此檔案類型**。 這會針對這個檔案類型的檔案，將 wscript.exe 或 cscript 註冊為預設腳本主機。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
