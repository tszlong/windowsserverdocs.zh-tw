---
title: winrs
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9224e2572d7d5efded149cd113730dabc1624299
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843609"
---
# <a name="winrs"></a>winrs

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Windows 遠端管理可讓您管理和遠端執行程式。   
## <a name="syntax"></a>語法  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[/remote]:\<endpoint>|指定目標端點使用的 NetBIOS 名稱或標準連線：<br /><br />-   <url>: [\<transport>://]\<target>[:\<port>]<br /><br />如果未指定， **/r:localhost**用。|  
|/unencrypted]|指定遠端殼層的訊息將不會加密。 這是適用於疑難排解，或當使用已加密的網路流量**ipsec**，或強制執行實體安全性的時機。<br /><br />根據預設，訊息會使用 Kerberos 或 ntlm 驗證的金鑰來加密。<br /><br />選取 HTTPS 傳輸時，會忽略此命令列選項。|  
|/username]:\<使用者名稱 >|命令列上指定使用者名稱。<br /><br />如果未指定，工具會使用交涉驗證或命令提示字元的名稱。<br /><br />如果 **/username**指定，則 **/password**也必須指定。|  
|/password]:\<password>|命令列上指定密碼。<br /><br />如果 **/password**未指定，但 **/username**是，此工具會提示您輸入密碼。<br /><br />如果 **/password**指定，則 **/username**也必須指定。|  
|/timeout:\<seconds>|此選項已被取代。|  
|/ 目錄：\<路徑 >|指定遠端殼層的起始目錄。<br /><br />如果未指定，遠端殼層會在使用者的環境變數所定義的主目錄中啟動 **%USERPROFILE%**。|  
|/environment:\<string>=<value>|指定當殼層的殼層啟動時，以允許變更預設環境設定的單一環境變數。<br /><br />若要指定多個環境變數，必須使用這個參數的多個項目。|  
|/noecho|指定應該停用該回應。 這可能需要以確保遠端提示使用者的答案都不會在本機的顯示。<br /><br />預設回應是 「 開啟 」。|  
|/noprofile|指定未載入使用者設定檔。<br /><br />根據預設，伺服器會嘗試載入使用者設定檔。<br /><br />如果遠端使用者不是在目標系統上，本機系統管理員，則將會需要這個選項 （預設值就會產生錯誤）。|  
|/allowdelegate|指定的使用者的認證可用來存取遠端共用，例如，比目標端點的不同電腦上找到。|  
|/compression|開啟壓縮。  在遠端電腦上的舊版安裝可能不支援壓縮，因此它預設為關閉。<br /><br />預設設定已關閉，因為在遠端電腦上的舊版安裝可能不支援壓縮。|  
|/usessl|使用遠端端點時，請使用 SSL 連線。  指定此而不傳輸**https:** 會使用預設**WinRM**預設連接埠。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
-   簡短形式] 或 [完整格式，就會接受所有的命令列選項。 比方說，同時 **/r**並 **/遠端**有效。  
-   終止 **/遠端**命令時，使用者可以輸入**Ctrl + C**或是**ctrl-break**，這將會傳送至遠端殼層。 第二個**Ctrl + C**會強制終止**winrs.exe**。  
-   若要管理使用中的遠端殼層或 winrs 組態，請使用 WinRM 工具。  URI 是別名，以管理使用中的殼層**殼層/命令**。  Winrs 設定別名的 URI 是**winrm/config/winrs**。  

## <a name="BKMK_Examples"></a>範例  
```  
winrs /r:https://contoso.com command  
```  
```  
winrs /r:contoso.com /usessl command  
```  
```  
winrs /r:myserver command  
```  
```  
winrs /r:http://127.0.0.1 command  
```  
```  
winrs /r:http://169.51.2.101:80 /unencrypted command  
```  
```  
winrs /r:https://[::FFFF:129.144.52.38] command  
```  
```  
winrs /r:http://[1080:0:0:0:8:800:200C:417A]:80 command  
```  
```  
winrs /r:https://contoso.com /t:600 /u:administrator /p:$%fgh7 ipconfig  
```  
```  
winrs /r:myserver /env:path=^%path^%;c:\tools /env:TEMP=d:\temp config.cmd  
```  
```  
winrs /r:myserver netdom join myserver /domain:testdomain /userd:johns /passwordd:$%fgh789  
```  
```  
winrs /r:myserver /ad /u:administrator /p:$%fgh7 dir \\anotherserver\share  
```  

## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
