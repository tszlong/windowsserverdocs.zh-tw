---
title: msinfo32
description: Msinfo32 命令的參考文章，此命令會開啟系統資訊工具，以顯示本機電腦上完整的硬體、系統元件和軟體環境的觀點。
ms.topic: reference
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30a1cdc45ff9c6efad94f620148ffdf1bf00492d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025242"
---
# <a name="msinfo32"></a>msinfo32

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

開啟 [系統資訊] 工具，以顯示本機電腦上的完整硬體、系統元件和軟體環境的觀點。

某些系統資訊類別包含大量的資料。 您可以使用 [ **開始/wait** ] 命令，將這些類別的報告效能優化。 如需詳細資訊，請參閱 [系統資訊](/previous-versions/windows/it-pro/windows-server-2003/cc783305(v=ws.10))。

## <a name="syntax"></a>語法

```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computername>] [/showcategories] [/category <categoryID>] [/categories {+<categoryID>(+<categoryID>)|+all(-<categoryID>)}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<path>` | 指定要以 *C:\Folder1\File1.xxx*格式開啟的檔案，其中 *C* 是磁碟機號、 *Folder1* 是資料夾、 *File1* 是檔案名，而 *xxx* 是副檔名。<p>這個檔案可以是 **.nfo**、 **.xml**、 **.txt**或 **.cab** 檔。 |
| `<computername>` | 指定目標或本機電腦的名稱。 這可以是 UNC 名稱、IP 位址或完整的電腦名稱稱。 |
| `<categoryID>` | 指定類別目錄專案的識別碼。 您可以使用 **/showcategories**取得類別目錄識別碼。 |
| /pch | 顯示系統資訊工具中的 [系統歷程記錄]。 |
| /nfo | 將匯出的檔案儲存為 **.nfo** 檔。 如果 *路徑* 中指定的檔案名結尾不是 **.nfo** 副檔名，則會自動將 **.nfo** 副檔名附加至檔案名。 |
| /report | *以文字檔的形式儲存*盤案。 檔案名的儲存方式與 *路徑*中所顯示的名稱完全相同。 除非在 path 中指定 .txt 副檔名，否則不會附加至該檔案。 |
| /computer | 啟動指定遠端電腦的系統資訊工具。 您必須擁有適當的許可權才能存取遠端電腦。 |
| /showcategories | 啟動具有所有可用類別識別碼的系統資訊工具，而不是顯示易記或當地語系化的名稱。 例如，[軟體環境] 類別會顯示為 [ **SWEnv** ] 類別。 |
| /類別 | 從選取的指定分類開始系統資訊。 使用 **/showcategories** 來顯示可用類別識別碼的清單。 |
| /categories | 啟動僅顯示指定類別或類別的系統資訊。 它也會將輸出限制為選取的類別或類別。 使用 **/showcategories** 來顯示可用類別識別碼的清單。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要列出可用的類別識別碼，請輸入：

```
msinfo32 /showcategories
```

若要啟動系統資訊工具，並顯示所有可用資訊（載入的模組除外），請輸入：

```
msinfo32 /categories +all -loadedmodules
```

若要顯示 **系統摘要** 資訊，並建立名為 *syssum*的 .nfo 檔案（其中包含 **系統摘要** 分類中的資訊），請輸入：

```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```

若要顯示資源衝突資訊，以及建立名為 *衝突*的 .nfo 檔案，其中包含資源衝突的相關資訊，請輸入：

```
msinfo32 /nfo conflicts.nfo /categories +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
