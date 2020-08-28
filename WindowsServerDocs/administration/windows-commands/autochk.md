---
title: autochk
description: Autochk 命令的參考文章，此命令會在電腦啟動時執行，且在 Windows Server 開始驗證檔案系統的邏輯完整性之前執行。
ms.topic: reference
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acdb9330ca1b99578c43c98e30c647171413ddd2
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028926"
---
# <a name="autochk"></a>autochk

在電腦啟動時以及 Windows Server 之前執行，以確認檔案系統的邏輯完整性。

**Autochk.exe** 是只在 NTFS 磁片上執行，而且只有在 Windows Server 啟動之前的 **chkdsk** 版本。 無法直接從命令列執行**autochk** 。 但是，在下列情況下，會執行 **autochk** ：

- 如果您嘗試在開機磁碟區上執行 **chkdsk** 。

- 如果 **chkdsk** 無法獲得磁片區的獨佔使用。

- 如果磁片區標示為中途。

## <a name="remarks"></a>備註

> [!WARNING]
> 無法直接從命令列執行 **autochk** 命令列工具。 相反地，請使用 **chkntfs** 命令列工具來設定要在啟動時執行 **autochk** 的方式。
>
> - 您可以搭配 **/x**參數使用**chkntfs** ，以防止在特定磁片區或多個磁片區上執行**autochk** 。
>
> - 使用 **chkntfs.exe** 命令列工具搭配 **/t** 參數，將 autochk 延遲從0秒變更為最多3天 (259200 秒) 。 不過，長時間延遲表示電腦在經過一段時間後才會啟動，直到您按下按鍵來取消 **autochk**為止。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [chkdsk 命令](chkdsk.md)

- [chkntfs 命令](chkntfs.md)
