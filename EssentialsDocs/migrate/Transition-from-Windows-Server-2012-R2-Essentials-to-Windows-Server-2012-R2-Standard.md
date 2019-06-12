---
title: 從 Windows Server 2012 Essentials 轉換到 Windows Server 2012 R2 Standard
description: 描述如何使用 Windows Server Essentials
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
ms.openlocfilehash: ca36533af169c899865789f153960bf5f0dda684
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432557"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>從 Windows Server 2012 Essentials 轉換到 Windows Server 2012 R2 Standard

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

Windows Server 2016 是雲端就緒作業系統，支援您目前的工作負載，同時還引入新的技術，可讓您輕鬆地轉換到雲端運算。 Windows Server 2016 內容可協助您做好準備。

 Windows Server Essentials 最多支援 25 位使用者及 50 台裝置。 當您的業務需求超過限制時，您可以從 Windows Server Essentials 中執行的就地授權轉換為 Windows Server 2012 R2 Standard 以維持授權相容性。  
  
 在轉換為 Windows Server 2012 R2 Standard 後，會移除使用者帳戶和裝置限制，但所特有 （例如儀表板、 遠端 Web 存取和用戶端電腦備份） 的 Windows Server Essentials 的功能仍然可使用。 不過，這些功能的技術限制最多支援 100 個使用者帳戶和 200 台裝置。 用戶端電腦的備份功能將支援最多 75 裝置的備份。  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard 每個使用者或裝置在您的環境中需要用戶端存取使用權 (CAL)。 這是不同於 Windows Server Essentials 中，不使用 CAL 模型，並不隨附任何 Cal。 當從 Windows Server Essentials 轉換到 Windows Server 2012 R2 Standard，您必須購買適當數量和類型的 Cal （大部分客戶是購買使用者 Cal） 環境。  
  
## <a name="before-the-transition"></a>轉換之前  
  
-   再從 Windows Server Essentials 轉換到 Windows Server 2012 R2 Standard，您應該完整備份伺服器資料。  
  
    > [!IMPORTANT]
    >  若未完整備份伺服器，就無法將伺服器還原成轉換之前的狀態。  
  
-   此外，請確認您已閱讀並了解針對 Windows Server 2012 R2 Standard 的終端使用者授權合約 (EULA)。 若要檢視 EULA：  
  
    1.  以系統管理員身分開啟命令視窗。  
  
    2.  執行下列命令：  
  
         **dism /online-/set-edition: ServerStandard /geteula:** *eula 路徑*(其中*eula 路徑*代表您要儲存 EULA 檔案的位置，例如：C:\ws8std_eula.rtf)。 務必使用 .rtf 做為副檔名。  
  
    3.  開啟您儲存檔案的位置，然後按兩下檔案將它開啟。  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>轉換到 Windows Server 2012 R2 Standard  
 之後，您已決定從 Windows Server Essentials 為 Windows Server 2012 R2 Standard，完成這兩個步驟的轉換：  
  
1. Windows Server 2012 R2 Standard 和適當數目的使用者和/或裝置用戶端存取使用權，為您的環境購買的授權。  
  
    向零售商、 經銷商或透過的協助，您可以針對 Windows Server 2012 R2 Standard 購買的授權[Microsoft 合作夥伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
   > [!NOTE]
   >  如果您購買 Windows Server 2012 R2 Standard 一開始，並執行降級權限，為 Windows Server Essentials 安裝其中一種將兩個虛擬執行個體，則您不需要購買任何其他項目。  
   >   
   >  如果大量授權通路購買 Windows Server 2012 R2 Standard，您可以針對 Windows Server 2012 R2 Standard 從大量授權服務中心 (VLSC) 下載的 ISO 映像和產品金鑰。  
   >   
   >  如果您購買 Windows Server 2012 R2 Standard 從所有其他通路，您可以從 Windows Server essentials 下載的 ISO 映像和評估產品金鑰[TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 執行下個步驟所述的轉換會將評估產品轉換成完整授權和支援的產品。  
  
2. 以系統管理員身分開啟 Windows PowerShell，然後執行下列命令：  
  
    **dism /online /set-edition:ServerStandard /accepteula /productkey:** *產品金鑰*(其中*產品金鑰*是您的 Windows Server 2012 R2 Standard 複本的產品金鑰)。  
  
    伺服器會重新啟動以完成轉換程序。  
  
   轉換之後，Windows Server Essentials 功能會保留在伺服器上，並支援最多 100 位使用者和 200 台裝置。  
  
## <a name="see-also"></a>另請參閱  
  

-   [移轉伺服器資料到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [移轉伺服器資料到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

