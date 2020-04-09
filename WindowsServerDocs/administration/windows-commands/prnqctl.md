---
title: prnqctl
description: 列印測試頁、暫停或繼續印表機。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d07d8caa0568b26f5edc16258085a59ecdafcf4e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837201"
---
# <a name="prnqctl"></a>prnqctl

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列印測試頁、暫停或繼續印表機，以及清除印表機佇列。  

## <a name="syntax"></a>語法  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
### <a name="parameters"></a>參數  

|參數|描述|  
|-------|--------|  
|-z|在使用 **-p**參數指定的印表機上暫停列印。|  
|-m|在使用 **-p**參數指定的印表機上繼續列印。|  
|-e|在以 **-p**參數指定的印表機上列印測試頁。|  
|-x|取消以 **-p**參數指定的印表機上的所有列印工作。|  
|-s \<ServerName >|指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。|  
|-p \<printerName >|指定您想要管理的印表機名稱。 必要。|  
|-u \<UserName >-w \<密碼 >|指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
- **Prnqctl**命令是位於%windir%\system32\ printing_Admin_Scripts\\<language> 目錄中的 Visual Basic 腳本。 若要使用此命令，請在命令提示字元中輸入**cscript** ，後面接著 prnqctl 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如，  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- 如果您提供的資訊包含空格，請使用引號括住文字（例如 `"computer Name"`）。  

## <a name="examples"></a><a name="BKMK_examples"></a>典型  
若要在 \\\Server1 電腦共用的 Laserprinter1 印表機上列印測試頁，請輸入：  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
若要在本機電腦的 Laserprinter1 印表機上暫停列印，請輸入：  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
若要取消本機電腦上 Laserprinter1 印表機上的所有列印工作，請輸入：  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
[列印命令參考](print-command-reference.md)  
