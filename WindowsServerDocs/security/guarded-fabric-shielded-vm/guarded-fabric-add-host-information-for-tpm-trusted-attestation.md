---
title: 新增 TPM 信任證明的主機資訊
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: f1c25cc88c577ccb1bc0e8cc690114471e86b6ba
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203398"
---
# <a name="add-host-information-for-tpm-trusted-attestation"></a>新增 TPM 信任證明的主機資訊

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

針對 TPM 模式，網狀架構系統管理員會捕獲三種主機資訊，每個都必須新增至 HGS 設定：

- 每部 Hyper-v 主機的 TPM 識別碼（EKpub）
- 程式碼完整性原則，這是 Hyper-v 主機允許之二進位檔的白名單
- TPM 基準（開機測量），代表在相同硬體類別上執行的一組 Hyper-v 主機

Af er：網狀架構系統管理員會捕捉資訊，將其新增至 HGS 設定，如下列程式所述。

1. 取得包含 EKpub 資訊的 XML 檔案，並將其複製到 HGS 伺服器。 每一部主機會有一個 XML 檔案。 然後，在 HGS 伺服器上提高許可權的 Windows PowerShell 主控台中，執行下列命令。 針對每個 XML 檔案重複此命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
       ```

    > [!NOTE]
    > If you encounter an error when adding a TPM identifier regarding an untrusted Endorsement Key Certificate (EKCert), ensure that the [trusted TPM root certificates have been added](guarded-fabric-install-trusted-tpm-root-certificates.md) to the HGS node.
    > Additionally, some TPM vendors do not use EKCerts.
    > You can check if an EKCert is missing by opening the XML file in an editor such as Notepad and checking for an error message indicating no EKCert was found.
    > If this is the case, and you trust that the TPM in your machine is authentic, you can use the `-Force` flag to override this safety check and add the host identifier to HGS.

2. Obtain the code integrity policy that the fabric administrator created for the hosts, in binary format (\*.p7b). Copy it to an HGS server. Then run the following command.

    For `<PolicyName>`, specify a name for the CI polic" that describes the type of host it appl"es to. A be"t practice is to name it after the"make/model of your machine and any special software configuration running on it.<br>For `<Path>`, specify the path and filename of the code integrity policy.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
       ```

    > [!NOTE]
    > If you're using a signed code integrity policy, register an unsigned copy of the same policy with HGS.
    > The signature on code integrity policies is used to control updates to the policy, but is not measured into the host TPM and therefore cannot be attested to by HGS.

3.    Obtain the TCGlog file that the fabric administrator captured from a reference host. Copy the file to an HGS server. Then run the following command. Typically, you will name the policy after the class of hardware it represents (for example, "Manufacturer Model Revision").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

這會完成為 TPM 模式設定 HGS 叢集的程式。 網狀架構系統管理員可能會要求您從 HGS 提供兩個 Url，然後才能完成主機的設定。 若要取得這些 Url，請在 HGS 伺服器上執行[HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps)。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)
