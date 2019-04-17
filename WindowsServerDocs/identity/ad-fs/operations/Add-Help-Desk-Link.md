---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: "加入協助支援連結"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d16cc0a75bfe636c29b44687b669e87f31b69ce
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/20/2018
---
# <a name="add-help-desk-link"></a>加入協助支援連結 

>適用於：Windows Server 2016、Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>若要新增的協助支援連結  
若要新增協助 desk 連結，會顯示在 sign\ 在頁面上，使用下列的 Windows PowerShell PowerShell cmdlet 和語法。  

![新增工程師](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> `linkText`在這個 cmdlet 參數不需要，除非您要使用預設值，也就是比另一個值*協助*。 使用預設的優點是他們當地語系化 client 所有的地區設定。 自訂頁面」之後，自訂的優先順序。因此，您應該自訂為所有要支援的語言。  


## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
