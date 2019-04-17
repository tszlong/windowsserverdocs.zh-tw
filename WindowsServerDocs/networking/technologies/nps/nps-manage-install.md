---
title: 安裝網路原則伺服器
description: 您可以使用此主題安裝使用「Windows PowerShell 或新增角色功能精靈中與 Windows Server 2016 的網路原則 Server (NPS)
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b76654fa01b68b5a8c9ea1efc80dfbc47e6a62f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-network-policy-server"></a>安裝網路原則伺服器

若要安裝的網路原則 Server (NPS) 來使用 Windows PowerShell 或新增角色和精靈中的功能，您可以使用此主題。 NPS 是以角色服務的網路原則與服務存取伺服器角色。

> [!NOTE]
> 根據預設，NPS RADIUS 連接埠 1812 年，1813 年、1645 年 1646 年所有安裝的網路介面卡上的資料傳輸接聽。 使用進階安全性 Windows 防火牆尚未安裝 NPS 時，如果上述連接埠防火牆例外會自動建立網際網路通訊協定第 6 版 \(IPv6\) 和 IPv4 流量的安裝程序期間。 如果您的網路存取伺服器設定 RADIUS 流量傳送到以外這些預設的連接埠，移除例外建立 NPS 在安裝期間，Windows 防火牆使用進階安全性，並建立例外 RADIUS 流量您使用的連接埠。

**管理認證**

若要完成此程序，您必須成員的**網域系統管理員**群組。

## <a name="to-install-nps-by-using-windows-powershell"></a>若要使用 Windows PowerShell 安裝 NPS

若要使用 Windows PowerShell，系統管理員身分執行 Windows PowerShell 來執行這個程序輸入下列命令，，然後按 ENTER 鍵。

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>若要使用伺服器管理員安裝 NPS

1.  NPS1，在伺服器管理員中，按一下 [**管理**，然後按**新增角色與功能**。 新增角色與功能精靈開啟。

2.  在**在您開始之前，請先**，按一下 [**下**。

    > [!NOTE]
    > **在您開始之前，請先**頁面上新增角色與精靈中的功能不顯示如果先前已選取**預設略過此頁面**功能精靈與新增的角色執行。

3.  在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。

4.  在**選取目的伺服器**，確保**選取伺服器伺服器集區的**選取。 在**伺服器集區**，請確定已選取 [本機電腦。 按一下**下一步**。

5.  在**選取伺服器角色**，請在**角色**、選取**網路原則與服務存取**。 對話方塊詢問是否它應該會新增所需的網路原則與服務存取的功能。 按一下**新增功能**，然後按**下一步**

6.  在**選擇功能**，按一下 [**下一步**，在**網路原則與服務存取**，檢視的資訊，提供，然後按一下**下一步**。

7.  在**選擇角色服務**，按一下 [**的網路原則伺服器**。  在**新增所需的網路原則伺服器功能**，按一下 [**新增功能**。 按一下**下一步**。

8.  在**確認安裝選項**，按一下 [**必要時自動重新開機目的伺服器**。 當系統提示您確認選擇時，請按一下**[是]**，然後按一下 [**安裝**。 安裝進度頁面會顯示在安裝期間狀態。 此程序完成時，該訊息」成功安裝*電腦名稱*「出現時，其中*電腦名稱*電腦時，您的網路原則伺服器的名稱。 按一下**關閉**。

如需詳細資訊，請查看[管理 NPS 伺服器]](nps-manage-servers.md)。