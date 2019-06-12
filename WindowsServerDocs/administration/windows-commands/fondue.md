---
title: fondue
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bcbbbf80f25f77d1feb83f358401e4d14da3d354
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439226"
---
# <a name="fondue"></a>fondue

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

從 Windows Update 或另一個群組原則所指定的來源下載所需的檔案，可讓 Windows 選用功能。 在您的 Windows 映像時，必須已安裝此功能的資訊清單檔案。 
## <a name="syntax"></a>語法
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>參數

|              參數              |                                                                                                                                                                     描述                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /featurename: <*feature_name*>   |                                                                               指定您想要啟用的 Windows 選擇性功能的名稱。 您可以只讓每個命令列的一項功能。 若要啟用多項功能，請使用 fondue.exe 每項功能。                                                                                |
|    /caller-name: <*sys.sysprocesses*>    |                                                                                 當您從指令碼或批次檔呼叫 fondue.exe 時，請指定程式或處理序名稱。 如果發生錯誤的程式名稱加入 SQM 報表，您可以使用此選項。                                                                                 |
| /hide-ux:{all &#124; rebootRequest} | 使用**所有**隱藏所有訊息都包括進度和權限的要求，若要存取 Windows Update 的使用者。 如果需要的權限，則作業會失敗。<br /><br />使用**rebootRequest**只能隱藏要求重新啟動電腦的權限的使用者訊息。 如果控制項重新開機要求的指令碼，請使用此選項。 |

## <a name="BKMK_Examples"></a>範例
若要啟用 Microsoft.NET Framework 3.5，請輸入：
```
fondue.exe /enable-feature:NETFX3
```
若要啟用 Microsoft.NET Framework 3.5，新增的程式名稱 SQM 報表，並顯示訊息給使用者，也就是型別：
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
  ## <a name="see-also"></a>另請參閱
  [Microsoft.NET Framework 3.5 部署考量](https://go.microsoft.com/fwlink/?LinkId=248869)
