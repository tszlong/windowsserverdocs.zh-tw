---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: 電腦命名
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae2483571d67b4cdb32c2a547b924b1315573da0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842159"
---
# <a name="computer-naming"></a>電腦命名

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

當執行於 Windows 2000，Windows XP、 Windows Server 2003、 Windows Server 2008 或 Windows Vista 作業系統的電腦加入網域，根據預設電腦會指派本身的名稱。 它會指派本身的名稱包含電腦 （也就是在系統內容中的電腦名稱） 的主機名稱和電腦加入 Active Directory 網域的網域名稱系統 (DNS) 名稱 （也就是主要的 DNS 尾碼系統屬性）。 串連的主機名稱和網域的 DNS 名稱就是所謂的完整的網域名稱 (FQDN)。 比方說，如果主機的電腦名稱 Server1 聯結 corp.contoso.com 網域，電腦的 FQDN 會是 server1.corp.contoso.com。  
  
如果電腦已有不同的 DNS 網域名稱是以靜態方式輸入 DNS 區域或登錄的整合的 DNS/動態主機設定通訊協定 (DHCP) 伺服器服務，電腦的 FQDN 是已註冊的名稱有所區別先前。 電腦可以是任一名稱參考。  
  
如需有關 Active Directory 網域服務 (AD DS) 中的命名慣例的詳細資訊，請參閱 Active Directory 中的命名慣例，電腦、 網域、 網站與組織單位 ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629))。  
  


