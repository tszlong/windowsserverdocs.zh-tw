---
title: auditpol resourceSACL
description: Auditpol resourceSACL 命令的參考文章，此命令會將全域資源系統存取控制清單設定 (Sacl) 。
ms.topic: reference
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 408dadfd29fb1dd6227d4d27651da400d1ede333
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028996"
---
# <a name="auditpol-resourcesacl"></a>auditpol resourceSACL

> 適用于： Windows 7 和 Windows Server 2008 R2

設定全域資源系統存取控制清單 (Sacl) 。

若要執行 *resourceSACL* 作業，您必須擁有安全描述項中該物件集的 [ **寫入** ] 或 [ **完全控制** ] 許可權。 如果您有 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege]) 使用者權限，也可以執行*resourceSACL*作業。

## <a name="syntax"></a>語法

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /set | 針對指定的資源類型，在資源 SACL 中新增或更新現有的專案。 |
| /remove | 移除全域物件存取審核清單中指定使用者的所有專案。 |
| /clear | 從全域物件存取審核清單中移除所有專案。|
| /view | 列出資源 SACL 中的全域物件存取審核專案。 使用者和資源類型是選擇性的。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="arguments"></a>引數

| 引數 | 描述 |
| -------- | ----------- |
| /type | 要設定其物件存取審核的資源。 支援、區分大小寫的引數值是目錄和檔案的 *檔案 (，*) 和登錄機碼) 的 *金鑰* (。 |
| /success | 指定成功的審核。 |
| /failure | 指定失敗的審核。 |
| /user | 以下列其中一種形式指定使用者：<ul><li> DomainName\Account (例如 DOM\Administrators) </li><li>StandaloneServer\Group 帳戶 (請參閱 [LookupAccountName 函數](/windows/win32/api/winbase/nf-winbase-lookupaccountnamea)) </li><li>{S-1-x-x x x x} (x 以十進位表示，整個 SID 必須以大括弧) 括住。 例如： {S-1-5-21-5624481-130208933-164394174-1001}<p>**注意：** 如果使用 SID 表單，則不會進行檢查以確認此帳戶是否存在。</li></ul> |
| /access | 指定可透過下列指定的許可權遮罩：<p>一般存取權限，包括：<ul><li>GA-一般全部</li><li>GR-泛型讀取</li><li>GW-一般寫入</li><li>GX-泛型 EXECUTE</li></ul><p>檔案的存取權限，包括：<ul><li>FA-檔案所有存取</li><li>FR-FILE 泛型 READ</li><li>FW-檔案一般寫入</li><li>FX-FILE GENERIC EXECUTE</li></ul><p>登錄機碼的存取權限，包括：<ul><li>KA-金鑰所有存取權</li><li>KR-讀取的金鑰</li><li>KW-金鑰寫入</li><li>KX-KEY EXECUTE</li></ul><p>例如： `/access:FRFW` 啟用讀取和寫入作業的 audit 事件。<p>代表存取遮罩 (的十六進位值，例如 0x1200a9) <p>當使用不是安全描述項定義語言一部分的資源特定位元遮罩 (SDDL) 標準時，這會很有用。 如果省略，則會使用完整存取。 |

## <a name="examples"></a>範例

若要設定全域資源 SACL，以在登錄機碼上對使用者進行成功的存取嘗試：

```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```

若要設定全域資源 SACL，以在使用者執行檔案或資料夾的一般讀取和寫入功能時，審核成功和失敗的嘗試：

```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```

移除檔案或資料夾的所有全域資源 SACL 專案：

```
auditpol /resourceSACL /type:File /clear
```

若要從檔案或資料夾中移除特定使用者的所有全域資源 SACL 專案：

```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```

若要列出在檔案或資料夾上設定的全域物件存取審核專案：

```
auditpol /resourceSACL /type:File /view
```

若要列出特定使用者在檔案或資料夾上設定的全域物件存取審核專案：

```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
