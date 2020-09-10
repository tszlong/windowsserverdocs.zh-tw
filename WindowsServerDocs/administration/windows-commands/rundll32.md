---
title: rundll32
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 10834c063db561f45b3920378188fca506841dcf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637192"
---
# <a name="rundll32"></a>rundll32



載入並執行32位動態連結程式庫 (Dll) 。 沒有可設定的 Rundll32.exe 設定。 系統會提供您使用 **rundll32.exe** 命令執行之特定 DLL 的說明資訊。

您必須從提升許可權的命令提示字元中執行 **rundll32.exe** 命令。 若要開啟提高許可權的命令提示字元，請按一下 [ **開始**]，以滑鼠右鍵按一下 **命令提示**字元，然後按一下 [以 **系統管理員身分執行**]

## <a name="syntax"></a>語法

```
Rundll32 <DLLname>
```

## <a name="commands"></a>命令

|參數|描述|
|---------|-----------|
|[Rundll32.exe printui.dll、PrintUIEntry](rundll32-printui.md)|顯示印表機使用者介面|

## <a name="remarks"></a>備註

Rundll32.exe 只能從明確撰寫的 DLL 呼叫函式，以由 Rundll32.exe 呼叫。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
