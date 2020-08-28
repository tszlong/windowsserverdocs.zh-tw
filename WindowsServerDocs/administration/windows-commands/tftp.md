---
title: tftp
description: 在遠端電腦之間傳輸檔案。
ms.topic: reference
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94884e2ae992441bb0031e25f9aeebe5984c207d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038276"
---
# <a name="tftp"></a>tftp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從遠端電腦（通常是執行 UNIX 的電腦）傳輸檔案到遠端電腦，這通常是執行一般檔案傳輸通訊協定 (tftp) service 或 daemon 的電腦。 tftp 通常是由內嵌裝置或系統從 tftp 伺服器開機程式期間，用來取出固件、設定資訊或系統映射的系統使用。

## <a name="syntax"></a>語法
```
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]
```

#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|-i|指定二進位影像傳輸模式 (也稱為八位模式) 。 在二進位影像模式中，檔案會以一個位元組單位傳送。 傳送二進位檔案時，請使用此模式。 如果省略 **-i** ，則會以 ASCII 模式傳送檔案。 這是預設的傳輸模式。 這個模式會將行尾 (EOL) 字元轉換成指定電腦的適當格式。 傳送文字檔時，請使用此模式。 如果檔案傳輸成功，則會顯示資料傳輸速率。|
|\<Host\>|指定本機或遠端電腦。|
|put|將本機電腦上的檔案 *來源* 傳送至遠端電腦上的檔案 *目的地* 。 因為 tftp 通訊協定不支援使用者驗證，所以使用者必須登入遠端電腦，而且檔案必須可以在遠端電腦上寫入。|
|get|將遠端電腦上的檔案 *目的地* 傳送到本機電腦上的檔案 *來源* 。|
|\<Source\>|指定要傳送的檔案。|
|\<Destination\>|指定要傳送檔案的位置。|

## <a name="remarks"></a>備註
-   您可以使用 [新增功能] 嚮導來安裝 tftp 用戶端。
-   Tftp 通訊協定不支援任何驗證或加密機制，因此可能會在存在時帶來安全性風險。 針對連線到網際網路的系統，不建議安裝 tftp 用戶端。
-   Tftp 用戶端是選用軟體，並在 Windows Vista 和更新版本的 Windows 作業系統上標示為已淘汰。 基於安全性考慮，Microsoft 不再提供 tftp 伺服器服務。

## <a name="examples"></a>範例
從遠端電腦**Host1**複製 [檔] **。**
```
tftp  -i Host1 get boot.img
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
