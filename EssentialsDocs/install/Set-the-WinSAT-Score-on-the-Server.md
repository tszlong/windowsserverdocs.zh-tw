---
title: "設定伺服器的 WinSAT 分數"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 77866acccac13ac48da8779700c8654f2c7f3277
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="set-the-winsat-score-on-the-server"></a>設定伺服器的 WinSAT 分數

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您應該會執行 Windows Server Essentials 的作業系統最佳化的影片的串流解析度伺服器設定 WinSAT CPU 分數。 建立及安裝.xml 檔案，包含 WinSAT 分數資訊來執行此動作。  
  
## <a name="obtain-the-winsat-cpu-score"></a>取得 WinSAT CPU 分數  
 您可以用 OPK 稱為 WinServerSAT.exe 探索 WinSAT CPU 分數和進入作業系統讀取 WinServerSAT.xml 檔案資訊的程式。  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>若要取得 WinSAT CPU 分數  
  
1.  在 [ADK 媒體複製 Resources\WinServerSAT\\ * 參考電腦。  
  
2.  參考在電腦上，開放提升權限的命令提示字元視窗。  
  
3.  如果 %ProgramFiles%\Windows Server\Bin\OEM 資料夾不存在，輸入下列命令，然後按 Enter 鍵。  
  
     **mkdir 」 %ProgramFiles%\Windows Server\Bin\OEM 」**  
  
4.  輸入下列命令，然後按 Enter 鍵。  
  
     **WinServerSAT.exe 」 %ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml 」**  
  
 下列範例顯示 XML 到建立 WinServerSAT.xml 檔案。  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 其中*WinSAT_Score*會取代發現在伺服器的值。  
  
> [!IMPORTANT]
>  您必須移除 WinServerSAT.exe、 winsat.prx、 winsat.wmv，以及 WinSATEncode.wmv 檔案參照電腦之前擷取映像。
