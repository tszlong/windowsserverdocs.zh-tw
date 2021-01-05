---
title: 從 Windows Server 2012 Essentials 轉換到 Windows Server 2012 R2 Standard
description: 瞭解如何從 Windows Server Essentials 轉換為 Windows Server 2012 R2 Standard。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 4838101de3ed9daa1150e208d7aa0a938c95041e
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810345"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>從 Windows Server 2012 Essentials 轉換到 Windows Server 2012 R2 Standard

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

Windows Server 2016 是雲端就緒的作業系統，可支援您目前的工作負載，同時引進可讓您輕鬆轉換至雲端運算的新技術。 Windows Server 2016 內容可協助您做好準備。

 Windows Server Essentials 最多可支援25位使用者和50裝置。 當您的業務需求超過限制時，您可以執行從 Windows Server Essentials 到 Windows Server 2012 R2 Standard 的就地授權轉換，以維持授權相容性。

 轉換至 Windows Server 2012 R2 Standard 之後，會移除使用者帳戶和裝置限制，但 Windows Server Essentials 的獨特功能 (例如儀表板、遠端 Web 存取和用戶端電腦備份) 仍保持可用。 不過，這些功能的技術限制最多支援 100 個使用者帳戶和 200 台裝置。 用戶端電腦的備份功能將支援最多 75 裝置的備份。

> [!IMPORTANT]
>   Windows Server 2012 R2 Standard 需要您環境中的每個使用者或裝置都具備用戶端存取使用權 (CAL) 。 這與 Windows Server Essentials 不同，它不會使用 CAL 模型，也不會隨附任何 Cal。 從 Windows Server Essentials 轉換到 Windows Server 2012 R2 Standard 時，您必須為您的環境購買適當數量和類型的 Cal， (大部分的客戶會) 購買使用者 Cal。

## <a name="before-the-transition"></a>轉換之前

-   從 Windows Server Essentials 轉換到 Windows Server 2012 R2 Standard 之前，您應該先完整備份伺服器資料。

    > [!IMPORTANT]
    >  若未完整備份伺服器，就無法將伺服器還原成轉換之前的狀態。

-   此外，請確定您已閱讀並瞭解 Windows Server 2012 R2 Standard 的使用者授權合約 (EULA) 。 若要檢視 EULA：

    1.  以系統管理員身分開啟命令視窗。

    2.  執行以下命令：

        ```console
        dism /online /set-edition:ServerStandard /geteula: <eula path>
        ```

        其中 *eula 路徑* 代表您要儲存 eula 檔案的位置;例如： C：\ ws8std_eula .rtf) 。 務必使用 .rtf 做為副檔名。

    3.  開啟您儲存檔案的位置，然後按兩下檔案將它開啟。

## <a name="transition-to--windows-server-2012-r2-standard"></a>轉換至 Windows Server 2012 R2 Standard
 決定從 Windows Server Essentials 轉換到 Windows Server 2012 R2 Standard 之後，請完成下列兩個步驟：

1. 購買適用于 Windows Server 2012 R2 Standard 的授權，以及適合您環境的使用者及/或裝置用戶端存取授權數目。

    您可以從零售商店、轉銷商或 [Microsoft 合作夥伴](https://pinpoint.microsoft.com/SelectCulture.aspx)的協助，購買 Windows Server 2012 R2 Standard 的授權。

   > [!NOTE]
   >  如果您一開始就購買了 Windows Server 2012 R2 Standard 並執行降級許可權，以將兩個虛擬實例的其中一個安裝為 Windows Server Essentials，您就不需要購買任何額外的產品。
   >
   >  如果您是透過大量授權管道購買 Windows Server 2012 R2 Standard，則可以從大量授權服務中心 (VLSC) 下載 ISO 映像與 Windows Server 2012 R2 Standard 的產品金鑰。
   >
   >  如果您從任何其他頻道購買 Windows Server 2012 R2 Standard，您可以從 [TechNet 評估中心](https://technet.microsoft.com/evalcenter/jj659306.aspx)下載 Windows server ESSENTIALS 的 ISO 映像和評估產品金鑰。 執行下個步驟所述的轉換會將評估產品轉換成完整授權和支援的產品。

2. 以系統管理員身分開啟 Windows PowerShell，然後執行下列命令：

    ```console
    dism /online /set-edition:ServerStandard /accepteula /productkey: <Product Key>
    ```

    其中 *產品金鑰* 是您的 Windows Server 2012 R2 Standard) 複本的產品金鑰。

    伺服器會重新啟動以完成轉換程序。

   轉換之後，Windows Server Essentials 功能會保留在伺服器上，且最多可支援100使用者和200裝置。

## <a name="additional-references"></a>其他參考資料


-   [移轉伺服器資料到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

