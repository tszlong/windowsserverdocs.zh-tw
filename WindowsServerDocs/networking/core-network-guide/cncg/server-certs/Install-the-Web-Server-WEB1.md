---
title: 安裝網頁伺服器 WEB1
description: 本主題是適用于 802.1 X 有線和無線部署的指南部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c3d71c1e0108c9036f3dba7e3508d774ab6677ee
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947834"
---
# <a name="install-the-web-server-web1"></a>安裝網頁伺服器 WEB1

>適用於：Windows Server (半年度管道)、Windows Server 2016

Windows Server 2016 中的 Web 服務器 (IIS) 角色，提供安全、容易管理、模組化且可擴充的平臺，可哥靠地裝載網站、服務和應用程式。 您可以使用 IIS，與網際網路、內部網路或外部網路上的使用者共用資訊。 IIS 是整合的 web 平臺，可將 IIS、ASP.NET、FTP 服務、PHP 和 Windows Communication Foundation (WCF) 。

當您部署伺服器憑證時，您的網頁伺服器會提供一個位置，讓您可以在證書 (頒發機構單位)  (CRL) 發佈憑證撤銷清單。 發行之後，您網路上的所有電腦都可以存取 CRL，讓他們可以在驗證程式期間使用此清單，以確認未撤銷其他電腦所顯示的憑證。

如果憑證在 CRL 上被撤銷，驗證工作會失敗，而您的電腦會受到保護，使其無法信任擁有不再有效之憑證的實體。

在安裝 Web 服務器 (IIS) 角色之前，請確定您已設定伺服器名稱和 IP 位址，並將電腦加入網域。

## <a name="to-install-the-web-server-iis-server-role"></a>若要安裝 Web 服務器 (IIS) 伺服器角色
若要完成此程序，您必須是 **Administrators** 群組的成員。

>[!NOTE]
>若要使用 Windows PowerShell 來執行此程式，請開啟 PowerShell 並輸入下列命令，然後按 ENTER。
`Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。
2.  在 [在您開始前] 中，按 [下一步]。

**注意** 如果您先前已執行 [新增角色及功能] 嚮導，並且選取 [在該時間 **預設略過此頁面**]，則不會顯示 [新增角色及功能] 嚮導的 [**開始之前**] 頁面。

3. 在 [安裝類型] 頁面上，按 [下一步]。
4. 在 [ **伺服器選取** ] 頁面上，按 **[下一步]**。
5. 在 [ **伺服器角色** ] 頁面上，選取 [ **WEB 伺服器 (IIS)**，然後按 **[下一步]**。
6. 在您接受所有的預設網頁伺服器設定之後，按 [下一步]，然後按一下 [安裝]。
7. 確認所有安裝成功，然後按一下 [關閉]。
