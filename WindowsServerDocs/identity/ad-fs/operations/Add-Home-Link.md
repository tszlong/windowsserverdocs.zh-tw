---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: 新增首頁連結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4a6210b1b7475a4ec34bbe0575915f376381fe1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859381"
---
# <a name="add-home-link"></a>新增首頁連結 

若要新增 [登\-] 頁面中顯示的首頁連結，請使用下列 Windows PowerShell Cmdlet 和語法。 


![新增主連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Home*。 使用預設的優點是會根據所有用戶端地區設定來當地語系化。 自訂頁面中的 登入\-之後，自訂的優先順序會較高;因此，您應該針對您想要支援的所有語言進行自訂。

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
