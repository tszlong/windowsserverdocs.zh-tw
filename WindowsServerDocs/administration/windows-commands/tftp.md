---
title: tftp
description: Tftp 命令的參考文章，此命令會在遠端電腦之間傳輸檔案。
ms.topic: reference
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 89a4edb69753ae34814d13f90f21332c6f748244
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717905"
---
# <a name="tftp"></a>tftp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從遠端電腦（通常是執行 UNIX 的電腦）傳輸檔案到遠端電腦，這通常是執行一般檔案傳輸通訊協定 (tftp) service 或 daemon 的電腦。 tftp 通常是由內嵌裝置或系統從 tftp 伺服器開機程式期間，用來取出固件、設定資訊或系統映射的系統使用。

> 須知Tftp 通訊協定不支援任何驗證或加密機制，因此可能會在存在時帶來安全性風險。 針對連線到網際網路的系統，不建議安裝 tftp 用戶端。 基於安全性考慮，Microsoft 不再提供 tftp 伺服器服務。

## <a name="syntax"></a>語法

```
tftp [-i] [<host>] [{get | put}] <source> [<destination>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -i | 指定二進位影像傳輸模式 (也稱為八位模式) 。 在二進位影像模式中，檔案會以一個位元組單位傳送。 傳送二進位檔案時，請使用此模式。 如果您未使用 **-i** 選項，檔案會以 ASCII 模式傳輸。 這是預設的傳輸模式。 這個模式會將行尾 (EOL) 字元轉換成指定電腦的適當格式。 傳送文字檔時，請使用此模式。 如果檔案傳輸成功，則會顯示資料傳輸速率。 |
| `<host>` | 指定本機或遠端電腦。 |
| get | 將遠端電腦上的檔案 *目的地* 傳送到本機電腦上的檔案 *來源* 。 |
| put | 將本機電腦上的檔案 *來源* 傳送至遠端電腦上的檔案 *目的地* 。 因為 tftp 通訊協定不支援使用者驗證，所以使用者必須登入遠端電腦，而且檔案必須可以在遠端電腦上寫入。 |
| `<source>` | 指定要傳送的檔案。 |
| `<destination>` | 指定要傳送檔案的位置。 |

## <a name="examples"></a>範例

若要從遠端電腦*Host1*複製 [ *img* ]，請輸入：

```
tftp  -i Host1 get boot.img
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
