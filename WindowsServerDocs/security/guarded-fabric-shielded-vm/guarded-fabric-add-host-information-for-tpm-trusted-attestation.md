---
title: 新增適用于 TPM 的主機資訊-受信任的證明
description: 新增 TPM 信任證明之主機資訊的相關資訊。
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 06/21/2019
ms.openlocfilehash: 2f4f684b0c18c19cdbdf09e672c83e51f426ca04
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388849"
---
# <a name="add-host-information-for-tpm-trusted-attestation"></a>新增適用于 TPM 的主機資訊-受信任的證明

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

若是 TPM 模式，網狀架構系統管理員會捕捉三種主機資訊，每個資訊都必須新增至 HGS 設定：

- 每一部 Hyper-v 主機 (EKpub) 的 TPM 識別碼
- 程式碼完整性原則，允許清單 Hyper-v 主機允許的二進位檔
- TPM 基準 (開機測量) ，代表在相同硬體類別上執行的一組 Hyper-v 主機。

在網狀架構系統管理員捕獲資訊之後，請將它新增至 HGS 設定，如下列程式所述。

1. 取得包含 EKpub 資訊的 XML 檔案，並將其複製到 HGS 伺服器。 每一部主機都會有一個 XML 檔案。 然後，在 HGS 伺服器上提高許可權的 Windows PowerShell 主控台中，執行下列命令。 針對每個 XML 檔案重複此命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果您在新增有關未受信任簽署金鑰憑證的 TPM 識別碼時發生錯誤 (EKCert) ，請確定 [已將受信任的 TPM 根憑證新增](guarded-fabric-install-trusted-tpm-root-certificates.md) 至 HGS 節點。
    > 此外，有些 TPM 廠商不會使用 EKCerts。
    > 您可以藉由在編輯器（例如 [記事本]）中開啟 XML 檔案，並檢查指出找不到任何 EKCert 的錯誤訊息，來檢查是否遺漏 EKCert。
    > 如果是這種情況，而且您信任電腦中的 TPM 是真實的，您可以使用旗標 `-Force` 來覆寫此安全性檢查，並將主機識別碼新增至 HGS。

2. 取得網狀架構系統管理員為主機建立的程式碼完整性原則，以二進位格式 (\* . p7b) 。 將它複製到 HGS 伺服器。 然後，執行下列命令。

    針對 `<PolicyName>` ，指定 CI 原則的名稱，以描述其所套用的主機類型。 最佳做法是將它命名為您電腦的製作/型號和在其上執行的任何特殊軟體設定。<br>`<Path>`在中，指定程式碼完整性原則的路徑和檔案名。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

    > [!NOTE]
    > 如果您使用的是已簽署的程式碼完整性原則，請向 HGS 註冊相同原則的不帶正負號複本。
    > 程式碼完整性原則的簽章是用來控制原則的更新，但不會測量到主機 TPM，因此不能由 HGS 證明。

3. 取得網狀架構系統管理員從參照主機捕獲的 TCG 記錄檔。 將檔案複製到 HGS 伺服器。 然後，執行下列命令。 一般來說，您會將原則命名在它所代表的硬體類別之後 (例如「製造商型號修訂」 ) 。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

這會完成為 TPM 模式設定 HGS 叢集的程式。 網狀架構系統管理員可能需要您從 HGS 提供兩個 Url，才能完成主機的設定。 若要取得這些 Url，請在 HGS 伺服器上執行 [HgsServer](/powershell/module/hgsserver/get-hgsserver?view=win10-ps)。

## <a name="next-step"></a>後續步驟

> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)