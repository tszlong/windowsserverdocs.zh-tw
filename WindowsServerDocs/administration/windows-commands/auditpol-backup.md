---
title: auditpol 備份
description: '**Auditpol backup**的 Windows 命令主題-備份系統稽核原則設定、所有使用者的每個使用者稽核原則設定，以及所有的審核選項到逗號分隔值（CSV）文字檔。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b98a05740d3ce1bfe14eda4c5d97ba6c09ff32
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382446"
---
# <a name="auditpol-backup"></a>auditpol 備份

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

備份系統稽核原則設定、所有使用者的每一使用者稽核原則設定，以及逗號分隔值（CSV）文字檔的所有 [審核選項]。

## <a name="syntax"></a>語法
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parameters

| 參數 |                                 描述                                 |
|-----------|-----------------------------------------------------------------------------|
|   /file   | 指定將備份稽核原則之檔案的名稱。 |
|    /?     |                    在命令提示字元顯示說明。                     |

## <a name="remarks"></a>備註
針對每個使用者原則和系統原則的備份作業，您必須在安全描述項中設定該物件的寫入或完全控制許可權。 您也可以透過擁有「**管理審核及安全性記錄檔**」（SeSecurityPrivilege）使用者權限來執行備份作業。 不過，此許可權允許不需要執行清單作業的其他存取權。
## <a name="BKMK_examples"></a>典型
若要將所有使用者、系統稽核原則設定和所有審核選項的每個使用者稽核原則設定備份到名為 auditpolicy 的 CSV 格式文字檔，請輸入：
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> 如果未指定磁片磁碟機，則會使用目前的目錄。
> #### <a name="additional-references"></a>其他參考
> [命令列語法索引鍵](command-line-syntax-key.md)
> [auditpol restore](auditpol-restore.md)
