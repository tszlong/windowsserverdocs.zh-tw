---
title: serverweroptin
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29545be99b14042d16a6f3a4118e0746f18b14ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869639"
---
# <a name="serverweroptin"></a>serverweroptin

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

可讓您啟用錯誤報告。
## <a name="syntax"></a>語法
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/query|確認目前的設定。|
|/ 詳細|傳送自動詳細報告。|
|/summary|自動傳送摘要報告。|
|/?|在命令提示字元顯示說明。|
## <a name="BKMK_Examples"></a>範例
若要確認目前的設定，請輸入：
```
serverweroptin /query
```
若要自動傳送詳細的報告，請輸入：
```
serverweroptin /detailed
```
若要自動傳送摘要報告，請輸入
```
serverweroptin /summary
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)

