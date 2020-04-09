---
title: autochk
description: 適用于**autochk**的 Windows 命令主題，會在電腦啟動時以及 Windows Server 開始驗證檔案系統的邏輯完整性時執行。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30ccdbb6aad6e116988ae9c782e3ff359642453e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851121"
---
# <a name="autochk"></a>autochk

會在電腦啟動時執行，且在 Windows Server&reg; 2008 R2 之前，會開始確認檔案系統的邏輯完整性。

**Autochk**是只在 NTFS 磁片上執行的**Chkdsk**版本，而且只有在 Windows Server 2008 R2 啟動之前。 無法直接從命令列執行**Autochk** 。 而是在下列情況下執行**Autochk** ：

- 如果您嘗試在開機磁碟區上執行**Chkdsk** 。

- 如果**Chkdsk**無法取得磁片區的獨佔使用。

- 如果磁片區標示為「中途」。

## <a name="remarks"></a>備註

> [!WARNING]
> 不能從命令列直接執行**Autochk**命令列工具。 相反地，請使用**Chkntfs**命令列工具來設定您想要在啟動時執行**Autochk**的方式。
> -  您可以搭配 **/x**參數使用**Chkntfs** ，以防止在特定磁片區或多個磁片區上執行**Autochk** 。
>
> - 使用**Chkntfs**命令列工具搭配 **/t**參數，將 Autochk 延遲從0秒變更為最多3天（259200秒）。 不過，長時間延遲表示電腦不會啟動到經過的時間，或直到您按下按鍵來取消**Autochk**為止。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Chkdsk](chkdsk.md)

- [Chkntfs](chkntfs.md)

- [磁片和檔案系統的疑難排解](https://go.microsoft.com/fwlink/?LinkId=4527)