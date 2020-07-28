---
title: 安裝或移除語言套件
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: fa167ec12122883fc0ccea914260407d13f0b992
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181164"
---
# <a name="install-or-remove-language-packs"></a>安裝或移除語言套件

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

> [!NOTE]
>  在新增 Windows Server Essentials 語言套件之前，您必須先建立[語言套件和部署](https://technet.microsoft.com/library/hh824829)中所述的多語系 Windows 映像。

 語言套件只能用來建立多語言映像。 本節中的資訊是在 Windows Server Essentials 上安裝或移除語言套件的特定資訊。

> [!NOTE]
>  如果您想從不支援東亞語言 (如 ja-jp) 的用戶端電腦執行初始設定 (IC)，且伺服器上的多語言映像不含英文，則 IC 網頁將會顯示方塊。 若要讓 IC 網頁預設為英文，您所建立的多語言映像必須包含英文。

## <a name="adding-language-packs-to-an-image"></a>將語言套件新增至映像
 OEM 自訂 DVD 提供語言套件。 建議您先將語言套件複製至技術人員電腦，之後才將語言套件新增至映像。

 您必須使用下列命令來安裝語言套件：

 **dism.exe/online/Add-Package/PackagePath： C： \\<cab 檔案目錄的完整路徑 \>\lp.cab**

 例如，下列命令顯示如何新增德文語言套件：

 **dism.exe /online /Add-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**

> [!IMPORTANT]
>  您也必須套用 Windows Server Essentials 的語言套件，才能將作業系統完全當地語系化。

## <a name="removing-language-packs-from-an-image"></a>從映像中移除語言套件
 您可以使用下列命令來移除不再想要併入映像的語言套件：

 **dism.exe/online/Remove-Package/PackagePath： C： \\<cab 檔案目錄的完整路徑 \>\lp.cab**

 例如，下列命令顯示如何移除德文語言套件：

 **dism.exe /online /Remove-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**

## <a name="see-also"></a>另請參閱

 [建立和自訂映射額外的](Creating-and-Customizing-the-Image.md)[自訂](Additional-Customizations.md)[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)

