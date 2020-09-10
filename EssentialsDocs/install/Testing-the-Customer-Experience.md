---
title: 測試客戶經驗
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: c109fbd7ef2e4852e296f893ae8981ba8feeaed8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623334"
---
# <a name="testing-the-customer-experience"></a>測試客戶經驗

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

若要驗證客戶經驗並檢查您的合作夥伴自訂項目，請完整執行目標電腦的「初始設定」。 建議您至少手動完成一次初始設定，以瞭解客戶經驗。 如果您在儀表板中同時加上您的標誌，則必須完成初始設定以確認商標。 如果您在遠端 Web 存取網站，您必須存取 HTTP://<servername \> 來確認商標 ( # B1 servername \> 是伺服器) 的名稱。 您可以使用 cfg.ini 檔案的初始設定區段，自動測試客戶經驗。 如需在 cfg.ini 檔案中建立此區段的詳細資訊，請參閱[建立 Cfg.ini 檔案](Create-the-Cfg.ini-File.md)。

> [!IMPORTANT]
>  您必須執行 Sysprep.exe 命令，以在測試初始設定經驗之前準備用於部署的映像。 如需執行 Sysprep.exe 的詳細資訊，請參閱[準備用於部署的映像](Preparing-the-Image-for-Deployment.md)。

> [!IMPORTANT]
>  測試初始設定時需要網路連線。 伺服器上未設定或安裝 DHCP，如此讓可您在不受干擾的情況下進行網路測試。

 若要確認合作夥伴支援資訊，請按一下儀表板中 [說明] 按鈕旁邊的向下箭頭。

## <a name="see-also"></a>另請參閱
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案，[以準備映射以進行部署](Preparing-the-Image-for-Deployment.md)