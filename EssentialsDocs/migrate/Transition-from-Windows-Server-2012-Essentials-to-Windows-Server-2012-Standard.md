---
title: 從 Windows Server Essentials 轉換到 Windows Server 2012 Standard
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: aace7849244bb65ec0042971e6ec899f554a62d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852301"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>從 Windows Server Essentials 轉換到 Windows Server 2012 Standard

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 Windows Server&reg; 2012 Essentials 最多可支援25位使用者和50裝置。 當您的業務需求超過限制時，您可以執行從 Windows Server Essentials 到 Windows Server 2012 Standard 的就地授權轉換，以維持與授權相容。  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>這個轉換對使用者和裝置限制有什麼影響  
 轉換到 Windows Server 2012 Standard 之後，會移除使用者帳戶和裝置限制，但 Windows Server Essentials 特有的功能（例如儀表板、遠端 Web 存取和用戶端電腦備份）仍會繼續使用。 不過，這些功能的技術限制最多支援 75 個使用者帳戶和 75 個裝置。 如果需要新增75個以上的使用者帳戶或裝置，您應該關閉 Windows Server Essentials 功能，並使用 Windows Server 2012 標準原生工具來管理使用者帳戶和裝置。  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard 需要您環境中的每個使用者或裝置都具備用戶端存取許可證（CAL）。 這不同于 Windows Server Essentials，這不會使用 CAL 模型，也不會隨附任何 Cal。  從 Windows Server Essentials 轉換到 Windows Server 2012 Standard 時，您必須為您的環境購買適當數量和類型的 Cal （大部分客戶都是購買使用者 Cal）。  
  
## <a name="before-the-transition"></a>轉換之前  
  
-   從 Windows Server Essentials 轉換到 Windows Server 2012 Standard 之前，您應該先完整備份伺服器資料。  
  
    > [!IMPORTANT]
    >  若未完整備份伺服器，就無法將伺服器還原成轉換之前的狀態。  
  
-   此外，請確定您已閱讀並瞭解 Windows Server 2012 Standard 的使用者授權合約（EULA）。 若要檢視 EULA：  
  
    1.  以系統管理員身分開啟命令視窗。  
  
    2.  執行下列命令：  
  
         **dism/online/set-edition： ServerStandard/geteula： eula 路徑**  
  
         其中 **eula 路徑**代表您要儲存 EULA 檔案的位置。 例如，C:\ws8std_eula.rtf。  務必使用 .rtf 做為副檔名。  
  
    3.  開啟您儲存檔案的位置，然後按兩下檔案將它開啟。  
  
## <a name="transition-to--windows-server-2012-standard"></a>轉換至 Windows Server 2012 Standard  
 在決定從 Windows Server Essentials 轉換到 Windows Server 2012 Standard 之後，請完成下列兩個步驟：  
  
1. 購買適用于 Windows Server 2012 Standard 的授權，以及適合您環境的使用者和/或裝置用戶端存取使用權數目。  
  
    您可以向零售商、轉銷商或[Microsoft 合作夥伴](https://pinpoint.microsoft.com/SelectCulture.aspx)的協助，購買 Windows Server 2012 Standard 的授權。  
  
   > [!NOTE]
   >  如果您一開始就購買 Windows Server 2012 Standard 並執行降級許可權，將兩個虛擬實例的其中一個安裝為 Windows Server Essentials，則不需要購買任何額外的專案。  
   >   
   >  如果您透過大量授權通路購買 Windows Server 2012 Standard，您可以從大量授權服務中心（VLSC）下載適用于 Windows Server 2012 Standard 的 ISO 映像和產品金鑰。  
   >   
   >  如果您從所有其他頻道購買 Windows Server 2012 Standard，可以從[TechNet 評估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)下載適用于 Windows server ESSENTIALS 的 ISO 映像和評估產品金鑰。 執行下個步驟所述的轉換會將評估產品轉換成完整授權和支援的產品。  
  
2. 以系統管理員身分開啟 Windows PowerShell，然後執行下列命令。  
  
    **dism/online/set-edition： ServerStandard/accepteula/productkey：** *產品金鑰*  
  
    其中*產品金鑰*是您的 Windows Server 2012 Standard 複本的產品金鑰。  
  
    伺服器會重新啟動以完成轉換程序。  
  
   轉換之後，Windows Server Essentials 功能會保留在伺服器上，且最多可支援75使用者和75裝置。 如果您超過其中一項限制，就應該使用 Windows Server 2012 標準原生工具來管理使用者帳戶和裝置。  
  
   此外，轉換到 Windows Server 2012 Standard 之後，就無法再使用 Windows Server Essentials 的媒體功能。 其中包括遠端 Web 存取的媒體功能和儀表板的媒體設定。  
  
## <a name="turn-off--windows-server-essentials-features"></a>關閉 Windows Server Essentials 功能  
 如果您不再需要 Windows Server Essentials 儀表板或其他附加功能來管理伺服器，您可以關閉這些功能，並將它們從伺服器中移除。  
  
 [**關閉 Windows Server Essentials 功能嚮導]** 可協助您卸載功能。 它也會清除 Windows Server Essentials 伺服器軟體所建立的檔案伺服器。  有些清除作業會立即執行，而其他則會在伺服器重新啟動後才進行。  
  
 [關閉**Windows Server Essentials 功能嚮導]** 需要您先手動卸載所有增益集，才能完成嚮導。 若要檢視已安裝的增益集清單，請開啟儀表板中的應用程式頁面。 如果精靈偵測到已安裝的增益集，會發出警告，並提示您將它們解除安裝。  
  
 [**關閉 Windows Server Essentials 功能] Wizard**可讓您選擇在關閉 Windows server essentials 功能之後，是否要保留用戶端電腦的備份檔案。  
  
 有兩種方式可從儀表板執行 [**關閉 Windows Server Essentials 功能] 嚮導**：  
  
#### <a name="from-the-alert"></a>從警示  
  
1.  從 [儀表板] 開啟 [警示檢視器]。  
  
2.  在 [組織清單] 中，選取警示，報告在轉換之後關閉 Windows Server Essentials 功能的相關資訊。  
  
3.  在警示中，按一下 [**關閉 Windows Server Essentials 功能**]。  
  
#### <a name="from-the-get-help-and-support-pane"></a>從 [取得說明與支援] 窗格  
  
1. 在 [首頁] 頁面上，按一下 [取得說明與支援]。  
  
2. 按一下 [**關閉 Windows Server Essentials 功能嚮導]** 。  
  
   [**關閉 Windows Server Essentials 功能嚮導]** 執行的某些工作可能會無法順利完成。 在某些情況下，可能會使儀表板無法執行。 若發生這種情況，您可以執行以下檔案，手動啟動精靈：  
  
   **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>另請參閱  
  

-   [轉換至 Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [移轉伺服器資料到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [轉換至 Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [移轉伺服器資料到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

