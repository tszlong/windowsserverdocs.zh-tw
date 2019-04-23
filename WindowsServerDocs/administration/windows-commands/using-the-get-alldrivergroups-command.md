---
title: 使用 get AllDriverGroups 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 236a2f798fb07ee6eafb9baf9314dbf46a984cdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873999"
---
# <a name="using-the-get-alldrivergroups-command"></a>使用 get AllDriverGroups 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示伺服器上的所有驅動程式群組的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[] / [顯示: {PackageMetaData&#124;篩選&#124;所有}]|顯示指定群組中的所有驅動程式套件的中繼資料。 **PackageMetaData**顯示驅動程式群組的所有篩選條件的相關資訊。 **篩選**會顯示所有的驅動程式套件的中繼資料和群組的篩選。|
## <a name="BKMK_examples"></a>範例
若要檢視驅動程式檔案的相關資訊，請輸入：
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get DriverGroup 命令](using-the-get-drivergroup-command.md)
