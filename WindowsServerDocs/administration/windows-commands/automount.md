---
title: 自動掛接
description: 自動掛接命令的參考文章，可啟用或停用自動掛接功能。
ms.topic: reference
ms.assetid: 4635fc91-a477-4f17-8dcc-aa08854bfe45
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f4be54f7610929627e0cc0332a7f5d65eafc8b2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632997"
---
# <a name="automount"></a>自動掛接

適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

- [命令列語法關鍵](command-line-syntax-key.md)

> [!IMPORTANT]
> 在存放區域網路 (SAN) 設定，停用自動掛接可防止 Windows 自動掛接或指派磁碟機號給系統可見的任何新基本磁碟區。

## <a name="syntax"></a>語法

自動掛接 [{enable | disable | 拖拽}] [noerr]

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| enable | 可讓 Windows 自動掛接新增至系統的新基本和動態磁碟區，並指派磁碟機號。 |
| disable | 防止 Windows 自動掛接任何新增至系統的新基本和動態磁碟區。<p>**注意**：停用自動掛接可能會導致容錯移轉叢集無法驗證設定向導的儲存部分。 |
| scrub | 移除磁碟區掛接點目錄，並登錄不再位於系統中之磁碟區的設定。 這防止了自動掛接先前在系統中的磁碟區，並在它們重新新增回系統時，給予它們先前的磁碟區掛接點。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要查看是否已啟用自動掛接功能，請在 diskpart 命令中輸入下列命令：

```
automount
```

若要啟用自動掛接功能，請輸入：

```
automount enable
```

若要停用自動掛接功能，請輸入：

```
automount disable
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskpart 命令](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc770877(v%3dws.11))
