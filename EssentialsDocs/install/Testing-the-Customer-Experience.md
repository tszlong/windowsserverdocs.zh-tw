---
title: 測試客戶經驗
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 938b64d96111a3ed128ec184acde56f20cb1abda
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819781"
---
# <a name="testing-the-customer-experience"></a>測試客戶經驗

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

若要驗證客戶經驗並檢查您的合作夥伴自訂項目，請完整執行目標電腦的「初始設定」。 建議您至少手動完成一次初始設定，以瞭解客戶經驗。 如果您在儀表板中同時加上您的標誌，則必須完成初始設定以確認商標。 如果您在遠端 Web 存取網站，則必須存取 HTTP：//< servername\> 以確認商標（< servername\> 是伺服器的名稱）。 您可以使用 cfg.ini 檔案的初始設定區段，自動測試客戶經驗。 如需在 cfg.ini 檔案中建立此區段的詳細資訊，請參閱[建立 Cfg.ini 檔案](Create-the-Cfg.ini-File.md)。  
  
> [!IMPORTANT]
>  您必須執行 Sysprep.exe 命令，以在測試初始設定經驗之前準備用於部署的映像。 如需執行 Sysprep.exe 的詳細資訊，請參閱[準備用於部署的映像](Preparing-the-Image-for-Deployment.md)。  
  
> [!IMPORTANT]
>  測試初始設定時需要網路連線。 伺服器上未設定或安裝 DHCP，如此讓可您在不受干擾的情況下進行網路測試。  
  
 若要確認合作夥伴支援資訊，請按一下儀表板中 [說明] 按鈕旁邊的向下箭頭。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)