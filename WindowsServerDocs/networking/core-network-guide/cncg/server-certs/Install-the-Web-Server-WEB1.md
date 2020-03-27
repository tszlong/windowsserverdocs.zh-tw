---
title: 安裝網頁伺服器 WEB1
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8871b2b82b30bca5b4efd62a31b52e8dbd7284a9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318288"
---
# <a name="install-the-web-server-web1"></a>安裝網頁伺服器 WEB1

>適用於：Windows Server (半年通道)、Windows Server 2016

Windows Server 2016 中的網頁伺服器（IIS）角色提供安全、容易管理、模組化且可擴充的平臺，讓您能夠可靠地裝載網站、服務和應用程式。 使用 IIS 時，您可以與網際網路、內部網路或外部網路上的使用者共用資訊。 IIS 是整合 IIS、ASP.NET、FTP 服務、PHP 和 Windows Communication Foundation （WCF）的統一 web 平臺。  

當您部署伺服器憑證時，您的網頁伺服器會提供一個位置，讓您可以為憑證授權單位單位（CA）發佈憑證撤銷清單（CRL）。 發行後，您網路上的所有電腦都可存取 CRL，以便在驗證程式期間使用此清單，以確認其他電腦所提供的憑證不會被撤銷。   

如果憑證在 CRL 上被撤銷，則驗證工作會失敗，且您的電腦會受到保護，使其無法信任憑證不再有效的實體。  

安裝網頁伺服器（IIS）角色之前，請確定您已設定伺服器名稱和 IP 位址，並已將電腦加入網域。  

## <a name="to-install-the-web-server-iis-server-role"></a>若要安裝網頁伺服器（IIS）伺服器角色  
若要完成此程序，您必須是 **Administrators** 群組的成員。  

>[!NOTE]  
>若要使用 Windows PowerShell 執行此程式，請開啟 PowerShell，輸入下列命令，然後按 ENTER。  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。  
2.  在 [在您開始前] 中，按 [下一步]。  

**注意**   
如果您先前已執行 [新增角色及功能] 嚮導，而且在當時選取 [**略過此頁面**]，則不會顯示 [新增角色及功能] 的 [**開始之前**] 頁面。  

3. 在 [安裝類型] 頁面上，按 [下一步]。  
4. 在 [**伺服器選取專案**] 頁面上，按 **[下一步]** 。  
5. 在 [**伺服器角色**] 頁面上，選取 [**網頁伺服器（IIS）** ]，然後按 **[下一步]** 。  
6. 在您接受所有的預設網頁伺服器設定之後，按 [下一步]，然後按一下 [安裝]。  
7. 確認所有安裝成功，然後按一下 [關閉]。
