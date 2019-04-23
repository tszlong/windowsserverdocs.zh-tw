---
title: bdehdcfg driveinfo
description: '適用於 Windows 命令主題 * * bdehdcfg: driveinfo * *-會顯示磁碟機代號、 的總大小、 最大的可用空間，以及分割區的特性。'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4aa041c27b1797e7d00476212887a7dc6dbc1880
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889059"
---
# <a name="bdehdcfg-driveinfo"></a>Bdehdcfg: driveinfo

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示磁碟機代號、 總大小、 最大的可用空間和分割區的特性。 列出有效的磁碟分割。 四個主要或延伸磁碟分割存在時，不會列出未配置的空間。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。
## <a name="syntax"></a>語法
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<DriveLetter>|指定磁碟機代號，後面接著冒號。|
## <a name="remarks"></a>備註
此命令只是參考，並不會對磁碟機進行任何修改。
## <a name="BKMK_Examples"></a>範例
下列範例會顯示 c 磁碟機的磁碟機資訊
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
