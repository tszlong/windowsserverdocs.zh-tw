---
title: "建立簡單的自訂映像"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e18ff5ded94127449072d28d00b98e17dbe63c3a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-simple-customized-image"></a>建立簡單的自訂映像

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以使用下列程序來建立簡單的自訂映像：  
  
#### <a name="to-create-the-image"></a>若要建立的影像  
  
1.  伺服器安裝之後，請在第一頁的初始設定中，按 Shift + F10 稍 cmd 視窗。  
  
2.  建立系統磁碟機的根 SkipIC.txt 檔案。  
  
3.  重新開機伺服器。  
  
4.  開始使用 USB 快閃磁碟機或 DVD，包括 unattend.xml 檔案伺服器。 有關如何建立可開機的 USB 快閃磁碟機，請查看[建立可開機的 USB 快閃磁碟機]](Create-a-Bootable-USB-Flash-Drive.md)。  
  
5.  新增商標商標儀表板。 如新增商標的相關詳細資訊，請查看[[新增至儀表板，遠端網路存取，Launchpad 商標](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md)。  
  
6.  建立顯示自訂的資訊，例如公司名稱、商標，以及使用者授權合約 OOBE.xml 檔案。 如需 OOBE.xml 檔案，請查看[建立 Oobe.xml 檔案包括商標及使用者授權合約](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md)。  
  
7.  如果您未定義它 unattend.xml 中，變更預設伺服器名稱。  
  
8.  根據預設伺服器名稱會隨機字串。 變更（例如，ContosoServer) 的另一個字串伺服器名稱，再通知您客戶伺服器名稱。  
  
9. 準備部署映像中所述[準備部署映像](Preparing-the-Image-for-Deployment.md)。  
  
## <a name="see-also"></a>也了  
 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)