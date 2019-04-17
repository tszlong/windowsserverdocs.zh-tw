---
title: "從 Windows Server Essentials 轉換到 Windows Server 2012 R2 標準"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d371e24b17310c0687666185f56fe07a135ff91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>從 Windows Server Essentials 轉換到 Windows Server 2012 R2 標準

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

Windows Server 2016 是時引進新的技術，可讓您輕鬆地轉換到雲端運算支援您目前的工作負載雲端準備作業系統。 Windows Server 2016 content 有助於讓您準備就緒。

 Windows Server Essentials 支援最多 25 位使用者和 50 裝置。 當您的企業需要超過這個限制時，您可以從 Windows Server Essentials 執行就地授權轉換到 Windows Server 2012 R2 標準保持不相容的授權。  
  
 您轉換到 Windows Server 2012 標準 R2 之後，移除使用者 account 和裝置限制，但之獨特功能（例如儀表板、遠端 Web 存取和 client 電腦備份）的 Windows Server essentials 仍然可用。 不過，技術限制這些功能的支援 100 帳號及 200 裝置的最大值。 Client 電腦的備份功能將會支援最多 75 裝置的備份。  
  
> [!IMPORTANT]
>   Windows Server 2012 標準 R2 每個使用者或您的環境中的裝置需要 client 存取授權 (CAL)。 這是與 Windows Server Essentials，這並不會使用 CAL 型號和並未隨附任何 Cal 不同。 當您從 Windows Server Essentials 轉換到 Windows Server 2012 標準 R2，您必須購買適當的數字和您的環境（針對大部分購買使用者 Cal）Cal 類型。  
  
## <a name="before-the-transition"></a>之前轉換  
  
-   之前的 Windows Server Essentials 轉換到 Windows Server 2012 標準 R2，您應該完整備份 server 的資料。  
  
    > [!IMPORTANT]
    >  Server 的完整備份，而您無法還原伺服器它已在之前轉換的狀態。  
  
-   此外，請確定您讀取並了解終端使用者授權合約 (EULA)，適用於 Windows Server 2012 標準 R2。 若要檢視使用者授權合約：  
  
    1.  以系統管理員身分開放在命令視窗。  
  
    2.  執行下列命令：  
  
         **dism /online /set-edition: ServerStandard /geteula:***使用者授權合約路徑*(其中*使用者授權合約路徑*代表您要儲存檔案使用者授權合約; 的位置，例如：C:\ws8std_eula.rtf)。 請務必.rtf 當做副檔名。  
  
    3.  打開儲存檔案的位置，然後按兩下 [打開該檔案。  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Windows Server 2012 R2 標準轉換  
 之後，您有要轉換的 Windows Server Essentials 到 Windows Server 2012 R2 標準，完成這兩個步驟：  
  
1.  適用於 Windows Server 2012 標準 R2 和適當數目的使用者及/或您的環境裝置 client 存取授權購買的授權。  
  
     您也可以購買授權的 Windows Server 2012 標準 R2 零售插座，代理商，或是的協助下[Microsoft 合作夥伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
    > [!NOTE]
    >  如果您購買最初的 Windows Server 2012 標準 R2 並執行安裝您有兩個 virtual 執行個體的 Windows Server Essentials 為您降級權限，您不需要任何其他購買。  
    >   
    >  如果您購買 Windows Server 2012 標準 R2 透過大量授權頻道，您可以針對 Windows Server 2012 R2 標準從大量授權服務中心 (VLSC) 下載 ISO 映像和 product 金鑰。  
    >   
    >  如果您購買 Windows Server 2012 標準 R2 從任何其他頻道，您可以從 Windows Server Essentials 的下載 ISO 映像和評估 product 鍵[TechNet 評估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 下一個步驟中所述執行轉換會將評估 product 轉換 product 完全授權與支援。  
  
2.  打開 Windows PowerShell 以系統管理員的身分，並執行下列命令：  
  
     **dism /online /set-edition: ServerStandard /accepteula /productkey:***Product 鍵*(其中*Product 鍵*是您的 Windows Server 2012 標準 R2 複本 product 鍵)。  
  
     伺服器重新開機才能完成轉換程序。  
  
 切換後, 的 Windows Server Essentials 功能維持在伺服器上和操之在 100 使用者和 200 裝置的支援。  
  
## <a name="see-also"></a>也了  
  

-   [伺服器資料移轉到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [伺服器資料移轉到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

