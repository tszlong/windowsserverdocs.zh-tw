---
title: manage-bde WipeFreeSpace
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc252bd0d9d8227badb35bafea96575e37fca243
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820788"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde： WipeFreeSpace



抹除磁片區上的可用空間，移除可能已存在於空間中的任何資料片段。 在使用 [僅限使用的空間加密] 方法加密的磁片區上執行這個命令，會提供與完整磁片區加密加密方法相同的保護層級。

## <a name="syntax"></a>語法

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|\<磁片磁碟機>|代表磁碟機號，後面接著冒號、磁片區 GUID 路徑或掛接的磁片區。|
|-取消|取消清除正在處理的可用空間。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<Name>|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="examples"></a>範例

說明如何使用 **-w**命令來建立抹除磁片磁碟機 C 上的可用空間。
```
manage-bde -w C:
```
說明使用 **-w**命令搭配 **-cancel**參數來取消抹除磁片磁碟機 C 上的可用空間。
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)