---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: 指派網域名稱
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba5a7ee8b7d9728c48e2798853aab8d55047e86f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866559"
---
# <a name="assigning-domain-names"></a>指派網域名稱

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您必須計劃中，將名稱指派給每個網域中。 Active Directory 網域服務 (AD DS) 網域中有兩種類型的名稱：網域名稱系統 (DNS) 名稱和 NetBIOS 名稱。 一般情況下，這兩個名稱會顯示給使用者。 Active Directory 網域的 DNS 名稱中包含兩個組件、 前置詞和後置詞。 建立網域名稱時，先判斷 DNS 前置詞。 這是在網域的 DNS 名稱的第一個標籤。 當您選取的樹系根網域名稱，尾碼是由決定。 下表列出的 DNS 名稱的命名規則的前置詞。  
  
|規則|說明|  
|--------|---------------|  
|選取不太可能會變成過期的前置詞。|避免使用名稱，例如產品線或可能會在未來變更的作業系統。 我們建議使用地理位置的名稱。|  
|選取包含僅限網際網路標準字元的前置詞。|A-Z、 a-z、 0-9 和 （-），但不是完全的數值。|  
|包含 15 個字元以內的前置詞。|如果您選擇的前置長度為 15 個字元以內，NetBIOS 名稱等同於前置詞。|  
  
如需詳細資訊，請參閱 Active Directory 中的電腦、 網域、 網站和 Ou 的命名慣例 ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629))。  
  
> [!NOTE]  
>  雖然 Windows Server 2008 和 Windows Server 2003 中的 Dcpromo.exe 可以讓您建立單一標籤的 DNS 網域名稱，但是因為一些原因，您不應該將單一標籤的 DNS 名稱用於網域。 在 Windows Server 2008 R2 中，Dcpromo.exe 不允許您為網域建立單一標籤的 DNS 名稱。 如需詳細資訊，請參閱 < [ https://go.microsoft.com/fwlink/?LinkId=92467。](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
如果目前的網域的 NetBIOS 名稱不適合用來代表區域，或無法滿足的前置詞命名規則，請選取 新的前置詞。 在此情況下，網域的 NetBIOS 名稱是不同網域的 DNS 首碼。  
  
如您所部署的每個新網域，選取適合的區域，並滿足之前置詞命名規則的前置詞。 我們建議在網域的 NetBIOS 名稱是 DNS 首碼相同。  
  
文件的 DNS 首碼和您選取您的樹系中每個網域的 NetBIOS 名稱。 您可以加入您建立用來記錄您的計劃，為新的和升級網域 」 網域計劃 」 工作表的 DNS 及 NetBIOS 名稱資訊。 若要開啟它，請從 工作輔助工具的 Windows Server 2003 Deployment Kit 下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，然後開啟 「 網域規劃 」 (DSSLOGI_5.doc)。  
  


