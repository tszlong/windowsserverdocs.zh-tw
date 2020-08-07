---
title: auditpol resourceSACL
description: Auditpol resourceSACL 命令的參考文章，其會設定全域資源系統存取控制清單 (Sacl) 。
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b20c97fce42bb57613ac421eabc5ac7acf7a921d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895356"
---
# <a name="auditpol-resourcesacl"></a>auditpol resourceSACL

> 適用于： Windows 7 和 Windows Server 2008 R2

設定 (Sacl) 的全域資源系統存取控制清單。

若要執行*resourceSACL*作業，您必須在安全描述項中擁有該物件集的**寫入**或**完整控制**許可權。 如果您有 [管理] [) ] 使用者權限的 [**管理審核及安全性記錄**檔 (]，也可以執行*resourceSACL*作業。

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
| /set | 針對指定的資源類型，將新專案新增至或更新資源 SACL 中的現有專案。 |
| /remove | 移除全域物件存取審核清單中指定使用者的所有專案。 |
| /clear | 移除全域物件存取審核清單中的所有專案。|
| /view | 列出資源 SACL 中的全域物件存取審核專案。 使用者和資源類型都是選擇性的。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="arguments"></a>引數

| 引數 | 描述 |
| -------- | ----------- |
| /type | 正在設定其物件存取審核的資源。 支援的、區分大小寫的引數值是目錄和檔案的*檔案 (，*) 和登錄機碼) 的*金鑰* (。 |
| /success | 指定成功的審核。 |
| /failure | 指定失敗的審核。 |
| /user | 以下列其中一種形式指定使用者：<ul><li> DomainName\Account (例如 DOM\Administrators) </li><li>StandaloneServer\Group 帳戶 (參閱[LookupAccountName 函數](/windows/win32/api/winbase/nf-winbase-lookupaccountnamea)) </li><li>{S-1-x-x-x-x} (x 以十進位表示，整個 SID 必須以大括弧括住) 。 例如： {S-1-5-21-5624481-130208933-164394174-1001}<p>**注意：** 如果使用 SID 表單，則不會進行檢查以確認此帳戶是否存在。</li></ul> |
| /access | 指定可透過下列指定的許可權遮罩：<p>一般存取權限，包括：<ul><li>GA-全部通用</li><li>GR-一般讀取</li><li>GW-一般寫入</li><li>GX-一般執行</li></ul><p>檔案的存取權限，包括：<ul><li>FA-檔案所有存取</li><li>FR-檔案一般讀取</li><li>FW-檔案一般寫入</li><li>FX-檔案一般執行</li></ul><p>登錄機碼的存取權限，包括：<ul><li>KA-金鑰所有存取</li><li>KR-金鑰讀取</li><li>KW-金鑰寫入</li><li>KX-金鑰執行</li></ul><p>例如： `/access:FRFW` 啟用讀取和寫入作業的 audit 事件。<p>代表存取遮罩 (的十六進位值，例如 0x1200a9) <p>當您使用不屬於安全描述項定義語言的資源特定位元遮罩 (SDDL) standard 時，這會很有用。 如果省略，則會使用完整存取權。 |

## <a name="examples"></a>範例

若要設定全域資源 SACL，以在登錄機碼上對使用者進行成功的存取嘗試：

```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```

若要設定全域資源 SACL，以審核使用者在檔案或資料夾上執行一般讀取和寫入功能的成功和失敗嘗試：

```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```

若要移除檔案或資料夾的所有全域資源 SACL 專案：

```
auditpol /resourceSACL /type:File /clear
```

若要從檔案或資料夾移除特定使用者的所有全域資源 SACL 專案：

```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```

列出檔案或資料夾上設定的全域物件存取審核專案：

```
auditpol /resourceSACL /type:File /view
```

若要列出針對檔案或資料夾所設定之特定使用者的全域物件存取審核專案：

```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
