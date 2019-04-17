---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: "指派網域名稱"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0d5ec9b76798f21503f650527a7961cff0b592b4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="assigning-domain-names"></a>指派網域名稱

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您必須計劃中指定每個網域名稱。 Active Directory Domain Services (AD DS) 網域有兩種類型的名稱： 網域名稱系統 」 (DNS) 和 NetBIOS 名稱。 一般而言，這兩個名稱會顯示使用者。 Active Directory 網域的 DNS 名稱包含兩個組件，尾碼前置詞。 建立網域名稱時，第一次判斷 DNS 前置詞。 這是在 DNS 網域名稱的第一個標籤。 尾碼會判斷當您選取的樹系根網域名稱。 下表列出的 DNS 名稱命名規則前置詞。  
  
|規則|解釋|  
|--------|---------------|  
|選取 [前置詞不會變成過時。|避免名稱，例如 product 列或作業系統，可能會在未來的變更。 我們建議使用的地理位置名稱。|  
|選取 [包含只網際網路標準字元前置詞。|A Z、 z，0-9 與 （-），但無法完全數字。|  
|包含的 15 字元或較少中前置詞。|如果您選擇首碼長度或較少的 15 字元，NetBIOS 名稱為前置詞相同。|  
  
如需詳細資訊，請查看 Active Directory 適用於電腦、網域、網站及 Ou 命名規格 ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629))。  
  
> [!NOTE]  
>  Windows Server 2008 和 Windows Server 2003 Dcpromo.exe 可讓您建立單一標籤 DNS 網域名稱，雖然您不應該使用的數個原因單一標籤 DNS 網域名稱。 在 Windows Server 2008 R2 Dcpromo.exe 不允許您建立單一標籤 DNS 網域名稱。 如需詳細資訊，請查看[https://go.microsoft.com/fwlink/?LinkId=92467。](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
如果目前 NetBIOS 的網域名稱不當代表地區或滿足命名規則前置詞失敗時，選取新的前置詞。 若是如此，NetBIOS 的網域名稱是不同的 DNS 網域前置詞。  
  
每個新的網域部署，選取首碼所定義適合地區的滿足命名規則前置詞。 我們建議您的網域名稱 NetBIOS 為相同的 DNS 前置詞。  
  
文件 DNS 前置詞和 NetBIOS 您選取每個您森林中的網域名稱。 您可以將 DNS 和 NetBIOS 名稱資訊新增到 「 規劃網域 」 試算表，您要建立的全新和已升級網域計劃的文件。 若要打開它，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 從工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) 和開放「網域規劃」(DSSLOGI_5.doc).  
  


