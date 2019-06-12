---
title: lpr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddc64f958dcb38129d81becfc8950059e055d8e5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437486"
---
# <a name="lpr"></a>lpr

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將檔案傳送到電腦或印表機共用裝置執行線上印表機服務精靈 (LPD) 服務，以準備進行列印。  

## <a name="syntax"></a>語法  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | "-o l"] [-x] [-d] <filename>  
```  
## <a name="parameters"></a>參數  

|     參數      |                                                                                                           描述                                                                                                           |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  -S <ServerName>   |                                    電腦或印表機共用裝載的狀態，您想要顯示的 LPD 列印佇列的裝置，請指定 （依名稱或 IP 位址）。 必要。                                    |
|  -P <printerName>  |                                                              指定 （依名稱） 的印表機列印佇列，您想要顯示的狀態。 必要。                                                              |
| -C <BannerContent> |                指定要列印的列印工作的 [橫幅] 頁面上的內容。 如果您未包含此參數，傳送列印工作的來源電腦的名稱會出現在 [橫幅] 頁面中。                 |
|    -J <JobName>    |                           指定 [橫幅] 頁面列印的列印工作名稱。 如果您未包含此參數，列印檔案的名稱會出現在 [橫幅] 頁面中。                            |
| [-o&#124; "-o l"]  | 指定您想要列印的檔案類型。 參數 **-o**指定您想要列印文字檔案。 參數 **「-o l"** 指定您想要列印的二進位檔案 （例如 PostScript 檔案）。 |
|         -d         |              指定的控制檔案之前，必須傳送資料的資料檔案。 如果您的印表機需要要先傳送的資料檔案，請使用此參數。 如需詳細資訊，請參閱您印表機的文件。               |
|         -x         |                               指定**lpr**命令必須與作業系統 （稱為 SunOS） 版本最多並包括 4.1.4_u1 Sun Microsystems 相容。                                |
|     <FileName>     |                                                                                      指定要列印的檔案 （依名稱）。 必要。                                                                                      |
|         /?         |                                                                                              在命令提示字元顯示說明。                                                                                               |

## <a name="remarks"></a>備註  
- 若要尋找的印表機名稱，開啟 [印表機] 資料夾。  
- **-S**， **-P**， **-C**，以及 **-J**參數會區分大小寫，而且必須以大寫字母輸入。  
  ## <a name="BKMK_examples"></a>範例  
  此範例示範如何在 10.0.0.45 LPD 主機上列印至 Laserprinter1 印表機佇列的".txt"文字檔案：  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
  ```  
  此範例示範如何在 10.0.0.45 LPD 主機上列印 Laserprinter1 印表機佇列的 「 PostScript_file.ps 「 Adobe PostScript 檔案：  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 "-o l" PostScript_file.ps  
  ```  

#### <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
-   [列印命令參考資料](print-command-reference.md)  
