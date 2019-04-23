---
title: 安裝資料中心橋接 (DCB) 在 Windows Server 或用戶端
description: 本主題為您提供有關如何安裝在 Windows Server 或 Windows 用戶端的資料中心橋接的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7c20ef027279780181ff176afa39a19f2976c4c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845439"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>安裝資料中心橋接\(DCB\)在 Windows Server 2016 或 Windows 10

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何在 Windows Server 2016 或 Windows 10 安裝 DCB。

## <a name="prerequisites-for-using-dcb"></a>使用 DCB 的必要條件

以下是設定和管理 DCB 的必要條件。

### <a name="install-a-compatible-operating-system"></a>安裝相容的作業系統

在下列作業系統中，您可以使用本指南 DCB 命令。

- Windows Server (半年度管道)
- Windows Server 2016
- Windows 10\(所有版本\)

在下列作業系統包含舊版 DCB 不使用 Windows Server 2016 和 Windows 10 的 DCB 文件中的命令相容。

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>硬體需求

下列是 DCB 硬體需求的清單。

- DCB\-支援的乙太網路介面卡\(s\)必須安裝在提供 Windows Server 2016 DCB 的電腦。
- DCB\-支援的硬體交換器必須部署在網路上。


## <a name="install-dcb-in-windows-server-2016"></a>Windows Server 2016 中安裝 DCB

您可以使用下列各節，以執行 Windows Server 2016 的電腦上安裝 DCB。

**系統管理認證**

若要執行這些程序，您必須隸屬**系統管理員**。

### <a name="install-dcb-using-windows-powershell"></a>安裝使用 Windows PowerShell 的 DCB

若要使用 Windows PowerShell 安裝 DCB，您可以使用下列程序。

1. 在執行 Windows Server 2016 的電腦，按一下 **啟動**，然後以滑鼠右鍵按一下 Windows PowerShell 圖示。 此時會出現一個功能表。 在功能表中，按一下**更多**，然後按一下**系統管理員身分執行**。 如果出現提示，請輸入電腦具有系統管理員權限帳戶的認證。 Windows PowerShell 會開啟以系統管理員權限。
2. 輸入下列命令，然後按 ENTER。

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>安裝使用伺服器管理員的 DCB

若要使用伺服器管理員安裝 DCB，您可以使用下列程序。

>[!NOTE]
>當您在此程序中，執行的第一個步驟之後**在您開始前**如果您先前已選取未顯示的 新增角色及功能精靈 的頁面**略過此頁面，依預設**時新增已執行的角色及功能精靈。 如果**在您開始前**頁面不會顯示，步驟 1 中跳至步驟 3。

1. 在 DC1 上，在 [伺服器管理員] 中，按一下**管理**，然後按一下**新增角色及功能**。 [新增角色及功能精靈] 隨即開啟。
2. 在 [在您開始前] 中，按 [下一步]。
3. 在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。
4. 在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 [下一步] 。
5. 在 [選取伺服器角色] 中按 [下一步]。
6. 在 **選取的功能**，請在**功能**，按一下 **資料中心橋接**。 對話方塊隨即開啟，要求是否您想要新增所需的 DCB 功能。 按一下 **將功能加入**。
7. 在 [**選取的功能**，按一下**下一步]**。 
8. 7.在**確認安裝選項**，按一下**安裝**。 **Průběh instalace**頁面會顯示安裝程序期間的狀態。 訊息會出現之後, 該安裝成功，請按一下**關閉**。

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>核心偵錯工具可讓 QoS 設定\(選擇性\)

 根據預設，核心偵錯工具會封鎖 NetQos。 不論您用來安裝 DCB，如果您有安裝在電腦的核心偵錯的方法，您必須設定偵錯工具，以便啟用並執行下列命令來設定 QoS。

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>在 Windows 10 中安裝 DCB

您可以在 Windows 10 電腦上執行下列程序。

若要執行此程序，您必須隸屬**系統管理員**。

### <a name="install-dcb"></a>安裝 DCB

1. 按一下 **開始**，然後向下捲動並按一下  **Windows 系統**。
2. 按一下 [控制台] 。 **控制台中**對話方塊隨即開啟。
3. 在**控制台中**，按一下**藉由檢視**，然後按一下**大圖示**或**小圖示**。
4. 按一下 **程式和功能**。 [程式和功能] 對話方塊隨即開啟。
5. 在 **程式和功能**，在左窗格中，按一下**開啟 Windows 功能開啟或關閉**。 **Windows 功能**對話方塊隨即開啟。
6. 在  **Windows 功能**，按一下**資料中心橋接**，然後按一下 **確定**。

![開啟或關閉對話方塊，開啟 Windows 功能](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


