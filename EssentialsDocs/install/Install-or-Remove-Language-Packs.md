---
title: "安裝或移除語言套件"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a41b491bbe4b4a8ee7f9743dc85e5bdaffb08496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-or-remove-language-packs"></a>安裝或移除語言套件

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

> [!NOTE]
>  中所述，您必須先建立多種語言的 Windows 映像[的語言套件和部署](https://technet.microsoft.com/library/hh824829)之前您將 Windows Server Essentials 語言套件。  
  
 語言套件只適用於建立多語言映像。 在本區段中的資訊是適用於安裝或移除語言套件，在 Windows Server Essentials。  
  
> [!NOTE]
>  如果您想要執行 client 的電腦不支援 ja-jp，例如東亞語言的初始設定 (IC) 以及英文不包含在伺服器上多語系映像，IC 網頁將會顯示的方塊。 多語您建立映像必須包含英文預設值為英文 IC 網頁。  
  
## <a name="adding-language-packs-to-an-image"></a>新增語言套件的影像  
 語言套件可用的自訂項目 OEM dvd。 建議您複製的語言套件到電腦的技術人員語言套件新增至映像前。  
  
 若要安裝語言套件，您必須使用下列命令：  
  
 **dism.exe /online /Add-Package /PackagePath:C: \\ < 完整 cab directory\ 檔案路徑 > \lp.cab**  
  
 例如，下列命令示範如何新增德國語言套件：  
  
 **dism.exe /online /Add-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  您必須也適用於 Windows Server Essentials 完全當地語系化作業系統的語言套件。  
  
## <a name="removing-language-packs-from-an-image"></a>移除語言套件的影像  
 若要移除您不想要包含影像中的語言套件，您可以使用下列命令：  
  
 **dism.exe /online /Remove-Package /PackagePath:C: \\ < 完整 cab directory\ 檔案路徑 > \lp.cab**  
  
 例如，下列命令示範如何移除德國語言套件：  
  
 **dism.exe /online /Remove-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>也了  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

