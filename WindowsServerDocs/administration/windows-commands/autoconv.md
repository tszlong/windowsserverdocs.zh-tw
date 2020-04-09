---
title: autoconv
description: 適用于**autoconv**的 Windows 命令主題，它會將檔案分配表（Fat）和 Fat32 磁片區轉換為 NTFS 檔案系統。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe0e388a1d4fd79567ef0562197e3181bbbc46f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851111"
---
# <a name="autoconv"></a>autoconv

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案分配表（Fat）和 Fat32 磁片區轉換為 NTFS 檔案系統，在執行**autochk**之後，讓現有的檔案和目錄保持不變。 轉換成 NTFS 檔案系統的磁片區無法轉換回 Fat 或 Fat32。

## <a name="remarks"></a>備註

您無法在命令列上執行**autoconv** 。 只有在透過**convert**設定時，才會在啟動時執行。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [autochk](autochk.md)

- [convert](convert.md)

- [使用檔案系統](https://go.microsoft.com/fwlink/?LinkId=4509)
