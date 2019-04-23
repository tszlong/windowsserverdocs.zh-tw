---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: 檢閱 OU 設計概念
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f05104466c1cedcfbc8d94060ffa8fbfd9d18033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832169"
---
# <a name="reviewing-ou-design-concepts"></a>檢閱 OU 設計概念

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

網域的組織單位 (OU) 結構包含下列項目：  
  
-   OU 階層的圖表  
  
-   Ou 清單  
  
-   針對每個 OU:  
  
    -   Ou 用途  
  
    -   使用者或群組都有控制權的 OU 或物件的 OU 中的清單  
  
    -   使用者和群組擁有物件的 OU 的控制項類型  
  
OU 階層不需要以反映部門的組織或群組的階層。 應用程式的群組原則，或是限制可見性的物件時，會針對特定的目的，例如管理委派建立 Ou。  
  
您可以設計 OU 結構，將管理委派給人員或組織需要管理他們自己的資源和資料自主性的群組。 Ou 代表系統管理界限，並讓您控制資料管理員的授權單位的範圍。  
  
比方說，您可以建立稱為 ResourceOU OU，並使用它來儲存屬於檔案和列印伺服器管理群組的所有電腦帳戶。 然後，您可以設定安全性在 OU 中，以便僅在群組中的資料系統管理員可以存取的 OU。 這可防止遭竄改的檔案和列印伺服器帳戶的其他群組中的資料管理員。  
  
您可以進一步精簡 OU 結構，藉由建立特定的用途，例如應用程式的群組原則，或是限制可見性的受保護的物件，以便只有特定使用者可以看到這些 Ou 的樹狀子目錄。 比方說，如果您需要將群組原則套用至選取的使用者或群組資源，您可以將這些使用者或資源新增到 OU，並再套用到該 OU 的 群組原則。 您也可以使用 OU 階層來啟用進一步的系統管理控制的委派。  
  
OU 結構中的層級數目沒有技術限制時，管理能力我們建議您限制您的 OU 結構，最多 10 個層級的深度。 沒有任何技術限制的 Ou 上每個層級數目。 請注意，Active Directory 網域服務 (AD DS)-已啟用的應用程式可能會對用於辨別名稱 （也就是完整輕量型目錄存取通訊協定 (LDAP) 路徑的目錄中的物件） 或字元數的限制在階層內的 OU 深度。  
  
在 AD DS 中的 OU 結構不是要讓使用者看到。 OU 結構是服務系統管理員和資料管理員的系統管理工具，而且很輕鬆地變更。 檢閱並更新您的 OU 結構設計以反映您的系統管理結構中的變更，並支援以原則為基礎的系統管理繼續。  
  


