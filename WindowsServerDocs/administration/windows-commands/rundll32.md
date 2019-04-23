---
title: rundll32
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b1f288d21a1dcac25ecc00f685ea179d8a6542f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835029"
---
# <a name="rundll32"></a>rundll32



載入並執行 32 位元動態連結程式庫 (Dll)。 有 Rundll32 沒有可設定的設定。 [說明] 資訊供您執行的特定 DLL **rundll32**命令。

您必須執行**rundll32**命令從提升權限的命令提示字元。 若要開啟提升權限的命令提示字元，請按一下**開始**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**。

## <a name="syntax"></a>語法

```
Rundll32 <DLLname>
```

## <a name="commands"></a>命令

|參數|描述|
|---------|-----------|
|[rundll32 printui.dll,PrintUIEntry](rundll32-printui.md)|會顯示印表機使用者介面|

## <a name="remarks"></a>備註

Rundll32 只能從 DLL 呼叫 Rundll32 明確撰寫呼叫函式。 如需有關 Rundll32 需求，請參閱[文章 164787](https://go.microsoft.com/fwlink/?LinkID=165773)在 Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkID=165773)。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)