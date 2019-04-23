---
title: auditpol 還原
description: 適用於 Windows 命令主題**auditpol 還原**-從語法一致使用逗號分隔的檔案還原系統稽核原則設定，每個使用者稽核原則設定，為所有使用者及所有的稽核選項/backup 所使用的值 (CSV) 檔案格式選項。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1961387083a8a61b27f3e44a2380a6060a02f98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868979"
---
# <a name="auditpol-restore"></a>auditpol 還原

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

從檔案 /backup 所使用的逗號分隔值 (CSV) 檔案格式與語法一致還原系統稽核原則設定，每個使用者稽核原則設定，所有使用者和所有的稽核選項的選項。

## <a name="syntax"></a>語法
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/file|指定應該從中還原的稽核原則的檔案。 該檔案必須已建立的使用 /backup 選項，或必須是語法配合 /backup 所使用的 CSV 檔案格式選項。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
針對每位使用者的原則和系統原則的還原作業，您必須撰寫或該物件上的完全控制權限設定中的安全性描述元。 您也可以執行還原作業所擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 發生意外的錯誤或惡意的攻擊時還原的安全性描述元時，適合使用 SeSecurityPrivilege。
## <a name="BKMK_examples"></a>範例
若要從名為 auditpolicy.csv 使用 /backup 所建立的檔案還原系統稽核原則設定，每個使用者稽核原則設定，所有使用者和所有的稽核選項的命令中，輸入：
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[auditpol 備份](auditpol-backup.md)
