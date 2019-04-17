---
title: "從 Windows Server Essentials 轉換到 Windows Server 2012 標準"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d2005b72adede72b718fa5b49b93435f5fbac1bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>從 Windows Server Essentials 轉換到 Windows Server 2012 標準

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

 Windows Server® 2012 Essentials 支援最多 25 位使用者和 50 裝置。 當您的企業需要超過這個限制時，您可以從 Windows Server Essentials 執行就地授權轉換到 Windows Server 2012 標準保持不相容的授權。  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>轉換如何影響的使用者與裝置限制  
 您轉換到 Windows Server 2012 標準之後，移除使用者 account 和裝置限制，但之獨特功能（例如儀表板、遠端網路存取和 client 電腦備份）、Windows Server essentials 仍然可用。 不過，技術限制這些功能的支援 75 帳號及 75 裝置的最大值。 如果您需要新增多個 75 帳號或裝置，您應該會關閉 Windows Server Essentials 的功能，並使用 Windows Server 2012 標準原生工具管理帳號及裝置。  
  
> [!IMPORTANT]
>   Windows Server 2012 標準每個使用者或您的環境中的裝置需要 Client 存取權的授權 (CAL)。 這是與 Windows Server Essentials，這並不會使用 CAL 型號和並未隨附任何 Cal 不同。  從 Windows Server Essentials 轉換到 Windows Server 2012 標準，當您將需要購買適當的數字和您的環境（針對大部分購買使用者 Cal）Cal 類型。  
  
## <a name="before-the-transition"></a>之前轉換  
  
-   之前的 Windows Server Essentials 轉換到 Windows Server 2012 標準，您應該完整備份 server 的資料。  
  
    > [!IMPORTANT]
    >  Server 的完整備份，而您無法還原伺服器它已在之前轉換的狀態。  
  
-   此外，請確定您讀取並了解終端使用者授權合約 (EULA)，適用於 Windows Server 2012 標準。 若要檢視使用者授權合約：  
  
    1.  以系統管理員身分開放在命令視窗。  
  
    2.  執行下列命令：  
  
         **Dism /online /set-edition: ServerStandard /geteula：路徑使用者授權合約**  
  
         其中**路徑使用者授權合約**代表您要儲存的使用者授權合約檔案的位置。 例如;C:\ws8std_eula.rtf。  請務必.rtf 當做副檔名。  
  
    3.  打開儲存檔案的位置，然後按兩下 [打開該檔案。  
  
## <a name="transition-to--windows-server-2012-standard"></a>Windows Server 2012 標準轉換  
 之後，您有要轉換的 Windows Server Essentials 到 Windows Server 2012 標準，完成這兩個步驟：  
  
1.  Windows Server 2012 標準與您的環境使用者和/或裝置 Client 存取授權適當數量購買授權。  
  
     您也可以購買授權的 Windows Server 2012 標準零售插座，代理商，或是的協助下[Microsoft 合作夥伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
    > [!NOTE]
    >  如果您購買最初的 Windows Server 2012 標準並執行安裝您有兩個 virtual 執行個體的 Windows Server Essentials 為您降級權限，您不需要任何其他購買。  
    >   
    >  如果您購買 Windows Server 2012 標準透過大量授權頻道，您可以針對 Windows Server 2012 標準從大量授權服務中心 (VLSC) 下載 ISO 映像和 product 金鑰。  
    >   
    >  如果您購買的所有其他頻道 Windows Server 2012 標準可以下載 ISO 映像和評估 product 鍵從 Windows Server Essentials 的[TechNet 評估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 下一個步驟中所述執行轉換會將評估 product 轉換 product 完全授權與支援。  
  
2.  打開以系統管理員的身分，Windows PowerShell，然後執行下列命令。  
  
     **dism /online /set-edition: ServerStandard /accepteula /productkey:***Product 鍵*  
  
     其中*Product 鍵*是您的 Windows Server 2012 標準複本 product 鍵。  
  
     伺服器重新開機才能完成轉換程序。  
  
 轉換之後, 維持在伺服器上 Windows Server Essentials 的功能，並操之在 75 使用者和 75 裝置的支援。 當您超出以上任一的這些限制，您應該使用 Windows Server 2012 標準原生工具來管理帳號及裝置。  
  
 此外，您轉換到 Windows Server 2012 標準之後，Windows Server Essentials 的媒體功能也不會再使用。 這包括遠端 Web 存取和媒體設定儀表板上的媒體功能。  
  
## <a name="turn-off--windows-server-essentials-features"></a>關閉 Windows Server Essentials 的功能  
 如果您不再需要的 Windows Server Essentials 儀表板或其他 value-add 功能管理的伺服器，您可以關閉功能，並移除您的伺服器。  
  
 **關閉 Windows Server Essentials 功能精靈**可協助您在解除安裝的功能。 它也會清除之檔案的 Windows Server Essentials 伺服器軟體所建立的伺服器。  某些清潔作業時，其他則車載機起始伺服器重新開機之後，執行。  
  
 **關閉 Windows Server Essentials 功能精靈**需要您手動解除安裝所有的增益集之前，您可以完成精靈。 若要檢視安裝的增益集的清單，開放儀表板中的應用程式] 頁面。 精靈會警告您，如果它偵測安裝增益集，並提示您將它們解除安裝。  
  
 **關閉 Windows Server Essentials 功能精靈**可讓您選擇是否要保留備份檔案 client 電腦後的 Windows Server Essentials 功能關閉。  
  
 有兩種方式可以執行**關閉 Windows Server Essentials 功能精靈**的儀表板：  
  
#### <a name="from-the-alert"></a>從提醒  
  
1.  從儀表板，開放警示檢視器。  
  
2.  在 [組織] 清單中，選取 [關閉轉換後的 Windows Server Essentials 功能的相關資訊的報告的警示。  
  
3.  在提醒中，按一下 [**關閉 Windows Server Essentials 功能**。  
  
#### <a name="from-the-get-help-and-support-pane"></a>從 [取得協助，並支援窗格  
  
1.  在首頁上，按一下 [取得協助和支援。  
  
2.  按一下**關閉 Windows Server Essentials 功能精靈**。  
  
 可能的一些工作來執行**關閉 Windows Server Essentials 功能精靈**將無法完成。 有時候，這可以執行防止儀表板。 發生這種情形，如果您可執行檔手動開始精靈：  
  
 **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>也了  
  

-   [Windows Server 2012 R2 標準轉換](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [伺服器資料移轉到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Windows Server 2012 R2 標準轉換](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [伺服器資料移轉到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

