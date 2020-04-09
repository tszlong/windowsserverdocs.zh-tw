---
title: 建立簡單自訂映象
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1c3c47a9cab8e08768cef265ff823d383eda4a74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818351"
---
# <a name="create-a-simple-customized-image"></a>建立簡單自訂映象

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以使用下列程序來建立簡單的自訂映像：  
  
#### <a name="to-create-the-image"></a>若要建立映像  
  
1.  安裝伺服器之後，請在初始設定的第一個頁面按下 Shift+F10 以啟動 cmd 視窗。  
  
2.  在系統磁碟機的根目錄下建立 SkipIC.txt 檔案。  
  
3.  重新啟動伺服器。  
  
4.  使用包含 unattend.xml 檔案的 USB 快閃磁碟機或 DVD 啟動伺服器。 如需建立可開機 USB 快閃磁碟機的詳細資訊，請參閱[建立可開機的 USB 快閃磁碟機](Create-a-Bootable-USB-Flash-Drive.md)。  
  
5.  將標誌商標新增至儀表板。 如需新增商標的詳細資訊，請參閱[將商標新增至儀表板、遠端 Web 存取和啟動列](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md)。  
  
6.  建立 OOBE.xml 檔案以顯示自訂資訊，如公司名稱、標誌及 EULA。 如需 OOBE.xml 檔案的詳細資訊，請參閱 [Create the Oobe.xml File Including Logo and EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md)。  
  
7.  如果未在 unattend.xml 中定義預設伺服器名稱，請變更該名稱。  
  
8.  根據預設，伺服器名稱將是隨機字串。 請將伺服器名稱變更為其他字串 (例如 ContosoServer)，然後將新的伺服器名稱告知客戶。  
  
9. 如[準備用於部屬的映像](Preparing-the-Image-for-Deployment.md)中所述，準備用於部屬的映像。  
  
## <a name="see-also"></a>另請參閱  
 [消費者入門與 Windows Server ESSENTIALS ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)