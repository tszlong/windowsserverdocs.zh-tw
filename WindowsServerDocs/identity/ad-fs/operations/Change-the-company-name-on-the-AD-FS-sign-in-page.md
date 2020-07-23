---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: 變更 AD FS 登入頁面上的公司名稱
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f32abbfbc74ad81dfeab5aed403178be4a9b0a57
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953740"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司名稱
 
若要變更登入頁面上顯示的公司名稱 \- ，請使用下列 Windows PowerShell Cmdlet 和語法。 這個值預設使用安裝期間輸入的 Federation Service 顯示名稱的值來設定。  

![變更名稱](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> 您也可以使用 Windows PowerShell 整合式腳本環境 \( ISE \) 來變更公司名稱。 藉由使用 Windows PowerShell ISE，您可以在 Unicode 相容環境中顯示內容 \- 。 如需詳細資訊，請參閱 [Windows PowerShell ISE 簡介](/previous-versions/mt707506(v=msdn.10))。  

## <a name="additional-references"></a>其他參考 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
  
