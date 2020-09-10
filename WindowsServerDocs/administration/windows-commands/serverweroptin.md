---
title: serverweroptin
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73bdce2a4b18f55239b61132df0417b02cb683c2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637787"
---
# <a name="serverweroptin"></a>serverweroptin

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您啟用錯誤報表。
## <a name="syntax"></a>語法
```
serverweroptin [/query] [/detailed] [/summary]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/query|驗證目前的設定。|
|/detailed|自動傳送詳細的報表。|
|/summary|自動傳送摘要報告。|
|/?|在命令提示字元顯示說明。|
## <a name="examples"></a>範例
若要確認目前的設定，請輸入：
```
serverweroptin /query
```
若要自動傳送詳細報告，請輸入：
```
serverweroptin /detailed
```
若要自動傳送摘要報告，請輸入
```
serverweroptin /summary
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

