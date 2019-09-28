---
title: manage-bde WipeFreeSpace
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 35d5f5fe079485d35fa412502bec745a136fbce9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373784"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde.wsfWipeFreeSpace



抹除磁片區上的可用空間，移除可能已存在於空間中的任何資料片段。 在使用「僅限使用的空間」加密方法加密的磁片區上執行這個命令，會提供與「完整磁片區加密」加密方法相同的保護層級。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive >|代表磁碟機號，後面接著冒號、磁片區 GUID 路徑或掛接的磁片區。|
|-取消|取消清除正在處理的可用空間。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<名稱 >|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="BKMK_Examples"></a>典型

下列範例說明如何使用 **-w**命令來建立抹除 C 磁片磁碟機上的可用空間。
```
manage-bde -w C:
```
下列範例說明如何使用 **-w**命令搭配 **-cancel**參數來取消抹除磁片磁碟機 C 上的可用空間。
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)