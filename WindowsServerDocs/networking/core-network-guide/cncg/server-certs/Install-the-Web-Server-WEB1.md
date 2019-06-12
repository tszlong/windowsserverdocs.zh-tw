---
title: 安裝網頁伺服器 WEB1
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15da16094a47a2492dc9054e0671c3709fe23362
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446451"
---
# <a name="install-the-web-server-web1"></a>安裝網頁伺服器 WEB1

>適用於：Windows Server （半年通道），Windows Server 2016

Windows Server 2016 中的網頁伺服器 (IIS) 角色提供安全、 容易管理、 模組化且可擴充的平台能夠可靠地主控網站、 服務和應用程式。 使用 IIS 時，您可以共用網際網路、 內部或外部網路與使用者資訊。 IIS 是一個整合 IIS、 ASP.NET、 FTP 服務、 PHP 和 Windows Communication Foundation (WCF) 的統合式的 web 平台。  

當您部署伺服器憑證時，您的 Web 伺服器為您提供您可以在其中發佈您的憑證授權單位 (CA) 憑證撤銷清單 (CRL) 的位置。 發行集，CRL 之後存取網路上的所有電腦，使其可以驗證程序期間使用這份清單，以確認其他電腦所出示的憑證不會撤銷。   

如果憑證位於 CRL 為已撤銷，驗證工作失敗，您的電腦不受信任實體已不再有效的憑證。  

您安裝網頁伺服器 (IIS) 角色之前，請確定您的伺服器名稱和 IP 位址設定，並已加入網域的電腦。  

## <a name="to-install-the-web-server-iis-server-role"></a>安裝網頁伺服器 (IIS) 伺服器角色  
若要完成此程序，您必須是 **Administrators** 群組的成員。  

>[!NOTE]  
>若要使用 Windows PowerShell 執行此程序，請開啟 PowerShell，輸入下列命令，，然後按 ENTER。  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。  
2.  在 [在您開始前]  中，按 [下一步]  。  

**附註**   
**在您開始前**如果您先前已執行 [新增角色及功能精靈]，而且您選取未顯示的 [新增角色及功能精靈] 的頁面**略過此頁面預設**在該時間。  

3. 在 [安裝類型]  頁面上，按 [下一步]  。  
4. 在 [**伺服器選取項目**頁面上，按一下**下一步]** 。  
5. 在 [**伺服器角色**頁面上，選取**網頁伺服器 (IIS)** ，然後按一下**下一步]** 。  
6. 在您接受所有的預設網頁伺服器設定之後，按 [下一步]  ，然後按一下 [安裝]  。  
7. 確認所有安裝成功，然後按一下 [關閉]  。
