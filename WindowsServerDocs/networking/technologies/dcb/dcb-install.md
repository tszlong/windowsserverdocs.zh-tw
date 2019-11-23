---
title: 在 Windows Server 或用戶端中安裝資料中心橋接（DCB）
description: 本主題提供有關如何在 Windows Server 或 Windows 用戶端中安裝資料中心橋接的指示。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ecb6ef072dd2328a0a45d57d181dca9c2928a30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405784"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>在 Windows Server 2016 或 Windows 10 中安裝資料中心橋接 \(DCB\)

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解如何在 Windows Server 2016 或 Windows 10 中安裝 DCB。

## <a name="prerequisites-for-using-dcb"></a>使用 DCB 的必要條件

以下是設定和管理 DCB 的必要條件。

### <a name="install-a-compatible-operating-system"></a>安裝相容的作業系統

您可以在下列作業系統中使用本指南中的 DCB 命令。

- Windows Server (半年度管道)
- Windows Server 2016
- Windows 10 \(所有版本\)

下列作業系統包含舊版 DCB，這些版本與 Windows Server 2016 和 Windows 10 的 DCB 檔中所使用的命令不相容。

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>硬體需求

以下是 DCB 的硬體需求清單。

- DCB\-功能的乙太網路介面卡\(s\) 必須安裝在提供 Windows Server 2016 DCB 的電腦上。
- DCB\-支援的硬體交換器必須部署在您的網路上。


## <a name="install-dcb-in-windows-server-2016"></a>在 Windows Server 2016 中安裝 DCB

您可以使用下列各節，在執行 Windows Server 2016 的電腦上安裝 DCB。

**系統管理認證**

若要執行這些程式，您必須是系統**管理員**的成員。

### <a name="install-dcb-using-windows-powershell"></a>使用 Windows PowerShell 安裝 DCB

您可以使用下列程式，使用 Windows PowerShell 安裝 DCB。

1. 在執行 Windows Server 2016 的電腦上，按一下 [**開始**]，然後以滑鼠右鍵按一下 Windows PowerShell 圖示。 功能表隨即出現。 在功能表中，按一下 [**更多**]，然後按一下 [**以系統管理員身分執行**]。 如果出現提示，請輸入在電腦上具有系統管理員許可權之帳戶的認證。 Windows PowerShell 會以系統管理員許可權開啟。
2. 輸入下列命令，然後按 ENTER。

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>使用伺服器管理員安裝 DCB

您可以使用下列程式，使用伺服器管理員來安裝 DCB。

>[!NOTE]
>執行此程式中的第一個步驟之後，如果您先前在執行 [新增角色及功能] Wizard 時選取 [**略過此頁面**]，則不會顯示 [新增角色及功能] wizard 的 [**開始之前**] 頁面。 如果沒有顯示 [在**您開始前**] 頁面，請略過步驟1到步驟3。

1. 在 DC1 的伺服器管理員中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。
2. 在 [在您開始前] 中，按 [下一步]。
3. 在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。
4. 在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 **\[下一步\]** 。
5. 在 [選取伺服器角色] 中按 [下一步]。
6. 在 [**選取功能**] 的 [**功能**] 中，按一下 [**資料中心橋接**]。 隨即會開啟對話方塊，詢問您是否要加入 DCB 必要的功能。 按一下 [**新增功能**]。
7. 在 [**選取功能**] 中，按 **[下一步]** 。 
8. 7.In**確認安裝選項**，按一下 **安裝**。 [**安裝進度**] 頁面會在安裝過程中顯示狀態。 出現指示安裝成功的訊息之後，請按一下 [**關閉**]。

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>將內核偵錯工具設定為允許 QoS \(選擇性\)

 根據預設，核心偵錯工具會封鎖 NetQos。 不論您用來安裝 DCB 的方法為何，如果您已在電腦上安裝內核偵錯工具，您必須將偵錯工具設定為允許透過執行下列命令來啟用和設定 QoS。

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>在 Windows 10 中安裝 DCB

您可以在 Windows 10 電腦上執行下列程式。

若要執行此程式，您必須是系統**管理員**的成員。

### <a name="install-dcb"></a>安裝 DCB

1. 按一下 [**開始**]，然後向下流覽至 [ **Windows 系統**]。
2. 按一下 [控制台]。 [**控制台**] 對話方塊隨即開啟。
3. 在 [**控制台**] 中，按一下 [ **View by**]，然後按一下**大圖示**或**小圖示**。
4. 按一下 [**程式和功能**]。 [程式和功能] 對話方塊隨即開啟。
5. 在 [**程式和功能**] 中，按一下左窗格中的 [**開啟或關閉 Windows 功能**]。 [ **Windows 功能**] 對話方塊隨即開啟。
6. 在 [ **Windows 功能**] 中，按一下 [**資料中心橋接**]，然後按一下 **[確定]** 。

![開啟或關閉 Windows 功能對話方塊](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


