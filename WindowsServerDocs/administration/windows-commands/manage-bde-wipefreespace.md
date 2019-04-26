---
title: 管理 bde WipeFreeSpace
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8750094e7357a3aefa307d24abd1470fbf8d2a71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867169"
---
# <a name="manage-bde-wipefreespace"></a>管理 power shell:WipeFreeSpace



抹除移除已存在的空間中的任何資料片段的磁碟區上的可用空間。 磁碟區上執行此命令使用 「 使用空間只加密？ 加密方法會提供相同的層級的保護 「 完整磁碟區加密？ 加密方法。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde –WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive>|表示磁碟機代號，後面接著冒號、 磁碟區 GUID 路徑或掛接的磁碟區。|
|-Cancel|取消抹除程序中的可用空間。|
|-computername|指定 bde.exe 用以修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。|
|\<名稱 >|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或 /？|顯示在命令提示字元中，簡短說明。|
|-help 或-h|顯示在命令提示字元完成說明。|

## <a name="BKMK_Examples"></a>範例

下列範例說明如何利用 **-w**命令來建立抹除 c 磁碟機上的可用空間
```
manage-bde -w C:
```
下列範例說明如何利用 **-w**命令搭配 **– 取消**參數，以取消清除 c 磁碟機上的可用空間
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)