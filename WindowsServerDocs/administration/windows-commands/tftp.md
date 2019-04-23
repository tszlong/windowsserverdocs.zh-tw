---
title: tftp
description: 來回傳輸檔案從遠端電腦。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad195409076840fda0e8d6bf5cd0c295a62cdede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845899"
---
# <a name="tftp"></a>tftp

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

傳輸檔案與遠端電腦時，通常執行 UNIX 中，執行簡單式檔案傳輸通訊協定 (tftp) 服務或精靈的電腦。 tftp 通常會使用內嵌的裝置或系統在開機程序，從 tftp 伺服器擷取韌體、 設定資訊或系統映像。   

## <a name="syntax"></a>語法  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|-i|指定二進位檔映像傳輸模式 （也稱為八位元模式）。 在二進位檔映像模式中，會將檔案傳輸以位元組為單位。 傳送二進位檔案時，請使用此模式。 如果 **-i**已省略，檔案會以 ASCII 模式傳輸。 這是預設的傳輸模式。 此模式會將行結尾 (EOL) 字元轉換成適當的格式指定的電腦。 傳送文字檔案時，請使用此模式。 如果檔案傳輸成功，則會顯示的資料傳輸速率。|  
|\<主應用程式\>|指定的本機或遠端電腦。|  
|put|會將檔案傳送*來源*檔案在本機電腦上*目的地*遠端電腦上。 因為 tftp 通訊協定不支援使用者驗證，使用者必須登入遠端電腦，並必須要在遠端電腦上的可寫入的檔案。|  
|取得|會將檔案傳送*目的地*檔案以在遠端電腦上*來源*本機電腦上。|  
|\<Source\>|指定要傳送的檔案。|  
|\<目的地\>|指定傳輸檔案的位置。|  

## <a name="remarks"></a>備註  
-   您可以安裝 tftp 用戶端會使用新增功能精靈。  
-   Tftp 通訊協定不支援任何驗證或加密機制，並因此可能導致安全性風險存在時。 Tftp 用戶端不是建議安裝適用於連線到網際網路的系統。  
-   Tftp 用戶端是選用軟體，並標示為已被取代，在 Windows Vista 和更新版本的 Windows 作業系統上。 由 Microsoft 基於安全性理由不再提供 tftp 伺服器服務。  

## <a name="BKMK_Examples"></a>範例  
將檔案複製**boot.img**從遠端電腦**Host1**。  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
