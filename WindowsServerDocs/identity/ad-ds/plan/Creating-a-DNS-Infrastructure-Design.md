---
description: 深入瞭解：建立 DNS 基礎結構設計
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: 建立 DNS 基礎結構設計
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: b9dc73b44fcfc104f8e96a7e693c3867d8072cf4
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049996"
---
# <a name="creating-a-dns-infrastructure-design"></a>建立 DNS 基礎結構設計

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立 Active Directory 樹系和網域設計之後，您必須將網域名稱系統設計 (DNS) 基礎結構，以支援您的 Active Directory 邏輯結構。 DNS 可讓使用者使用容易記住的易記名稱，以連接至 IP 網路上的電腦和其他資源。 Windows Server 2008 中的 Active Directory Domain Services (AD DS) 需要 DNS。

設計 DNS 以支援 AD DS 的程式會根據您的組織是否已有現有的 DNS 伺服器服務，或您要部署新的 DNS 伺服器服務而有所不同：

- 如果您已經有現有的 DNS 基礎結構，則必須將 Active Directory 命名空間整合到該環境中。 如需詳細資訊，請參閱 [將 AD DS 整合至現有的 DNS 基礎結構](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)。
- 若您沒有 DNS 基礎結構，則必須指定並部署新的 DNS 基礎結構以支援 AD DS。 如需詳細資訊，請參閱 [部署網域名稱系統 (DNS) ](/previous-versions/windows/it-pro/windows-server-2003/cc780661(v=ws.10))。

如果您的組織有現有的 DNS 基礎結構，您必須確定您瞭解 DNS 基礎結構與 Active Directory 命名空間的互動方式。 若要協助您記錄現有的 DNS 基礎結構設計，請從 [適用于 Windows Server 2003 部署套件的工作輔助](https://microsoft.com/download/details.aspx?id=9608) 程式下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟 [DNS 清查] ( # A1) 。

> [!NOTE]
> 除了 IP 第4版 (IPv4) 位址，Windows Server 也支援 IP 第6版 (IPv6) 位址。 如需協助您在記錄目前 DNS 結構的遞迴名稱解析方法時列出 IPv6 位址的工作表，請參閱 [附錄 a： DNS 清查](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)。

在您設計 DNS 基礎結構以支援 AD DS 之前，請先閱讀 DNS 階層、DNS 名稱解析程式，以及 DNS 支援 AD DS 的相關資訊。 如需有關 DNS 階層和名稱解析程式的詳細資訊，請參閱 [Dns 技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc779926(v=ws.10))。 如需 DNS 如何支援 AD DS 的詳細資訊，請參閱 [Active Directory 技術參考的 Dns 支援](/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10))。

## <a name="in-this-section"></a>本節內容

- [檢閱 DNS 概念](../../ad-ds/plan/Reviewing-DNS-Concepts.md)
- [DNS 與 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)
- [指派 AD DS 擁有者角色的 DNS](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)
- [將 AD DS 整合至現有 DNS 基礎結構](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)
