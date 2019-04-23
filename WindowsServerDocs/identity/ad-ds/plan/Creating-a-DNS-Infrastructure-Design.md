---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: 建立 DNS 基礎結構設計
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 080c36f8410be4d6b1933c74730e2b55ce8d0a0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856129"
---
# <a name="creating-a-dns-infrastructure-design"></a>建立 DNS 基礎結構設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立您的 Active Directory 樹系和網域設計之後，您必須設計的網域名稱系統 (DNS) 基礎結構，以支援您的 Active Directory 邏輯結構。 DNS 可讓使用者使用都很容易就能連線到電腦和 IP 網路上的其他資源，請記得的易記名稱。 在 Windows Server 2008 的 active Directory 網域服務 (AD DS) 需要 DNS。  
  
設計 DNS 以支援 AD DS 的程序會根據您組織是否已有現有的 DNS 伺服器服務而有所不同，或您要部署新的 DNS 伺服器服務：  
  
- 如果您已經有現有的 DNS 基礎結構，您必須整合到該環境中的 Active Directory 命名空間。 如需詳細資訊，請參閱 <<c0> [ 將現有 DNS 基礎結構的整合 AD DS](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)。  
- 如果在位置中沒有 DNS 基礎結構，您必須設計和部署新的 DNS 基礎結構，以支援 AD DS。 如需詳細資訊，請參閱 <<c0> [ 部署網域名稱系統 (DNS)](https://go.microsoft.com/fwlink/?LinkId=93656)。  
  
如果您的組織有現有的 DNS 基礎結構，您必須確定您了解您的 DNS 基礎結構與 Active Directory 命名空間的互動方式。 為協助您記載您現有的 DNS 基礎結構設計的工作表，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 從[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)和開啟 「 DNS 清查 」 (DSSLOGI_8.doc)。  
  
> [!NOTE]  
> 除了 IP 第 4 版 (IPv4) 位址，Windows Server 也支援 IP 版本 6 (IPv6) 位址。 可協助您列出的 IPv6 位址時記錄目前的 DNS 結構的遞迴名稱解析方法為工作表，請參閱[附錄 a:DNS 清查](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)。
  
在設計您的 DNS 基礎結構，以支援 AD DS 之前，很有幫助，若要了解 DNS 階層，DNS 名稱解析程序，以及如何 DNS 支援 AD DS。 如需有關 DNS 階層和名稱解析程序的詳細資訊，請參閱 DNS 技術參考資料 ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145))。 DNS 如何支援 AD DS 的詳細資訊，請參閱 DNS 支援，如需 Active Directory 技術參考 ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147))。  
  
## <a name="in-this-section"></a>本節內容  

- [檢閱 DNS 概念](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS 和 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [AD DS 擁有者角色指派的 DNS](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [將 AD DS 整合到現有的 DNS 基礎結構](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
