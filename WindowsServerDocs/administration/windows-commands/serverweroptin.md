---
title: serverweroptin
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18b4a56888b3f23bf3bac4a12b2dba7079b50923
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834631"
---
# <a name="serverweroptin"></a>serverweroptin

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您啟用錯誤報表。
## <a name="syntax"></a>語法
```
serverweroptin [/query] [/detailed] [/summary]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/query|驗證目前的設定。|
|/detailed|自動傳送詳細報告。|
|/summary|自動傳送摘要報告。|
|/?|在命令提示字元顯示說明。|
## <a name="examples"></a><a name=BKMK_Examples></a>典型
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
-   - [命令列語法關鍵](command-line-syntax-key.md)

