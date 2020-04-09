---
title: manage-bde 狀態
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c8248333944b030dc8868ba4408024727a08c3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839841"
---
# <a name="manage-bde-status"></a>manage-bde：狀態



提供電腦上所有磁片磁碟機的下列資訊;是否受 BitLocker 保護：
-   大小
-   BitLocker 版本
-   轉換狀態
-   已加密百分比
-   加密方法
-   保護狀態
-   鎖定狀態
-   識別欄位
-   金鑰保護裝置

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁片磁碟機 >|表示後面接著冒號的磁碟機號。|
|-protectionaserrorlevel|當磁片區受到保護時，會導致 Manage-bde 命令列工具傳送傳回碼0，而當磁片區未受保護時，則傳回 1;最常用於批次腳本，用來判斷磁片磁碟機是否受 BitLocker 保護。 您也可以使用 **-p**作為此命令的縮寫版本。|
|-computername|指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<名稱 >|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="examples"></a><a name=BKMK_Examples></a>典型

下列範例說明如何使用 **-status**命令來顯示磁片磁碟機 C 的狀態。
```
manage-bde –status C:
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)