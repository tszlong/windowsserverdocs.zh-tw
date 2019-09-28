---
title: 搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾概觀
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 40cc953ce7393781497d957fc8e6690c5c9abc0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365919"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>使用 AD FS 和 Web 應用程式 Proxy 部署工作資料夾：總覽

>適用於：Windows Server (半年度管道)、Windows Server 2016

本節中的主題提供使用 Active Directory 同盟服務 (AD FS) 與 Web 應用程式 Proxy 進行工作資料夾部署的指示。 其設計旨在協助您對於已準備透過內部部署或網際網路開始使用工作資料夾的用戶端電腦，建立運作正常的完整工作資料夾設定。  
  
「工作資料夾」是 Windows Server 2012 R2 中所導入的一個元件，允許資訊工作者在他們的裝置間同步工作檔案。 如需有關工作資料夾的詳細資訊，請參閱[工作資料夾概觀](Work-Folders-Overview.md)。  
  
若要讓使用者能夠透過網際網路同步他們的工作資料夾，您需要透過反向 Proxy 發佈工作資料夾，讓工作資料夾可以在網際網路上從外部提供。 內含在 AD FS 中的 Web 應用程式 Proxy 是一個選項，您可用來提供反向 Proxy 功能。 Web 應用程式 Proxy 會使用 AD FS 預先驗證工作資料夾 Web 應用程式的存取，讓使用者可以在任何裝置上從公司網路以外存取工作資料夾。 

> [!NOTE]
>   本節中涵蓋的指示適用於 Windows Server 2016 環境。 如果您使用 Windows Server 2012 R2，請依照 [Windows Server 2012 R2 指示](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。
  
這些主題提供下列資訊：  
  
-   透過 Windows Server 使用者介面搭配 AD FS 與 Web 應用程式 Proxy 設定和部署工作資料夾的逐步指示。 如何以自我簽署憑證設定簡單的測試環境的操作指示。 然後您可以使用測試範例做為指南，協助您建立一個使用公開信任憑證的生產環境。  
  
## <a name="prerequisites"></a>必要條件  
若要遵循這些主題中的程序和範例，您需要備妥下列元件︰  
  
-   Windows Server 2012 R2 中 Active Directory® Domain Services 樹系具有架構延伸，以支援您在使用多部檔案伺服器時，自動將電腦和裝置轉介到正確的檔案伺服器。 最好在樹系中啟用 DNS，不過這不一定必要。  
  
-   網域控制站：已啟用 [AD DS] 角色並已設定網域（適用于測試範例 contoso.com）的伺服器。  
  
    網域控制站需要至少執行 Windows Server 2012 R2，才能支援 Workplace Join 的裝置註冊。 如果您不想使用 Workplace Join，您可以在網域控制站上執行 Windows Server 2012。  
  
-   加入網域 (例如 contoso.com) 的兩個伺服器，並且都執行 Windows Server 2016。 一部伺服器將用於AD FS，另一部則用於工作資料夾。  
  
-   一部沒有加入網域的伺服器，並且執行 Windows Server 2016。 此部伺服器將執行 Web 應用程式 Proxy，而且其網路網域 (例如 contoso.com) 必須有一張網路卡，另一張網路卡則用於外部網路。  
  
-   一部已加入網域的用戶端電腦且執行 Windows 7 或更新版本。  
  
-   一個未加入網域的用戶端電腦且執行 Windows 7 或更新版本。  
  
若是我們在本指南所涵蓋的測試環境，您應該擁有下圖所示的拓撲。 電腦可以是實體電腦或虛擬機器 (VM)。 
  
![圖中顯示網際網路、DMZ 和 Contoso 網路區段。 在 [網際網路] 區段中：Client2在 DMZ 中： WAP 伺服器;在 Contoso 區段中：工作資料夾伺服器、網域控制站、AD FS 伺服器和 Client1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>部署概觀  
在此主題群組中，您將在測試環境中透過範例逐步設定 AD FS、Web 應用程式 Proxy 和工作資料夾。 元件會以此順序設定︰  
  
1.  AD FS  
  
2.  工作資料夾  
  
3.  Web 應用程式 Proxy  
  
4.  加入網域的工作站和未加入網域的工作站  
  
您也將使用 Windows PowerShell 指令碼來建立自我簽署憑證。  
  
## <a name="deployment-steps"></a>部署步驟  
若要使用 Windows Server 使用者介面執行部署，請遵循這些主題中的步驟操作︰  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟1，設定 AD FS @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟2，AD FS 設定後的工作 @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟3，設定工作資料夾 @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟4，設定 Web 應用程式 Proxy @ no__t-0  
  
-   @no__t-使用 AD FS 和 Web 應用程式 Proxy 來0Deploy 工作資料夾：步驟5：設定用戶端 @ no__t-0  

## <a name="see-also"></a>另請參閱  
[工作資料夾總覽](Work-Folders-Overview.md)  
[設計工作資料夾實作](Plan-Work-Folders.md)  
[部署工作資料夾](Deploy-Work-Folders.md)  
  

