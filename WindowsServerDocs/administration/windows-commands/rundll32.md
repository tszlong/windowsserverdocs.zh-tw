---
title: rundll32
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0639206b26ea58c4ec8473c0a736fda3c435021
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722242"
---
# <a name="rundll32"></a>rundll32



載入並執行32位動態連結程式庫（Dll）。 沒有可設定的 Rundll32.exe 設定。 系統會提供您使用**rundll32.exe**命令執行之特定 DLL 的說明資訊。

您必須從提高許可權的命令提示字元執行**rundll32.exe**命令。 若要開啟提升許可權的命令提示字元，請按一下 [**開始**]，以滑鼠右鍵按一下 [**命令提示**字元]，然後按一下 [以**系統管理員**身分

## <a name="syntax"></a>語法

```
Rundll32 <DLLname>
```

## <a name="commands"></a>命令

|參數|描述|
|---------|-----------|
|[Rundll32.exe printui.dll .dll、PrintUIEntry](rundll32-printui.md)|顯示印表機使用者介面|

## <a name="remarks"></a>備註

Rundll32.exe 只能從明確寫入要由 Rundll32.exe 所呼叫的 DLL 中呼叫函式。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
