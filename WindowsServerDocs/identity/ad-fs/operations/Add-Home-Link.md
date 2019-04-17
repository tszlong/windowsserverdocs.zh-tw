---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: "加入家庭的連結"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="add-home-link"></a>加入家庭的連結 

>適用於：Windows Server 2016、Windows Server 2012 R2

若要加入家庭連結，會顯示在 sign\ 在頁面上，使用下列的 Windows PowerShell cmdlet 和語法。 


![加入家庭的連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> `linkText`在這個 cmdlet 參數不需要，除非您要使用預設值，也就是比另一個值*Home*。 使用預設的優點是他們當地語系化 client 所有的地區設定。 自訂 sign\ 頁面會之後，自訂的優先順序。因此，您應該自訂為所有要支援的語言。

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
