---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: 變更 AD FS 登入頁面上的圖例
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: de0fc7af4c4eecad59b7af6b08426ef207da5db1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859951"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的圖例

## <a name="change-the-illustration"></a>變更圖例  


若要變更圖例（顯示在 [登\-] 頁面上的左側圖形），請使用下列 Windows PowerShell Cmdlet 和語法。  

![變更圖例](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 建議插圖尺寸為 1420x1080 pixels @ 96 DPI，檔案大小不超過 200 KB。  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
  
  
