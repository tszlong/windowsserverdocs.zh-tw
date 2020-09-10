---
title: ftp prompt
description: Ftp prompt 命令的參考文章，此命令會開啟和關閉提示模式。
ms.topic: reference
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d045e72ace4364b1ab8832efa35728dc3af2b123
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624766"
---
# <a name="ftp-prompt"></a>ftp prompt

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

開啟和關閉提示模式。 預設會開啟 [提示模式]。 如果開啟提示模式，ftp 命令會在多個檔案傳輸期間提示，讓您可以選擇性地取出或儲存檔案。

> [!NOTE]
> 當提示模式關閉時，您可以使用 [ftp mget](ftp-mget.md) 和 [ftp mput](ftp-mput_1.md) 命令來傳送所有檔案。

## <a name="syntax"></a>語法

```
prompt
```

### <a name="examples"></a>範例

若要切換開啟和關閉提示模式，請輸入：

```
prompt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
