---
title: 在 MultiPoint 伺服器上安裝伺服器備份
description: 引導您完成安裝備份和修復工具的步驟
ms.date: 07/22/2016
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 82d017767a5e6e601e70ff28461848b7673bcd00
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970515"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>在 MultiPoint 伺服器上安裝伺服器備份
建議您考慮 MultiPoint 伺服器的備份和復原計畫。

良好的備份和復原計畫對於任何規模的環境而言非常重要。 Windows Server Backup 是 Windows Server 2016 中的一項功能，可提供一組可讓您針對安裝它的伺服器執行基本備份和復原工作的工具和其他工具。 您可以使用 Windows Server Backup 來備份完整伺服器 (所有磁片區) 、選取的磁片區、系統狀態或特定的檔案或資料夾，以及建立可用於重建系統的備份。

您可以復原磁碟區、資料夾、檔案、特定應用程式以及系統狀態。 而且，對於硬碟故障之類的嚴重損壞，您可以從頭開始或使用替代硬體重建系統。 若要這樣做，您必須擁有完整伺服器的備份，或是只有包含作業系統檔案和 Windows 修復環境的磁片區。 這會將您的完整系統還原到舊系統或新的硬碟上。

Windows Server Backup 的一項主要功能是能夠排程自動執行備份。

請使用下列程式來設定您所需的備份類型。

## <a name="install-backup-and-recovery-tools"></a>安裝備份和修復工具

1.  從 [**開始**] 畫面開啟 [**伺服器管理員**]。

2.  按一下 [**新增角色及功能**] 以啟動 [新增角色嚮導]。 然後在您完成筆記**之前**，按一下 [**下一步]** 。

3.  選取 [以**角色為基礎] 或 [功能**型] 安裝選項，然後按 **[下一步]**。

4.  選取您要管理的本機電腦，然後按 **[下一步]**。

    此時會開啟 [新增功能精靈]。

5.  在 [**選取功能**] 頁面上，展開 [Windows Server Backup 功能]，選取**Windows Server Backup**和**命令列工具**的核取方塊，然後按 **[下一步]**。

    > [!NOTE]
    > 或者，如果您只想要安裝嵌入式管理單元和 Wbadmin 命令列工具，請展開 [ **Windows Server Backup 功能**]，然後只選取 [ **Windows Server Backup** ] 核取方塊，確認 [**命令列工具**] 核取方塊是清除的。

6.  在 [**確認安裝選項**] 頁面上，檢查您的選擇，然後按一下 [**安裝**]。

    如果在安裝期間發生任何錯誤，[**安裝結果**] 頁面將會記錄錯誤。

7.  成功完成安裝之後，您應該能夠存取這些備份和修復工具：

    -   若要開啟 [Windows Server Backup] 嵌入式管理單元，請在 [**開始**] 畫面上輸入**Backup**，然後按一下結果中的 [ **Windows Server Backup** ]。

    -   若要啟動其命令的 Wbadmin tool 和 view 語法：在 [**開始**] 畫面上，輸入**命令**。 在結果中，以滑鼠右鍵按一下 [**命令提示**字元]，按一下頁面底部的 [**以系統管理員身分執行**]，然後在確認提示中按一下 [**是]** 。 在命令提示字元中，輸入**wbadmin/？** 然後按 ENTER 鍵。 您應該會看到工具的命令語法和描述。

## <a name="configure-backups-using-windows-server-backup"></a>使用 Windows Server Backup 設定備份

-   請遵循[備份伺服器](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753528(v=ws.11))中的指引。
