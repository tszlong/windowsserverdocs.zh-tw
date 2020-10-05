---
title: wdsutil get-alldrivergroups
description: Wdsutil alldrivergroups 的參考文章，會顯示伺服器上所有驅動程式群組的相關資訊。
ms.topic: reference
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b616cb013b0f4f6913ee2fe6618dd893b989fda6
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729926"
---
# <a name="wdsutil-get-alldrivergroups"></a>wdsutil get-alldrivergroups

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有驅動程式群組的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Show： {PackageMetaData &#124; 濾波器 &#124; All}]|顯示指定群組中所有驅動程式套件的中繼資料。 **PackageMetaData** 會顯示驅動程式群組所有篩選器的相關資訊。 **篩選器** 會顯示群組之所有驅動程式套件和篩選器的中繼資料。|
## <a name="examples"></a>範例
若要查看驅動程式檔案的相關資訊，請輸入：
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-drivergroup 命令](wdsutil-get-drivergroup.md)
