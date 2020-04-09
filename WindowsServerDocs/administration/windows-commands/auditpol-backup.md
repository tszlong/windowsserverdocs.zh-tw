---
title: auditpol 備份
description: 適用于**auditpol backup**的 Windows 命令主題，會備份系統稽核原則設定、所有使用者的每個使用者稽核原則設定，以及逗號分隔值（CSV）文字檔的所有審核選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8895f312606a6a6c45a77c659a1cd98d115babe3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851211"
---
# <a name="auditpol-backup"></a>auditpol 備份

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

備份系統稽核原則設定、所有使用者的每一使用者稽核原則設定，以及逗號分隔值（CSV）文字檔的所有 [審核選項]。

## <a name="syntax"></a>語法

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|-----------|------------- |
| /file | 指定將備份稽核原則之檔案的名稱。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

針對每個使用者原則和系統原則的備份作業，您必須在安全描述項中設定該物件的寫入或完全控制許可權。 您也可以透過擁有「**管理審核及安全性記錄檔**」（SeSecurityPrivilege）使用者權限來執行備份作業。 不過，此許可權允許不需要執行清單作業的其他存取權。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將所有使用者、系統稽核原則設定和所有審核選項的每個使用者稽核原則設定備份到名為 auditpolicy 的 CSV 格式文字檔，請輸入：

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> 如果未指定磁片磁碟機，則會使用目前的目錄。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
- [auditpol restore](auditpol-restore.md)
