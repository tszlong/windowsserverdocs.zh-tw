---
title: 安裝網路原則伺服器
description: 您可以使用本主題，以使用 Windows PowerShell 或 Windows Server 2016 中的 [新增角色及功能] Wizard，來安裝 (NPS) 的網路原則伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: d982ddfe05ec9df3429de4997aaddc4d0b850072
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948294"
---
# <a name="install-network-policy-server"></a>安裝網路原則伺服器

您可以使用本主題，利用 Windows PowerShell 或 [新增角色及功能] 嚮導，將網路原則伺服器安裝 (NPS) 。 NPS 是網路原則與存取服務伺服器角色的角色服務。

> [!NOTE]
> 根據預設值，NPS 會為所有已安裝的網路介面卡接聽連接埠 1812、1813、1645 和 1646 上的 RADIUS 流量。 如果您在安裝 NPS 時啟用 [具有 Advanced Security 的 Windows 防火牆]，則會在安裝程式期間，針對網際網路通訊協定第6版 \( IPv6 \) 和 IPv4 流量自動建立這些埠的防火牆例外。 如果您的網路存取伺服器設定為透過非預設的埠來傳送 RADIUS 流量，請移除 NPS 安裝期間在 [Windows 防火牆] 中建立的例外狀況，並為您要用於 RADIUS 流量的埠建立例外。

**系統管理認證**

您必須是 **Domain Admins** 群組的成員，才可以完成這個程序。

## <a name="to-install-nps-by-using-windows-powershell"></a>使用 Windows PowerShell 安裝 NPS

若要使用 Windows PowerShell 來執行此程式，請以系統管理員身分執行 Windows PowerShell，輸入下列命令，然後按 ENTER。

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>使用伺服器管理員安裝 NPS

1.  在 NPS1 的 [伺服器管理員] 中，按一下 [管理]，然後按一下 [新增角色及功能]。 [新增角色及功能精靈] 隨即開啟。

2.  在 [在您開始前] 中，按 [下一步]。

    > [!NOTE]
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]，就不會顯示 [新增角色及功能精靈] 的 [在您開始前] 頁面。

3.  在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。

4.  在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 [下一步] 。

5.  在 [ **選取伺服器角色**] 的 [ **角色**] 中，選取 [ **網路原則與存取服務**]。 對話方塊隨即開啟，詢問是否應新增網路原則和存取服務所需的功能。 按一下 [ **新增功能**]，然後按 **[下一步]**

6.  在 [選取功能] 中，按 [下一步]，然後在 [網路原則與存取服務]中，檢閱提供的資訊，接著按 [下一步]。

7.  在 [選取角色服務] 中，按一下 [網路原則伺服器]。  在 [新增網路原則伺服器所需的功能] 中，按一下 [新增功能]。 按一下 [下一步] 。

8.  在 [確認安裝選項] 中，按一下 [必要時自動重新啟動目的地伺服器]。 提示您確認這個選取項目時，按一下 [是]，然後按一下 [安裝]。 [安裝進度] 頁面會在安裝程序期間顯示狀態。 當程式完成時，會顯示「 *computername* 上安裝成功」的訊息，其中 *ComputerName* 是您安裝網路原則伺服器的電腦名稱稱。 按一下 [關閉]  。

如需詳細資訊，請參閱 [管理 NPSs](nps-manage-servers.md)。