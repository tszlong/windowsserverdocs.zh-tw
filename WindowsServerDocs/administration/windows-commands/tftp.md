---
title: tftp
description: 在遠端電腦之間傳輸檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0abcdb0fc9aad7ed42abed66b6e8d4dbc843f86
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721436"
---
# <a name="tftp"></a>tftp

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在執行簡單檔案傳輸通訊協定（tftp）服務或 daemon 的遠端電腦（通常是執行 UNIX 的電腦）之間傳輸檔案。 tftp 通常是由內嵌裝置或從 tftp 伺服器開機過程中抓取固件、設定資訊或系統映射的系統所使用。   

## <a name="syntax"></a>語法  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|-i|指定二進位影像傳輸模式（也稱為「八位模式」）。 在二進位影像模式中，檔案是以一個位元組的單位傳輸。 傳送二進位檔案時，請使用此模式。 如果省略 **-i** ，則會以 ASCII 模式傳送檔案。 這是預設的傳輸模式。 此模式會將指定電腦的行尾（EOL）字元轉換為適當的格式。 傳送文字檔時，請使用此模式。 如果檔案傳輸成功，則會顯示資料傳送速率。|  
|\<主控件\>|指定本機或遠端電腦。|  
|put|將本機電腦上的檔案*來源*傳送到遠端電腦上的檔案*目的地*。 因為 tftp 通訊協定不支援使用者驗證，所以使用者必須登入遠端電腦，而且檔案必須可在遠端電腦上寫入。|  
|get|將遠端電腦上的檔案*目的地*傳送到本機電腦上的檔案*來源*。|  
|\<Source\>|指定要傳送的檔案。|  
|\<目的地\>|指定要將檔案傳送到何處。|  

## <a name="remarks"></a>備註  
-   您可以使用 [新增功能] [Wizard] 來安裝 tftp 用戶端。  
-   Tftp 通訊協定不支援任何驗證或加密機制，因此在出現時可能會導致安全性風險。 不建議針對連線到網際網路的系統安裝 tftp 用戶端。  
-   Tftp 用戶端是選用軟體，在 Windows Vista 和更新版本的 Windows 作業系統上會標示為已淘汰。 基於安全性理由，Microsoft 不再提供 tftp 伺服器服務。  

## <a name="examples"></a>範例  
從遠端電腦**Host1**複製檔**開機。**  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
