---
title: 起司火鍋
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2e3b64ff05ab9a539b7734f37b7515c6f9ef123
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725592"
---
# <a name="fondue"></a>起司火鍋

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由從群組原則所指定的 Windows Update 或另一個來源下載所需的檔案，啟用 Windows 選用功能。 功能的資訊清單檔必須已經安裝在您的 Windows 映像中。 
## <a name="syntax"></a>語法
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
#### <a name="parameters"></a>參數

|              參數              |                                                                                                                                                                     描述                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /enable-feature： <*feature_name*>   |                                                                               指定您想要啟用之 Windows 選用功能的名稱。 您只能針對每個命令列啟用一項功能。 若要啟用多個功能，請針對每個功能使用 fondue。                                                                                |
|    /caller-name： <*program_name*>    |                                                                                 當您從腳本或批次檔呼叫 fondue 時，會指定程式或進程名稱。 如果發生錯誤，您可以使用此選項將程式名稱新增至 SQM 報告。                                                                                 |
| /hide-ux： {all &#124; rebootRequest} | 使用 [**全部**] 以隱藏使用者的所有訊息，包括存取 Windows Update 的進度和許可權要求。 如果需要許可權，作業將會失敗。<p>使用**rebootRequest**只會隱藏要求重新開機電腦許可權的使用者訊息。 如果您有可控制重新開機要求的腳本，請使用此選項。 |

## <a name="examples"></a>範例
若要啟用 Microsoft .NET Framework 3.5，請輸入：
```
fondue.exe /enable-feature:NETFX3
```
若要啟用 Microsoft .NET Framework 3.5，請將程式名稱新增至 SQM 報表，而不要向使用者顯示訊息，請輸入：
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>其他參考
- - [命令列語法關鍵](command-line-syntax-key.md)
  ## <a name="see-also"></a>另請參閱
  [Microsoft .NET Framework 3.5 部署考量](https://go.microsoft.com/fwlink/?LinkId=248869)
