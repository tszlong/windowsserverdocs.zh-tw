---
title: 疑難排解 「 主機守護者服務
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7fe01039b47c36d940973fba97d25c401f5af8a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851369"
---
# <a name="troubleshooting-guarded-hosts"></a>疑難排解受防護的主機

> 適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

本主題說明部署和操作您的受防護網狀架構中受防護的 HYPER-V 主機時所遇到的常見問題的解決方法。
如果您不確定您的問題，第一次嘗試執行的性質[受防護網狀架構診斷](guarded-fabric-troubleshoot-diagnostics.md)上您的 HYPER-V 主機，縮小可能會導致。

## <a name="guarded-host-feature"></a>受防護主機的功能

如果您遇到了您的 HYPER-V 主機的問題，先確定**主機守護者 HYPER-V 支援**安裝功能。
沒有此功能，HYPER-V 主機將會遺失一些重要的組態設定和軟體，以便將證明及佈建受防護 Vm。

若要檢查是否已安裝的功能，使用伺服器管理員或提高權限的 PowerShell 視窗中執行下列命令：

```powershell
Get-WindowsFeature HostGuardian
```

如果未安裝此功能，請使用下列 PowerShell 命令安裝：

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>證明失敗

如果主機未通過證明的主機守護者服務，它將無法執行受防護的 Vm。
輸出[Get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914500.aspx)該主機上會顯示您為何該主機通過證明的資訊。

下表說明可能會出現在值**AttestationStatus**欄位以及潛在後續步驟中，如果適用的話。

AttestationStatus         | 說明
--------------------------|------------
已到期                   | 主應用程式之前，通過證明，但它已發行的健康情況憑證已過期。 請確定在主機與 HGS 時間都保持同步。
InsecureHostConfiguration | 主應用程式未通過證明，因為它不符合設定 HGS 的證明原則。 如需詳細資訊，請參閱 AttestationSubStatus 資料表。
NotConfigured             | 主機未設定為使用 HGS 證明與金鑰保護。 它可能已本機模式中的。 如果此主機受防護網狀架構中，請使用[組 HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx)為提供您的 HGS 伺服器 Url。
傳遞                    | 主機通過證明。
TransientError            | 最後證明嘗試失敗，因為網路功能、 服務或其他暫時性錯誤。 重試最後一個作業。
TpmError                  | 主應用程式無法完成其最後一次的證明嘗試由於 TPM 發生錯誤。 如需詳細資訊，請參閱您的 TPM 記錄檔。
UnauthorizedHost          | 主應用程式未通過證明，因為它未獲授權執行受防護的 Vm。 請確定主機屬於安全性群組，信任的 HGS 執行受防護的 Vm。
不明                   | 主機未嘗試尚未與 HGS 證明。


當**AttestationStatus**回報為**InsecureHostConfiguration**，將會填入一個或多個原因**AttestationSubStatus**欄位。
下表說明可能的值，如需 AttestationSubStatus 及如何解決此問題的秘訣。

AttestationSubStatus       | 是什麼意思，該怎麼辦
---------------------------|-------------------------------
BitLocker                  | 主機的 OS 磁碟區不會由 BitLocker 加密。 若要解決這[啟用 BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) OS 磁碟區或[停用 BitLocker 原則上 HGS](guarded-fabric-manage-hgs.md#review-attestation-policies)。
CodeIntegrityPolicy        | 主機未設定為使用程式碼完整性原則，或未使用受信任的 HGS 伺服器的原則。 請確定程式碼完整性原則已設定，尚未重新啟動主機，且原則已向 HGS 伺服器。 如需詳細資訊，請參閱 <<c0> [ 建立和套用程式碼完整性原則](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy)。
DumpsEnabled               | 主機已設定為允許損毀傾印或即時記憶體傾印，這不允許由 HGS 原則。 若要解決此問題，停用主機上的傾印。
DumpEncryption             | 主機已設定為允許損毀傾印或即時記憶體傾印，但不會加密這些傾印。 請停用主機上的傾印或[設定傾印加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
DumpEncryptionKey          | 主應用程式設定為允許並加密傾印，但不是使用 HGS 已知的憑證來加密它們。 若要解決這[更新傾印加密金鑰](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)主機上或[向 HGS 註冊金鑰](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
FullBoot                   | 主應用程式會繼續從睡眠或休眠狀態。 重新啟動主機，以便進行清除、 完整的開機。
HibernationEnabled         | 主機已設定為允許未加密的休眠檔案，這不允許由 HGS 原則的休眠。 停用休眠，然後重新啟動主機，或是[設定傾印加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
HypervisorEnforcedCodeIntegrityPolicy | 主機未設定為使用 hypervisor 強制執行的程式碼完整性原則。 請確認程式碼完整性已啟用、 設定以及由 hypervisor 所強制執行。 請參閱[Device Guard 部署指南](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)如需詳細資訊。
Iommu 所致                      | 為需要 IOMMU 裝置以抵禦直接記憶體存取的攻擊，視需要由 HGS 原則未設定主機的虛擬化型安全性功能。 請確認主機有 iommu 所致，已啟用，且該 Device Guard[設定為需要 DMA 保護](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard)時啟動 VBS。
PagefileEncryption         | 在主機上未啟用分頁檔加密。 若要解決此問題，請執行`fsutil behavior set encryptpagingfile 1`啟用分頁檔加密。 如需詳細資訊，請參閱 < [fsutil 行為](https://technet.microsoft.com/library/cc785435.aspx)。
SecureBoot                 | 安全開機未啟用這台主機上或不使用 Microsoft 安全開機範本。 [啟用安全開機](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot)了 Microsoft 安全開機範本，以解決此問題。
SecureBootSettings         | 此主機上的 TPM 基準不符合任何這些受信任的 HGS。 這可能是藉由安裝新的硬體或軟體變更您 UEFI 啟動授權單位、 DBX 變數、 偵錯旗標或自訂的 「 安全開機 」 原則時。 如果您信任目前的硬體、 韌體及軟體組態，此機器，您可以[擷取新的 TPM 基準](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware)並[向 HGS 註冊](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
TcgLogVerification         | 無法取得 TCG 記錄檔 （TPM 基準），或驗證。 這表示主機的韌體、 TPM 或其他硬體元件的問題。 如果您的主機已嘗試啟動 Windows 之前的 PXE 開機，過期的網路開機程式 (NBP) 也可能導致此錯誤。 請確定啟用 PXE 開機時，所有的 Nbp 是最新狀態。
VirtualSecureMode          | 虛擬化基礎的安全性功能不會在主機上執行。 確定 VBS 已啟用，而且您的系統符合已設定[平台安全性功](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)。 請參閱[Device Guard 文件](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)的 VBS 需求的詳細資訊。
