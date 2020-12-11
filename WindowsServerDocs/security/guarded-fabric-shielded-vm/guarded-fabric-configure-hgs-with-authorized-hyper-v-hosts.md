---
description: 深入瞭解：部署受防護的主機
title: 部署受保護的主機
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: fb3d189ce0817d52a536af8f25fd9b78fda719e5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049876"
---
# <a name="deploy-guarded-hosts"></a>部署受保護的主機

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本節中的主題描述網狀架構系統管理員設定 Hyper-v 主機使用主機守護者服務 (HGS) 所需的步驟。 在您可以開始這些步驟之前， [必須先設定 HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)叢集中的一個節點。

**針對受 TPM 信任的證明**：
1. 設定網狀[架構 dns](guarded-fabric-configuring-fabric-dns.md)：告知如何設定從網狀架構網域到 HGS 網域的 dns 轉寄站。
2. [抓取 HGS 所需的資訊](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)：告訴您如何捕獲 tpm 識別碼 (也稱為平臺識別碼) 、建立程式碼完整性原則，以及建立 tpm 基準。 然後，您會將此資訊提供給 HGS 系統管理員，以設定證明。
3. [確認受防護主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**針對主機金鑰證明**：
1. [建立主機金鑰](guarded-fabric-create-host-key.md#create-a-host-key)：告知如何設定從網狀架構網域到 HGS 網域的 DNS 轉寄站。
2. [將主機金鑰新增至證明服務](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service)：告知如何設定網狀架構網域中的 Active Directory 安全性群組、新增受防護主機作為該群組的成員，並將該群組識別碼提供給 HGS 系統管理員。
3. [確認受防護主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**針對系統管理員信任的證明**：
1. 設定網狀[架構 dns](guarded-fabric-configuring-fabric-dns.md)：告知如何設定從網狀架構網域到 HGS 網域的 dns 轉寄站。
2. [建立安全性群組](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)：告知如何設定網狀架構網域中的 Active Directory 安全性群組、新增受防護主機作為該群組的成員，以及將該群組識別碼提供給 HGS 系統管理員。
3. [確認受防護主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="additional-references"></a>其他參考資料

- [受防護網狀架構與受防護 Vm 的部署工作](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
