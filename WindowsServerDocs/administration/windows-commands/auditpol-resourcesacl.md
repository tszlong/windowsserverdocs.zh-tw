---
title: auditpol resourceSACL
description: '**Auditpol resourceSACL**的 Windows 命令主題，會設定全域資源系統存取控制清單（sacl）。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b004be6f21cd076fe20e73c45268731c35d654e5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851151"
---
# <a name="auditpol-resourcesacl"></a>auditpol resourceSACL

設定全域資源系統存取控制清單（Sacl）。

> [!NOTE]
> 僅適用于 Windows 7 和 Windows Server 2008 R2。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

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

## <a name="arguments"></a>引數

| 引數 | 描述 |
| -------- | ----------- | 
| /type | 正在設定其物件存取審核的資源。 支援的區分大小寫、引數值為檔案（適用于目錄和檔案）和金鑰（適用于登錄機碼）。 |
| /success | 指定成功的審核。 |
| /failure | 指定失敗的審核。 |
| /user | 以下列其中一種形式指定使用者：<ul><li> DomainName\Account （例如 DOM\Administrators）</li><li>StandaloneServer\Group 帳戶（請參閱[LookupAccountName 函數](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx)）</li><li>{S-1-x-x-x-x}（x 以十進位表示，整個 SID 必須以大括弧括住）。 例如： {S-1-5-21-5624481-130208933-164394174-1001}<p>**注意：** 如果使用 SID 表單，則不會進行檢查以確認此帳戶是否存在。</li></ul> |
| /access | 指定可透過下列指定的許可權遮罩：<p>一般存取權限，包括：<ul><li>GA-全部通用</li><li>GR-一般讀取</li><li>GW-一般寫入</li><li>GX-一般執行</li></ul><p>檔案的存取權限，包括：<ul><li>FA-檔案所有存取</li><li>FR-檔案一般讀取</li><li>FW-檔案一般寫入</li><li>FX-檔案一般執行</li></ul><p>登錄機碼的存取權限，包括：<ul><li>KA-金鑰所有存取</li><li>KR-金鑰讀取</li><li>KW-金鑰寫入</li><li>KX-金鑰執行</li></ul><p>例如： `/access:FRFW` 將會啟用讀取和寫入作業的 audit 事件。<p>代表存取遮罩的十六進位值（例如0x1200a9）<p>    當您使用不屬於安全描述項定義語言（SDDL）標準的資源特定位元遮罩時，這會很有用。 如果省略，則會使用完整存取權。 |

## <a name="remarks"></a>備註

針對 resourceSACL 作業，您必須在安全描述項中設定該物件的寫入或完整控制許可權。 您也可以透過擁有 [管理] [**審核及安全性記錄檔**] （SeSecurityPrivilege）使用者權限來執行 resourceSACL 作業。 不過，此許可權允許執行移除作業所不需要的其他存取權。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

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