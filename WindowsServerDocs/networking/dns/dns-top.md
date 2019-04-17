---
title: 網域名稱系統」(DNS)
description: 本主題提供 Windows Server 2016 中 DNS 的概觀
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23851d2d8015fc6ae9e0653e8a0843f8c4295162
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="domain-name-system-dns"></a>網域名稱系統」(DNS)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

網域名稱系統」(DNS) 是通訊協定構成 TCP/IP，業界標準系列，並在一起 DNS Client 和 DNS 伺服器提供電腦名稱 TO-IP 位址對應名稱解析度服務到電腦和使用者。  
  
> [!NOTE]  
> 本主題中，除了下列的 DNS content 使用。  
>   
> -   [DNS Client 中的新功能](What-s-New-in-DNS-Client.md)  
> -   [在 [DNS 伺服器的新功能](What-s-New-in-DNS-Server.md)  
> -   [DNS 原則案例指南](deploy/DNS-Policy-Scenario-Guide.md)  
> -   影片：[Windows Server 2016：中 IPAM DNS 管理](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
在 Windows Server 2016 DNS 是您可以使用伺服器管理員或 Windows PowerShell 命令安裝伺服器角色。 如果您安裝新的 Active Directory 森林和網域，DNS 會自動安裝 Active Directory 與全球目錄伺服器的樹系和網域。  
  
Active Directory Domain Services (AD DS) 會使用 DNS 網域控制站的位置機制。 主要 Active Directory 作業執行時，驗證，例如更新，或搜尋時，電腦使用 DNS 尋找 Active Directory 網域控制站。 此外，網域控制站會使用 DNS 不到彼此。  
  
DNS Client 服務包含所有 client 和 server 版本的 Windows 作業系統，在與作業系統安裝時，預設執行。 當您的 DNS 伺服器的 IP 位址設定 TCP/IP 上網時，DNS Client 查詢 DNS 伺服器探索網域控制站，並將電腦的名稱解析為 IP 位址。 例如網路使用者的 Active Directory 帳號 Active Directory domain 登入，DNS Client 服務查詢 Active Directory domain 的網域控制站找出 DNS 伺服器。 當回應查詢 DNS 伺服器，並提供 client 網域控制站的 IP 位址時，client 連絡人的網域控制站和驗證程序就可以開始。  
  
Windows Server 2016 DNS 伺服器，並 DNS Client 服務使用 TCP/IP 通訊協定套件中包含 DNS 通訊協定。 DNS 是部分應用程式層的 TCP/IP 參考型號，如下所示。  
  
![Tcp/ip DNS](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

