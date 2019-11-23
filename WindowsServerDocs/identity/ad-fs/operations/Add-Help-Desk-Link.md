---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: 新增服務台連結
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1673e6ee6357a9d59e8ac5891625d453bb434088
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358460"
---
# <a name="add-help-desk-link"></a>新增服務台連結 


## <a name="to-add-a-help-desk-link"></a>新增技術支援中心連結  
若要新增 [登\-] 頁面中顯示的服務台連結，請使用下列 Windows PowerShell Cmdlet 和語法。  

![新增技術支援中心](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Help*。 使用預設的優點是會根據所有用戶端地區設定來當地語系化。 在自訂頁面之後，自訂會有較高的優先順序；因此，您應該自訂您想要支援的所有語言。  


## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
