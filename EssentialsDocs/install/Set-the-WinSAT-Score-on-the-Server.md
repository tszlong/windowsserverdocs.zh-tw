---
title: 在伺服器上設定 WinSAT 分數
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819949"
---
# <a name="set-the-winsat-score-on-the-server"></a>在伺服器上設定 WinSAT 分數

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您應該執行 Windows Server Essentials 作業系統，以最佳化視訊串流解析度的伺服器設定 WinSAT CPU 分數。 建立並安裝含有 WinSAT 分數資訊的 .xml 檔案，即可達到此目的。  
  
## <a name="obtain-the-winsat-cpu-score"></a>取得 WinSAT CPU 分數  
 OPK 隨附一個名為 WinServerSAT.exe 的程式，這個程式會探索 WinSAT CPU 分數，並將該資訊放置在作業系統會讀取的 WinServerSAT.xml 檔案中。  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>若要取得 WinSAT CPU 分數  
  
1.  複製 Resources\WinServerSAT\\* 到參照電腦的 ADK 媒體中。  
  
2.  在參照電腦上，開啟提高權限的命令提示字元視窗。  
  
3.  如果 %ProgramFiles%\Windows Server\Bin\OEM 資料夾不存在，請輸入以下命令，然後按 Enter。  
  
     **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4.  輸入以下命令，然後按 Enter。  
  
     **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
 以下範例顯示已建立的 WinServerSAT.xml 檔案的 XML 內容。  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 其中 *WinSAT_Score* 會以在伺服器上探索的值取代。  
  
> [!IMPORTANT]
>  在擷取映像之前，您必須先將參照電腦上的 WinServerSAT.exe、winsat.prx、winsat.wmv 和 WinSATEncode.wmv 檔案移除。
