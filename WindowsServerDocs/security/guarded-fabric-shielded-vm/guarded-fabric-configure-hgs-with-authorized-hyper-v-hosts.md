---
title: 部署受防護主機
ms.prod: windows-server
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0f678172d397ff61fd336b7c844d43f77bea7fad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856831"
---
# <a name="deploy-guarded-hosts"></a>部署受防護主機

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

本節中的主題說明網狀架構系統管理員設定 Hyper-v 主機以與主機守護者服務（HGS）搭配使用所需的步驟。 在您可以開始這些步驟之前，[必須先設定 HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)叢集中至少一個節點。

**針對 TPM 信任證明**：
1. 設定網狀[架構 dns](guarded-fabric-configuring-fabric-dns.md)：告訴您如何將 DNS 轉寄站從網狀架構網域設定為 HGS 網域。
2. [抓取 HGS 所需的資訊](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)：告訴您如何捕捉 TPM 識別碼（也稱為平臺識別碼）、建立程式碼完整性原則，以及建立 TPM 基準。 然後，您會將此資訊提供給 HGS 系統管理員，以設定證明。
3. [確認受防護主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**對於主機金鑰證明**：
1. [建立主機金鑰](guarded-fabric-create-host-key.md#create-a-host-key)：告訴您如何設定從網狀架構網域到 HGS 網域的 DNS 轉寄站。
2. [將主機金鑰新增至證明服務](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service)：告訴您如何在網狀架構網域中設定 Active Directory 安全性群組，將受防護主機新增為該群組的成員，並將該群組識別碼提供給 HGS 系統管理員。 
3. [確認受防護主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**針對系統管理員信任的證明**：
1. 設定網狀[架構 dns](guarded-fabric-configuring-fabric-dns.md)：告訴您如何將 DNS 轉寄站從網狀架構網域設定為 HGS 網域。
2. [建立安全性群組](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)：告訴您如何在網狀架構網域中設定 Active Directory 安全性群組、將受防護主機新增為該群組的成員，並將該群組識別碼提供給 HGS 系統管理員。 
3. [確認受防護主機可以證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>另請參閱

- [受保護網狀架構和受防護 Vm 的部署工作](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
