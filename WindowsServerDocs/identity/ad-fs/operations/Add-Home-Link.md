---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: 新增首頁連結
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a9a043390f5bfb412e549779ed4a9048d1c8a0b5
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190177"
---
# <a name="add-home-link"></a>新增首頁連結 

若要新增登顯示的首頁連結\-在頁面上，使用下列 Windows PowerShell cmdlet 和語法。 


![新增首頁連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Home*。 使用預設的優點是會根據所有用戶端地區設定來當地語系化。 號後面\-頁面中為自訂，自訂會有優先順序; 因此，您應該自訂您想要支援的所有語言。

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
