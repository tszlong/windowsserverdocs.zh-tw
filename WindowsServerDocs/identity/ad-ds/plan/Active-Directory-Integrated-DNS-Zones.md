---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: "Active Directory 整合 DNS 區域"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 11940ddda320ea36bf8439afed91fcf6803f2fee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-integrated-dns-zones"></a>Active Directory 整合 DNS 區域

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

執行網域控制站的網域名稱系統」(DNS) 伺服器可以將其區域儲存在 Active Directory Domain Services (AD DS)。 如此一來，不需要設定另一個 DNS 複寫拓撲使用一般的 DNS 區域轉送因為所有區域資料會自動都複製透過複寫 Active Directory。 這簡化部署 DNS 的程序，並提供下列優點：  
  
-   多個主機建立 DNS 複寫。 因此，執行 DNS 伺服器服務的網域中的任何網域控制站可以撰寫 Active Directory 整合 DNS 網域名稱他們的授權的區域的更新。 另外 DNS 區域傳輸拓撲就不需要。  
  
-   支援的安全的動態更新。 安全的動態更新可讓您控制哪些電腦更新哪些名稱，以避免覆寫現有的 dns 名稱未經授權的電腦系統管理員。  
  
Windows Server 2008 的 active Directory 整合 DNS 儲存應用程式 directory 磁碟分割區資料。 （有任何行為變更的 Active Directory 與 Windows Server 2003 DNS 整合。）下列的 DNS 特定應用程式 directory 磁碟分割 AD DS 安裝期間建立：  
  
-   樹系應用程式 directory 磁碟分割，稱為「ForestDnsZones  
  
-   針對每個、樹系網域全網域應用程式 directory 磁碟分割名 DomainDnsZones  
  
如需如何 AD DS DNS 會將資訊儲存的應用程式的磁碟分割的相關資訊，請查看[DNS 技術參考](https://go.microsoft.com/fwlink/?LinkId=106636)。  
  
> [!NOTE]  
> 我們建議您安裝 DNS，當您執行 Active Directory Domain Services 安裝精靈 (Dcpromo.exe)。 若您這樣做，精靈會自動建立 DNS 區域委派。 如需詳細資訊，請查看[部署 Windows Server 2008 森林根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  


