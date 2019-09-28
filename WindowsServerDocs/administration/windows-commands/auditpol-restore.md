---
title: auditpol restore
description: '**Auditpol restore**的 Windows 命令主題-還原系統稽核原則設定、所有使用者的每一使用者稽核原則設定，以及與逗號分隔值（CSV）檔案格式一致之檔案中的所有審核選項由/備份選項使用。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b91f3745354c695c4ab0c71b429718bff05d8098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382406"
---
# <a name="auditpol-restore"></a>auditpol restore

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

針對所有使用者，還原系統稽核原則設定、每個使用者的稽核原則設定，以及與/備份選項所使用之逗號分隔值（CSV）檔案格式一致的檔案中所有的審核選項。

## <a name="syntax"></a>語法
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/file|指定應該從中還原稽核原則的檔案。 檔案必須已經使用/備份選項建立，或必須與/備份選項所使用的 CSV 檔案格式一致。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
針對每個使用者原則和系統原則的還原作業，您必須在安全描述項中設定該物件的「寫入」或「完全控制」許可權。 您也可以透過擁有 [管理] [**審核及安全性記錄檔**] （SeSecurityPrivilege）使用者權限來執行還原作業。 在發生意外錯誤或惡意攻擊的情況中還原安全描述項時，SeSecurityPrivilege 會很有用。
## <a name="BKMK_examples"></a>典型
若要針對所有使用者還原系統稽核原則設定、每個使用者的稽核原則設定，以及使用/備份命令所建立之名為 auditpolicy 之檔案中的所有審核選項，請輸入：
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[auditpol backup](auditpol-backup.md)
