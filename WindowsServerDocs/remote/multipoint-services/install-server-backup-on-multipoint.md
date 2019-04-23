---
title: 在您的 MultiPoint 伺服器上安裝伺服器備份
description: 將逐步引導您逐步完成安裝備份及復原工具
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 51932c5f0796cfd757d3322e10c17de2a3081f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832489"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>在您的 MultiPoint 伺服器上安裝伺服器備份
建議您考量您的 MultiPoint 伺服器的備份和復原計劃。
  
良好的備份和復原計劃是很重要的任何大小的環境。 Windows Server Backup 是提供一組精靈及其他工具，讓您針對安裝所在的伺服器執行基本備份及復原工作的 Windows Server 2016 中的功能。 您可以使用 Windows Server Backup 來備份完整伺服器 （所有磁碟區）、 選取的磁碟區、 系統狀態或特定檔案或資料夾，並建立可用來重建您的系統備份。  
  
您可以復原磁碟區、資料夾、檔案、特定應用程式以及系統狀態。 此外，諸如硬碟故障的災害時，您可以重建從頭或使用替代硬體的系統。 若要這樣做，您必須使用完整伺服器或只包含作業系統檔案和 Windows 修復環境磁碟區的備份。 這會還原至舊系統或新的硬碟，將完整的系統。  
  
Windows Server Backup 的重要功能是能夠排程自動執行備份。  
  
使用下列程序來設定您所需要的備份類型。  
  
## <a name="install-backup-and-recovery-tools"></a>安裝備份及復原工具  
  
1.  從**開始**畫面上，開啟**伺服器管理員**。  
  
2.  按一下 **新增角色及功能**以啟動 新增角色精靈。 然後按一下**下一步**檢閱之後**在開始之前**備忘稿。  
  
3.  選取 [**型角色型或功能型安裝**選項，然後再按一下**下一步]**。  
  
4.  選取本機電腦，您要管理，然後按一下**下一步**。  
  
    此時會開啟 [新增功能精靈]。  
  
5.  在上**選取功能**頁面上，依序展開 Windows Server Backup 功能，選取核取方塊**Windows Server Backup**並**命令列工具**，然後按一下  **下一步**。  
  
    > [!NOTE]  
    > 或者，如果您只想安裝嵌入式管理單元與 Wbadmin 命令列工具，依序展開**Windows Server Backup 功能**，然後選取**Windows Server Backup**只核取方塊，確定**命令列工具**已清除核取方塊。  
  
6.  在 **確認安裝選項**頁面上，檢閱您的選擇，然後按一下**安裝**。  
  
    如果在安裝時，會發生任何錯誤**安裝結果**頁面將會注意錯誤。  
  
7.  成功完成安裝之後，您應該能夠存取這些備份及復原工具：  
  
    -   若要開啟 Windows Server Backup 嵌入式管理單元，在**開始**畫面上，輸入**備份**，然後按一下**Windows Server Backup**結果中。  
  
    -   若要啟動 Wbadmin 工具，並檢視其命令的語法：在 **開始**畫面上，輸入**命令**。 在結果中，以滑鼠右鍵按一下**命令提示字元**，按一下**系統管理員身分執行**頁面上，然後再按一下底部**是**在確認提示。 在命令提示字元中，輸入**wbadmin /？** 然後按 ENTER。 您應該會看到命令語法和工具的描述。  
  
## <a name="configure-backups-using-windows-server-backup"></a>設定使用 Windows Server Backup 的備份  
  
-   請依照下列中的指導方針[備份伺服器](https://technet.microsoft.com/library/cc753528.aspx)。 