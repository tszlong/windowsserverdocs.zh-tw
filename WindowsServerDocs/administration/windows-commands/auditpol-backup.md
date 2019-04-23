---
title: auditpol 備份
description: 適用於 Windows 命令主題**auditpol 備份**-備份系統稽核原則設定]、 [所有使用者和所有的稽核選項，以逗號分隔值 (CSV) 文字檔的每個使用者稽核原則設定。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78594c0445ae482e49d47b3b67bb867e53866017
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867949"
---
# <a name="auditpol-backup"></a>auditpol 備份

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

備份系統稽核原則設定]、 [所有使用者和所有的稽核選項，以逗號分隔值 (CSV) 文字檔的每個使用者稽核原則設定。

## <a name="syntax"></a>語法
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/file|指定要稽核原則將備份檔案的名稱。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
針對每位使用者的原則和系統原則的備份作業，您必須撰寫或該物件上的完全控制權限設定中的安全性描述元。 您也可以執行備份作業所擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 不過，此權限可讓其他不是執行清單作業所需的存取。
## <a name="BKMK_examples"></a>範例
若要備份每個使用者稽核原則設定的所有使用者，系統會稽核原則設定，以及 CSV 格式的文字檔案，名為 auditpolicy.csv，類型的所有稽核選項：
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> 如果未不指定任何磁碟機，則會使用目前的目錄。
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[auditpol 還原](auditpol-restore.md)
