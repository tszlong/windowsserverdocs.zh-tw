---
title: 測試客戶經驗
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 223b0e1be3a53e9a7d198dc005fc8725e421db58
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838689"
---
# <a name="testing-the-customer-experience"></a>測試客戶經驗

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

若要驗證客戶經驗並檢查您的合作夥伴自訂項目，請完整執行目標電腦的「初始設定」。 建議您至少手動完成一次初始設定，以瞭解客戶經驗。 如果您在儀表板中同時加上您的標誌，則必須完成初始設定以確認商標。 如果您 cobranded 遠端 Web 存取網站時，您必須存取 http://<servername\>以確認商標 (< 伺服器名稱\>是伺服器的名稱)。 您可以使用 cfg.ini 檔案的初始設定區段，自動測試客戶經驗。 如需在 cfg.ini 檔案中建立此區段的詳細資訊，請參閱[建立 Cfg.ini 檔案](Create-the-Cfg.ini-File.md)。  
  
> [!IMPORTANT]
>  您必須執行 Sysprep.exe 命令，以在測試初始設定經驗之前準備用於部署的映像。 如需執行 Sysprep.exe 的詳細資訊，請參閱 [Preparing the Image for Deployment](Preparing-the-Image-for-Deployment.md)。  
  
> [!IMPORTANT]
>  測試初始設定時需要網路連線。 伺服器上未設定或安裝 DHCP，如此讓可您在不受干擾的情況下進行網路測試。  
  
 若要確認合作夥伴支援資訊，請按一下儀表板中 [說明] 按鈕旁邊的向下箭頭。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)