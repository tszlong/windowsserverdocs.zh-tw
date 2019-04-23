---
title: prnqctl
description: 列印測試頁、 暫停或繼續印表機。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 26af9527b7b16b42fd9d389f3409143dfc3e9aa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858079"
---
# <a name="prnqctl"></a>prnqctl

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

列印測試頁、 暫停或繼續印表機，以及清除印表機佇列。  
  
## <a name="syntax"></a>語法  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|-z|暫停具有指定的印表機的列印 **-p**參數。|  
|-m|繼續使用指定的印表機上列印 **-p**參數。|  
|-e|使用指定的印表機上列印測試頁 **-p**參數。|  
|-x|使用指定的印表機上的所有列印工作會取消 **-p**參數。|  
|-s\<伺服器名稱 >|指定裝載的印表機，您想要管理遠端電腦的名稱。 如果您沒有指定的電腦，則會使用本機電腦。|  
|-p \<printerName>|指定您想要管理印表機的名稱。 必要。|  
|-u \<UserName> -w \<Password>|指定具有連線至裝載的印表機，您想要管理之電腦的權限的帳戶。 目標電腦的本機 Administrators 群組的所有成員都有這些權限，但可以也對其他使用者授與權限。 如果您未指定帳戶，您必須登入具有可使用該指令的這些權限的帳戶。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
-   **Prnqctl**命令是 Visual Basic 指令碼位於 %WINdir%\System32\printing_Admin_Scripts\\ <language>目錄。 若要使用這個命令中，在命令提示字元中，輸入**cscript**後面 prnqctl 檔案或將目錄變更為適當的資料夾完整路徑。 例如:   
    ```  
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
    ```  
-   如果您提供的資訊包含空格，請使用引號括住的文字 (例如`"computer Name"`)。  

## <a name="BKMK_examples"></a>範例  
若要共用的 Laserprinter1 印表機上列印測試頁\\\Server1 電腦中，輸入：  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
若要暫停本機電腦上 Laserprinter1 印表機上的列印，請輸入：  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
若要取消 Laserprinter1 印表機上的本機電腦上的所有列印工作，請輸入：  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
[列印命令參考資料](print-command-reference.md)  
