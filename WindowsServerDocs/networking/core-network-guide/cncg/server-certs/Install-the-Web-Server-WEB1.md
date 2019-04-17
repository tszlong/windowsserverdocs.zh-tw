---
title: 安裝網頁伺服器 WEB1
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e818eac49719b394a2c73cc125a2e7ba9ea80c82
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-web-server-web1"></a>安裝網頁伺服器 WEB1

>適用於：Windows Server（以每年次管道）、Windows Server 2016

在 Windows Server 2016 網頁伺服器 (IIS) 角色提供安全，輕鬆管理、 模組且最具擴充性的平台會可靠地裝載的網站、 服務和應用程式。 使用 IIS，您可以分享網際網路、 內部網路，或外部網路使用者的資訊。 IIS 是整合 IIS、 ASP.NET、 FTP 服務、 PHP 及 Windows 通訊基本知識 (WCF) 的整合的 web 平台。  

當您部署伺服器的憑證時，您的網頁伺服器為您提供的位置，您可以針對您憑證授權單位發行憑證撤銷清單 (CRL)。 在發行後, CRL 是存取網路上的所有電腦，讓他們可以使用此清單驗證程序期間驗證憑證的其他電腦，不撤銷。   

如果憑證為撤銷 CRL 上，驗證努力會失敗，且您的電腦不受信任實體的不是有效的憑證。  

您安裝的網頁伺服器 (IIS) 角色之前，請確定您有伺服器名稱與 IP 位址設定，並已經加入網域的電腦。  

## <a name="to-install-the-web-server-iis-server-role"></a>若要安裝的網頁伺服器 (IIS) 伺服器角色  
若要完成此程序，您必須成員的**系統管理員**群組。  

>[!NOTE]  
>使用 Windows PowerShell 來執行這個程序，開放 PowerShell、 輸入下列命令，然後再按 ENTER。  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。 新增角色與功能精靈開啟。  
2.  在**在您開始之前，請先**，按一下 [**下**。  

**注意**   
**在您開始之前，請先**頁面上新增角色與精靈中的功能不顯示如果先前已執行新增角色與功能精靈，並選取**預設略過此頁面**在該時間。  

3.  在**安裝類型**頁面上，按**下**。  
4.  在**選擇伺服器**頁面上，按一下 [**下**。  
5.  在**伺服器角色**頁面上，選取**網頁伺服器 (IIS)**，然後按一下 [**下一步**。  
6.  按一下**下一步**直到您接受預設的所有網頁伺服器設定，然後按一下 [**安裝**。  
7.  安裝所有已成功，請確認，然後按一下**關閉**。
