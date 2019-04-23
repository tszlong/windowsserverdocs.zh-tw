---
title: msinfo32
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3de5088b64105e970fc38f55ecaf54382670549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843459"
---
# <a name="msinfo32"></a>msinfo32

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

開啟 系統資訊工具，以顯示本機電腦上的硬體、 系統元件和軟體環境的全方位檢視。 
## <a name="syntax"></a>語法
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<path>|指定要被開啟格式檔案*C:\Folder1\File1.XXX*，其中*C*是磁碟機代號*Folder1*是資料夾， *File1*是檔案名稱，並*XXX*是檔案名稱副檔名。<br /><br />這個檔案可以是 **.nfo**， **.xml**， **.txt**，或 **.cab**檔案。|
|<computerName>|指定目標或本機電腦的名稱。 這可以是 UNC 名稱、 IP 位址或完整電腦名稱。|
|<CategoryID>|指定類別目錄項目的識別碼。 您可以使用，以取得類別目錄識別碼 **/showcategories**。|
|/pch|在 系統資訊工具，會顯示系統歷程記錄檢視。|
|/nfo|儲存匯出的檔案 **.nfo**檔案。 中指定的檔名，如果*路徑*結尾不是 **.nfo**延伸模組 **.nfo**延伸模組會自動附加至檔案名稱。|
|/report|將檔案中的儲存*路徑*為文字檔案。 檔案名稱會儲存在出現的相同*路徑*。 .Txt 副檔名不會附加至檔案，除非它在路徑中指定。|
|/computer|啟動指定的遠端電腦的系統資訊工具。 您必須具有適當的權限以存取遠端電腦。|
|/showcategories|使用所有可用的類別識別碼顯示，啟動 系統資訊工具，而不是顯示的易記或當地語系化的名稱。 比方說，軟體環境 類別目錄會顯示為**SWEnv**類別目錄。|
|/category|開始所選取的指定類別中的系統資訊。 使用 **/showcategories**以顯示可用的類別識別碼的清單。|
|/categories|開始只指定的類別或顯示的類別目錄的系統資訊。 它也會限制對選取的類別或類別的輸出。 使用 **/showcategories**以顯示可用的類別識別碼的清單。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
某些系統資訊類別目錄包含大量的資料。 您可以使用**start /wait**命令，以將這些類別報告的效能最佳化。 如需詳細資訊，請參閱 <<c0> [ 系統資訊](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx)。
## <a name="BKMK_Examples"></a>範例
若要列出可用的類別識別碼，請輸入：
```
msinfo32 /showcategories
```
若要啟動的所有可用資訊的系統資訊工具顯示，除了載入的模組，型別：
```
msinfo32 /categories +all -loadedmodules
```
若要只顯示系統摘要資訊，並建立.nfo 檔案稱為 syssum.nfo，其中包含在 [系統摘要] 類別中的資訊，請輸入：
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
若要顯示資源的衝突資訊，並建立稱為，其中包含資源的衝突資訊 conflicts.nfo.nfo 檔案，請輸入：
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)

