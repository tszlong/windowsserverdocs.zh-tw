---
title: "疑難排解 Windows Server Essentials 中的遠端 Web 存取連接"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: af4725fd3b1861c847434e3ed62c3da030689fb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>疑難排解 Windows Server Essentials 中的遠端 Web 存取連接
 
>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
  
 一般而言，Windows Server Essentials 可以自動設定寬頻路由器路由器是否 UPnP 認證的裝置」和 UPnP 設定如果尚未在路由器上。  
  
## <a name="possible-issues"></a>可能的問題  
 使用遠端 Web 存取連接可能遭遇下列問題：  
  
-   您的路由器已不或不連接到您的網路。  
  
-   您的路由器 UPnP 設定已關閉。  
  
-   您的路由器可能不完整支援 UPnP 標準。 Microsoft 會維持路由器可搭配 Windows 作業系統的清單。 若要檢視路由器（包括 wireless 路由器）與 Windows Server Essentials 相容的清單，請造訪[Windows 相容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。  
  
## <a name="possible-fixes"></a>可能的修正方法  
 下列動作可能可以修正這些問題：  
  
-   確認您的路由器，您電腦，正常運作。  
  
-   請確定您的伺服器會直接連接到您的路由器或，它已連接到連接到您的路由器切換。  
  
-   確認寬頻裝置連接到網際網路服務提供者 (ISP) 已，正常運作，以及寬頻的裝置已連接您的路由器。  
  
-   關閉 UPnP 路由器設定。 若要將 UPnP 設定路由器設定網頁連接。 如何登入您的路由器，以及如何關閉 UPnP 設定的相關資訊，會看到您的路由器的文件。 您將 UPnP 設定之後，執行關閉上遠端 Web 存取精靈一次，設定您的路由器。  
  
-   如果您的路由器完全不支援的 UPnP 標準，它不會自動設定。 您必須手動設定您的路由器，或購買路由器支援 UPnP 標準。  
  
     若要手動設定您的路由器，請完成下列工作：  
  
    -   建立您的 Windows Server Essentials 伺服器的 IP 位址保留。  
  
         您手動設定 Windows Server essentials 轉寄需要連接埠路由器之前，您必須設定為您的路由器上執行 Windows Server Essentials 伺服器動態主機設定通訊協定」(DHCP) 保留的項目。 這個步驟可確保您將轉送的連接埠不會變更的 IP 位址。  
  
         了解如何手動設定 DHCP 保留您的伺服器，您的路由器上的資訊，會看到您的路由器製造商 s 文件。  
  
    -   設定為下列連接埠路由器上的連接埠轉接：  
  
        |服務或通訊協定|連接埠|  
        |-------------------------|----------|  
        |HTTP|TCP 80|  
        |HTTPS|TCP 443|  
  
     如何連接埠轉送路由器上手動設定的相關資訊，會看到製造商 s 文件。  
  
     一般路由器設定網頁包含表，如下所示。  
  
    > [!NOTE]
    >  此表格的電腦執行 Windows Server Essentials 的 IP 位址會 192.168.0.100。 您必須判斷您電腦的 IP 位址和替代表所示的 IP 位址的 IP 位址。  
  
    |IP 位址|通訊協定 (TCP 日 UDP)|排程|輸入篩選器|  
    |----------------|---------------------------|--------------|--------------------|  
    |192.168.0.100|TCP 80|隨時|允許所有|  
    |192.168.0.100|TCP 443|隨時|允許所有|  
  
     您手動設定您的路由器之後，執行關閉上遠端 Web 存取精靈，確保您選取 [**略過路由器設定**選項，在**開始**頁面。  
  
-   購買新的路由器，如果您的路由器完全不支援的 UPnP 標準。  
  
> [!TIP]
>  請確定您的路由器已安裝最新的 BIOS 韌體。 您通常可以 BIOS 韌體更新適用於您的路由器從路由器設定網頁。 如需詳細資訊，查看您路由器的文件。 您的路由器會在更新之後，請執行設定任何地方存取精靈。  
  
## <a name="see-also"></a>也了  
  
-   [使用遠端存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理網路遠端存取](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理隨時隨地存取](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Windows Server Essentials 的支援](Support-Windows-Server-Essentials.md)

-   [Windows Server Essentials 的支援](../support/Support-Windows-Server-Essentials.md)

