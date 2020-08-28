---
title: 起司火鍋
description: Fondue 命令的參考文章，此命令會從 Windows Update 或群組原則所指定的其他來源下載必要的檔案，以啟用 Windows 選用功能。
ms.topic: reference
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 132441d48ce1f65b38955130fe19535775d02377
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035066"
---
# <a name="fondue"></a>起司火鍋

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從 Windows Update 或群組原則所指定的其他來源下載必要的檔案，以啟用 Windows 選用功能。 功能的資訊清單檔必須已經安裝在您的 Windows 映像中。

## <a name="syntax"></a>語法

```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootrequest}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /enable-feature`<feature_name>` | 指定您想要啟用之 Windows 選用功能的名稱。 每個命令列只能啟用一項功能。 若要啟用多個功能，請使用每個功能的 fondue.exe。 |
| /caller-name:`<program_name>` | 當您從腳本或批次檔呼叫 fondue.exe 時，指定程式或進程名稱。 如果發生錯誤，您可以使用此選項將程式名稱新增至 SQM 報告。 |
| /hide-ux:`{all | rebootrequest}` | 使用 [ **全部** ] 隱藏使用者的所有訊息，包括存取 Windows Update 的進度和許可權要求。 如果需要許可權，作業將會失敗。<p>使用 **rebootrequest** 只隱藏要求重新開機電腦之許可權的使用者訊息。 如果您有可控制重新開機要求的腳本，請使用此選項。 |

### <a name="examples"></a>範例

若要啟用 Microsoft .NET Framework 4.8，請輸入：

```
fondue.exe /enable-feature:NETFX4
```

若要啟用 Microsoft .NET Framework 4.8，請將程式名稱新增至 SQM 報表，而不會向使用者顯示訊息，請輸入：

```
fondue.exe /enable-feature:NETFX4 /caller-name:Admin.bat /hide-ux:all
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Microsoft .NET Framework 4.8 下載](https://dotnet.microsoft.com/download/dotnet-framework/net48)
