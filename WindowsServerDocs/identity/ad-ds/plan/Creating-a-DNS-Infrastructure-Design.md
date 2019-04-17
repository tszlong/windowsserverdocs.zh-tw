---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: "建立 DNS 基礎結構設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6e5093b05fd81a693cec87ddb00d39e70483df23
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-dns-infrastructure-design"></a>建立 DNS 基礎結構設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立您的 Active Directory 樹系和網域設計之後，您必須支援您 Active Directory 邏輯結構網域名稱系統」(DNS) 基礎結構設計。 DNS 可讓使用者使用易記連接到電腦和其他資源 IP 網路上的易記名稱。 Windows Server 2008 的 active Directory Domain Services (AD DS) 需要 DNS。  
  
設計支援 AD DS DNS 的程序會根據您的組織已經是否有現有的 DNS 伺服器服務而有所不同，或您的部署新的 DNS 伺服器服務：  
  
-   如果您已經現有的 DNS 基礎結構，您必須整合的環境中的 Active Directory 命名空間。 如需詳細資訊，請查看[現有 DNS 結構拷貝 AD DS](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)。  
  
-   如果您尚未 DNS 基礎結構位置中，您必須設計和部署新支援 AD DS DNS 基礎結構。 如需詳細資訊，請查看部署網域名稱系統」(DNS) ([https://go.microsoft.com/fwlink/?LinkId=93656](https://go.microsoft.com/fwlink/?LinkId=93656))。  
  
如果您的組織已經現有的 DNS 基礎結構，您必須確定您了解如何使用 Active Directory 命名空間互動 DNS 基礎結構。 為了幫助您記載您現有的 DNS 基礎結構設計試算表，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 從工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) 和開放」DNS 庫存「(DSSLOGI_8.doc).  
  
> [!NOTE]  
> 除了版本 4 的 IP 位址 (IPv4)、Windows Server 2008 也支援 IP 版本 6 (IPv6) 的位址。 適用於試算表中列出的 IPv6 位址時記載您目前的 DNS 結構遞迴名稱解析方法協助您，請查看[附錄 a: DNS 庫存](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)。  
  
設計 DNS 基礎結構支援 AD DS 之前，可以了解 DNS 階層、DNS 名稱解析程序，以及如何 DNS 支援 AD DS 很有幫助。 如需 DNS 階層和名稱解析程序，查看 [DNS 技術參考 ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145))。 如需有關如何 DNS 支援 AD DS，查看 [DNS 支援的 Active Directory 技術參考 ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147))。  
  
## <a name="in-this-section"></a>在本區段中  
  
-   [審查 DNS 概念](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
  
-   [DNS 和 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
  
-   [指定 DNS AD DS 擁有者角色](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
  
-   [整合現有的 DNS 基礎結構 AD DS](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
  


