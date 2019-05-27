---
title: 從 Windows Server Essentials 轉換到 Windows Server 2012 Standard
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882499"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>從 Windows Server Essentials 轉換到 Windows Server 2012 Standard

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

 Windows Server® 2012 Essentials 可支援最多 25 位使用者及 50 台裝置。 當您的業務需求超過限制時，您可以從 Windows Server Essentials 中執行的就地授權轉換到 Windows Server 2012 Standard 以維持授權相容性。  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>這個轉換對使用者和裝置限制有什麼影響  
 在轉換到 Windows Server 2012 Standard 後，會移除使用者帳戶和裝置限制，但所特有 （例如儀表板、 遠端 Web 存取和用戶端電腦備份），Windows Server Essentials 的功能仍然可使用。 不過，這些功能的技術限制最多支援 75 個使用者帳戶和 75 個裝置。 如果您需要新增 75 個以上的使用者帳戶或裝置，您應該關閉 Windows Server Essentials 的功能，並使用 Windows Server 2012 Standard 的原生工具來管理使用者帳戶和裝置。  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard 每個使用者或裝置在您的環境中需要用戶端存取使用權 (CAL)。 這是不同於 Windows Server Essentials 中，不使用 CAL 模型，並不隨附任何 Cal。  當從 Windows Server Essentials 轉換到 Windows Server 2012 Standard，您必須購買適當數量和類型的 Cal （大部分客戶是購買使用者 Cal） 環境。  
  
## <a name="before-the-transition"></a>轉換之前  
  
-   再從 Windows Server Essentials 轉換到 Windows Server 2012 Standard，您應該完整備份伺服器資料。  
  
    > [!IMPORTANT]
    >  若未完整備份伺服器，就無法將伺服器還原成轉換之前的狀態。  
  
-   此外，請確認您已閱讀並了解針對 Windows Server 2012 Standard 的終端使用者授權合約 (EULA)。 若要檢視 EULA：  
  
    1.  以系統管理員身分開啟命令視窗。  
  
    2.  執行下列命令：  
  
         **dism /online /set-edition:ServerStandard /geteula: eula path**  
  
         其中 **eula 路徑**代表您要儲存 EULA 檔案的位置。 例如，C:\ws8std_eula.rtf。  務必使用 .rtf 做為副檔名。  
  
    3.  開啟您儲存檔案的位置，然後按兩下檔案將它開啟。  
  
## <a name="transition-to--windows-server-2012-standard"></a>轉換到 Windows Server 2012 Standard  
 之後，您已決定轉換從 Windows Server Essentials 為 Windows Server 2012 Standard，完成這兩個步驟：  
  
1.  Windows Server 2012 Standard 和適當數目的環境的使用者和/或裝置用戶端存取授權購買的授權。  
  
     向零售商、 經銷商或透過的協助，您可以針對 Windows Server 2012 Standard 購買的授權[Microsoft 合作夥伴](https://pinpoint.microsoft.com/SelectCulture.aspx)。  
  
    > [!NOTE]
    >  如果您購買 Windows Server 2012 Standard 一開始，並執行降級權限，為 Windows Server Essentials 安裝其中一種將兩個虛擬執行個體，則您不需要購買任何其他項目。  
    >   
    >  如果大量授權通路購買 Windows Server 2012 Standard，您可以針對 Windows Server 2012 Standard 從大量授權服務中心 (VLSC) 下載的 ISO 映像和產品金鑰。  
    >   
    >  如果您從所有其他通路購買 Windows Server 2012 Standard 可以下載的 ISO 映像和評估產品金鑰適用於從 Windows Server Essentials [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx)。 執行下個步驟所述的轉換會將評估產品轉換成完整授權和支援的產品。  
  
2.  以系統管理員身分開啟 Windows PowerShell，然後執行下列命令。  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *產品金鑰*  
  
     何處*產品金鑰*是您的 Windows Server 2012 Standard 複本的產品金鑰。  
  
     伺服器會重新啟動以完成轉換程序。  
  
 轉換之後，Windows Server Essentials 功能會保留在伺服器上，並支援高達 75 名使用者和 75 個裝置。 如果您超出其中一個限制，您應該使用 Windows Server 2012 Standard 的原生工具管理使用者帳戶和裝置。  
  
 此外，您轉換到 Windows Server 2012 Standard 之後，Windows Server Essentials 的媒體功能就不再可用。 其中包括遠端 Web 存取的媒體功能和儀表板的媒體設定。  
  
## <a name="turn-off--windows-server-essentials-features"></a>關閉 Windows Server Essentials 的功能  
 如果您不再需要管理伺服器的 Windows Server Essentials 儀表板或其他的加值功能，您可以關閉這些功能，並從您的伺服器移除它們。  
  
 **關閉 [Windows Server Essentials 功能精靈]** 可協助您解除安裝功能。 它也會清除的伺服器所建立的 Windows Server Essentials 伺服器軟體的檔案。  有些清除作業會立即執行，而其他則會在伺服器重新啟動後才進行。  
  
 **關閉 [Windows Server Essentials 功能精靈]** 需要您手動解除安裝所有增益集才能完成精靈。 若要檢視已安裝的增益集清單，請開啟儀表板中的應用程式頁面。 如果精靈偵測到已安裝的增益集，會發出警告，並提示您將它們解除安裝。  
  
 **關閉 [Windows Server Essentials 功能精靈]** 可讓您選擇是否要保留備份檔案，用戶端電腦之後關閉 Windows Server Essentials 功能。  
  
 有兩種方式來執行**關閉 [Windows Server Essentials 功能精靈]** 儀表板：  
  
#### <a name="from-the-alert"></a>從警示  
  
1.  從 [儀表板] 開啟 [警示檢視器]。  
  
2.  在 [組合管理] 清單中，選取的警示，會報告在轉換之後關閉 Windows Server Essentials 功能的相關資訊。  
  
3.  在警示中，按一下**關閉 Windows Server Essentials 功能**。  
  
#### <a name="from-the-get-help-and-support-pane"></a>從 [取得說明與支援] 窗格  
  
1.  在 [首頁] 頁面上，按一下 [取得說明與支援]。  
  
2.  按一下 **關閉 Windows Server Essentials 功能精靈**。  
  
 它是所執行的某些工作可能**關閉 [Windows Server Essentials 功能精靈]** 將無法順利完成。 在某些情況下，可能會使儀表板無法執行。 若發生這種情況，您可以執行以下檔案，手動啟動精靈：  
  
 **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>另請參閱  
  

-   [轉換到 Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [將伺服器資料移轉到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [轉換到 Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [將伺服器資料移轉到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

