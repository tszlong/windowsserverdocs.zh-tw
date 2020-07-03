---
title: serverceipoptin
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2430907237fd82dc6788c8b68f4de35629f5f35
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935923"
---
# <a name="serverceipoptin"></a>serverceipoptin

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您參與客戶經驗改進計畫（CEIP）。
## <a name="syntax"></a>語法
```
serverceipoptin [/query] [/enable] [/disable]
```
#### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|/query|驗證目前的設定。|
|/enable|啟用參與。|
|/disable|停用參與。|
|/?|在命令提示字元顯示說明。|
## <a name="examples"></a>範例
若要確認目前的設定，請輸入：
```
serverceipoptin /query
```
若要啟用參與，請輸入：
```
serverceipoptin /enable
```
若要停用參與，請輸入：
```
serverceipoptin /disable
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

