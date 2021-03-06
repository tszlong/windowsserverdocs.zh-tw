---
title: manage-bde tpm
description: Manage-bde tpm 命令的參考文章，會設定電腦的信賴平臺模組 (TPM) 。
ms.topic: reference
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d108deffb12048241704e643574698d25ac60a06
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635262"
---
# <a name="manage-bde-tpm"></a>manage-bde tpm

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定電腦的信賴平臺模組 (TPM) 。

## <a name="syntax"></a>語法

```
manage-bde -tpm [-turnon] [-takeownership <ownerpassword>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -homeautomation.turnon | 啟用並啟用 TPM，以允許設定 TPM 擁有者密碼。 您也可以使用 **-t** 作為此命令的縮寫版本。 |
| -takeownership | 藉由設定擁有者密碼來取得 TPM 的擁有權。 您也可以使用 **-o** 作為此命令的縮寫版本。 |
| `<ownerpassword>` | 代表您為 TPM 指定的擁有者密碼。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要開啟 TPM，請輸入：

```
manage-bde  tpm -turnon
```

若要取得 TPM 的擁有權，並將擁有者密碼設定為 *0wnerP@ss* ，請輸入：

```
manage-bde  tpm  takeownership 0wnerP@ss
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows PowerShell 的 TPM 管理 Cmdlet](/powershell/module/trustedplatformmodule/)

- [manage-bde 命令](manage-bde.md)
