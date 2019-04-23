---
title: 部署受防護主機
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3b20a7eb2b5097d8ddb7381fd0304581ca4e6722
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845349"
---
# <a name="deploy-guarded-hosts"></a>部署受防護主機

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

在本節中的主題描述設定才能與主機守護者服務 (HGS) 的 HYPER-V 主機網狀架構系統管理員所需的步驟。 您可以開始這些步驟中，至少一個節點之前[必須設定 HGS 叢集](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。

**TPM 信任證明**:
1. [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns.md):說明如何設定 HGS 網域從網狀架構網域的 DNS 轉送工具。
2. [擷取資訊所需的 HGS](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md):說明如何擷取 TPM 識別項 （也稱為平台識別項），建立程式碼完整性原則，並建立 TPM 基準。 然後您會將此資訊提供給 HGS 系統管理員設定證明。
3. [確認可以證明的受防護的主機](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**針對主機金鑰證明**:
1. [建立主索引鍵](guarded-fabric-create-host-key.md#create-a-host-key):說明如何設定 HGS 網域從網狀架構網域的 DNS 轉送工具。
2. [加入 「 證明 」 服務的主機鍵](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service):說明如何在網狀架構的網域中，設定一個 Active Directory 安全性群組隸屬該群組中，新增受守護的主機，並提供該群組識別碼給 HGS 系統管理員。 
3. [確認可以證明的受防護的主機](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**針對系統管理信任證明**:
1. [設定網狀架構 DNS](guarded-fabric-configuring-fabric-dns.md):說明如何設定 HGS 網域從網狀架構網域的 DNS 轉送工具。
2. [建立安全性群組](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md):說明如何在網狀架構的網域中，設定一個 Active Directory 安全性群組隸屬該群組中，新增受守護的主機，並提供該群組識別碼給 HGS 系統管理員。 
3. [確認可以證明的受防護的主機](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>另請參閱

- [部署工作，針對受防護網狀架構與受防護的 Vm](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
