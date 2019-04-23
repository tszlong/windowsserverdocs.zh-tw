---
title: auditpol resourceSACL
description: 適用於 Windows 命令主題**uditpol resourceSACL** -會設定全域資源系統存取控制清單 (Sacl)。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 375f37250404dd6740027cb18959697626c1ffc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837489"
---
# <a name="auditpol-resourcesacl"></a>auditpol resourceSACL



設定全域資源系統存取控制清單 (Sacl)。

> [!NOTE]
> 僅適用於 Windows 7 和 Windows Server 2008 R2。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/set|加入新項目或更新現有資源的資源 SACL 中的項目指定的型別。|
|/remove|全域物件存取稽核清單中移除指定使用者的所有項目。|
|/ 清除|從全域物件存取稽核清單中移除所有項目。|
|/view|列出全域物件存取稽核項目資源 SACL 中。 使用者] 及 [資源類型是選擇性的。|
|/?|在命令提示字元顯示說明。|

## <a name="arguments"></a>引數

|引數|描述|
|--------|-----------|
|/type|哪一個物件存取稽核設定的資源。 支援的引數的值為檔案 （適用於目錄和檔案） 和金鑰 （適用於登錄機碼）。</br>注意：檔案與索引鍵的值會區分大小寫。|
|/success|指定成功稽核。|
|/failure|指定失敗稽核。|
|/user|指定的使用者，其中一種下列形式：</br>-DomainName\Account （例如 DOM\Administrators)</br>-StandaloneServer\Group 帳戶 (請參閱[LookupAccountName 函式](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} （以十進位、 表示 x 和整個 SID 必須括在大括號;）例如: {S-1-5-21-5624481-130208933-164394174-1001}</br>    注意：   如果使用 SID 表單，則不會檢查是為了確認此帳戶存在。|
|/access|指定可以在兩種形式的其中一個指定的權限遮罩：</br>-一系列簡單的權限：</br>    -一般存取權限：</br>        -   GA - GENERIC ALL</br>        -GR-一般讀取</br>        -   GW - GENERIC WRITE</br>        -GX-泛型執行</br>    檔案的存取權限：</br>        -FA-檔案的所有存取</br>        泛型-FR-檔案讀取</br>        -FW-檔案一般寫入</br>        -FX-檔案泛型執行</br>    登錄機碼的存取權限：</br>        -KA-索引鍵的所有存取</br>        讀取-韓國-機碼</br>        -千瓦-金鑰寫入</br>        -KX-機碼執行</br>    例如: ' / 存取： FRFW' 會啟用稽核事件的 讀取和寫入作業</br>-A 表示 （例如 0x1200a9) 的存取遮罩的十六進位值</br>    使用不屬於安全性描述元定義語言 (SDDL) 標準的特定資源的位元遮罩時，這非常有用。 如果省略，則會使用完整存取權。|

## <a name="remarks"></a>備註

ResourceSACL 作業，您必須寫入 」 或 「 完全控制 」 權限的安全性描述元中設定該物件。 您也可以執行 resourceSACL 作業擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 不過，此權限可讓其他不需要執行的移除作業的存取。

## <a name="BKMK_Examples"></a>範例

若要設定全域資源 SACL，若要稽核成功存取嘗試登錄機碼上的使用者：
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
若要設定全域資源 SACL，若要稽核成功和失敗的嘗試，由使用者執行一般讀取和寫入檔案或資料夾上的函式：
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
若要移除的檔案或資料夾的所有全域資源 SACL 項目：
```
auditpol /resourceSACL /type:File /clear
```
若要從檔案或資料夾移除特定使用者的所有全域資源 SACL 項目：
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
若要列出全域物件存取稽核檔案或資料夾上設定的項目：
```
auditpol /resourceSACL /type:File /view
```
若要列出全域物件存取特定使用者的稽核檔案或資料夾設定的項目：
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)