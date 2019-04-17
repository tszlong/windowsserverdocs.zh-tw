---
title: 安裝資料中心橋接 (DCB)，在 Windows Server 或 Client
description: 本主題提供您如何安裝 Windows Server 或 Windows Client 的資料中心橋接上的指示操作。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 491bdeb1a7458be1f991be68724e7a7b51f67ecf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>安裝橋接 \(DCB\) Windows Server 2016 或 Windows 10 中的資料中心

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何在 Windows Server 2016 或 Windows 10 上安裝 DCB。

## <a name="prerequisites-for-using-dcb"></a>使用 DCB 必要條件

以下是設定及管理 DCB 的必要條件。

### <a name="install-a-compatible-operating-system"></a>安裝相容的作業系統

在下列作業系統中，您可以使用此指南 DCB 命令。

- Windows Server（以每年次通道）
- Windows Server 2016
- Windows 10 \(all versions\)

在下列作業系統包含的舊版 DCB 不是相容 DCB Windows Server 2016 和 Windows 10 的文件中所使用的命令。

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>硬體需求

以下是 DCB 的硬體需求的清單。

- DCB\ 能力乙太網路 adapter\(s\) 必須安裝在 Windows Server 2016 DCB 提供的電腦。
- 網路上必須部署參數 DCB\ 支援的硬體。


## <a name="install-dcb-in-windows-server-2016"></a>Windows Server 2016 中安裝 DCB

若要執行的 Windows Server 2016 的電腦上安裝 DCB，您可以使用下列各節。

**管理認證**

執行下列程序，您必須成員可以的**系統管理員**。

### <a name="install-dcb-using-windows-powershell"></a>安裝 DCB 使用 Windows PowerShell

您可以使用 Windows PowerShell 來安裝 DCB 使用下列程序。

1. 在執行 Windows Server 2016 的電腦，請按一下**[開始]**，然後以滑鼠右鍵按一下 [Windows PowerShell 圖示。 會出現功能表。 功能表中，按一下 [**更多**，然後按一下 [**以系統管理員身分執行**。 如果出現提示，請輸入認證帳號在電腦上的系統管理員權限。 Windows PowerShell 開啟以系統管理員權限。
2. 輸入下列命令，，然後按 ENTER 鍵。

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>安裝 DCB 使用伺服器管理員

您可以使用下列程序使用伺服器管理員安裝 DCB。

>[!NOTE]
>之後您的第一個步驟執行這個程序，**在您開始之前**頁面上新增角色與精靈中的功能不顯示如果先前已選取**預設略過此頁面**功能精靈與新增的角色執行。 如果**在您開始之前，請先**頁面上未顯示，步驟 1 跳至步驟 3。

1. 在 DC1，在伺服器管理員中，按一下**管理**，然後按一下 [**新增角色與功能**。 新增角色與功能精靈開啟。
2. 在**在您開始之前，請先**，按一下 [**下**。
3. 在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。
4. 在**選取目的伺服器**，確保**選取伺服器伺服器集區的**選取。 在**伺服器集區**，請確定已選取 [本機電腦。 按一下**下一步**。
5. 在**選擇伺服器角色**，按一下 [**下**。
6. 在**選擇功能**，請在**功能**，按一下 [**資料中心橋接**。 對話方塊要求您是否要將新增 DCB 所需的功能。 按一下**[新增功能**。
7. 在**選擇功能**，按一下 [**下**。 
8. 7.的**確認安裝選項**，按一下 [**安裝**。 **安裝進度**頁面上的安裝程序期間會顯示狀態。 安裝該成功的訊息會出現 stating 之後，請按一下**關閉**。

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>設定允許 QoS \(Optional\) 核心偵錯工具

 根據預設，核心偵錯工具封鎖 NetQos。 無論您用來安裝 DCB，如果您有核心偵錯工具安裝於電腦的方法，您必須設定允許 QoS 控制和設定，執行下列命令，偵錯工具。

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>在 Windows 10 中安裝 DCB

您可以在 Windows 10 電腦上，執行下列程序。

若要執行此程序，您必須成員的**系統管理員**。

### <a name="install-dcb"></a>安裝 DCB

1. 按一下**[開始]**，然後向下捲動並按一下 [ **Windows 系統**。
2. 按一下**控制台**。 **[控制台]**對話方塊。
3. 在**[控制台]**，按一下**，以檢視**，然後按一下 \**大圖示**或**小圖示**。
4. 按一下**程式和功能**。 [程式和功能] 對話方塊。
5. 在**程式和功能**，在左窗格中，按**Windows 功能或關閉**。 **Windows 功能**對話方塊。
6. 在**Windows 功能**，按一下 [**資料中心橋接**，然後按一下 [ **[確定]**。

![關閉對話方塊中，關閉 Windows 功能](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


