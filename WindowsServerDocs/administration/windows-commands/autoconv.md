---
title: autoconv
description: Autoconv 命令的參考文章，此命令會將檔案配置表 (Fat) 和 Fat32 磁片區轉換為 NTFS 檔案系統。
ms.topic: reference
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d69d70200b4885404486bc4956903a17b329630
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028936"
---
# <a name="autoconv"></a>autoconv

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案配置表 (Fat) 和 Fat32 磁片區轉換為 NTFS 檔案系統，在執行 **autochk** 之後，讓現有的檔案和目錄原封不動地在啟動時保持不變。 轉換為 NTFS 檔案系統的磁片區無法轉換回 Fat 或 Fat32。

> [!IMPORTANT]
> 您無法從命令列執行 **autoconv** 。 如果透過 **convert.exe**設定，則只能在啟動時執行。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [autochk 命令](autochk.md)

- [轉換命令](convert.md)
