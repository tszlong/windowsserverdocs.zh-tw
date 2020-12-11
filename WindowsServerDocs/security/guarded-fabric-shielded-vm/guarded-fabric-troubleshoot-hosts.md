---
description: 深入瞭解：對受防護主機進行疑難排解
title: 針對受防護主機進行疑難排解
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 4a9d38e2e702610bb1a6905cdc198d3b4e708d6e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049246"
---
# <a name="troubleshooting-guarded-hosts"></a>針對受防護主機進行疑難排解

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本主題說明在受防護網狀架構中部署或操作受防護的 Hyper-v 主機時所遇到的常見問題解決方案。
如果您不確定問題的本質，請先試著在您的 Hyper-v 主機上執行受防護網狀 [架構診斷](guarded-fabric-troubleshoot-diagnostics.md) ，以縮小可能的原因。

## <a name="guarded-host-feature"></a>受防護主機功能

如果您在 Hyper-v 主機上遇到問題，請先確定已安裝主機守護 **者 Hyper-v 支援** 功能。
如果沒有這項功能，Hyper-v 主機將會遺失一些重要的設定和軟體，讓它能夠通過證明並布建受防護的 Vm。

若要檢查是否已安裝此功能，請使用伺服器管理員，或在提高許可權的 PowerShell 視窗中執行下列命令：

```powershell
Get-WindowsFeature HostGuardian
```

如果尚未安裝此功能，請使用下列 PowerShell 命令來加以安裝：

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>證明失敗

如果主機未通過主機守護者服務的證明，它將無法執行受防護的 Vm。
該主機上的 [>get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914500.aspx) 輸出將會顯示為何該主機無法證明的相關資訊。

下表說明可能出現在 [ **AttestationStatus** ] 欄位中的值，以及可能的後續步驟（如果有的話）。

AttestationStatus         | 說明
--------------------------|------------
已過期                   | 主機先前已通過證明，但簽發給該主機的健康情況憑證已過期。 請確認主機和 HGS 時間為同步。
InsecureHostConfiguration | 主機未通過證明，因為它不符合 HGS 上設定的證明原則。 如需詳細資訊，請參閱 AttestationSubStatus 資料表。
NotConfigured             | 主機未設定為使用 HGS 進行證明和金鑰保護。 它是針對原生模式而設定。 如果此主機位於受防護網狀架構中，請使用 [>get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914494.aspx) 來提供 HGS 伺服器的 url。
已通過                    | 主機通過證明。
TransientError            | 由於網路、服務或其他暫時性錯誤，導致上次證明嘗試失敗。 請重試您的上次操作。
TpmError                  | 因為您的 TPM 發生錯誤，所以主機無法完成其上次證明嘗試。 如需詳細資訊，請參閱您的 TPM 記錄。
UnauthorizedHost          | 主機未通過證明，因為未獲授權執行受防護的 Vm。 確定主機屬於 HGS 所信任的安全性群組，以執行受防護的 Vm。
Unknown                   | 主機尚未嘗試使用 HGS 進行證明。

當 **AttestationStatus** 回報為 **InsecureHostConfiguration** 時，會在 **AttestationSubStatus** 欄位中填入一個或多個原因。
下表說明 AttestationSubStatus 的可能值，以及如何解決問題的秘訣。

AttestationSubStatus       | 代表的意義及應採取的動作
---------------------------|-------------------------------
BitLocker                  | 主機的 OS 磁片區未以 BitLocker 加密。 若要解決此問題，請在 OS 磁片區上 [啟用 bitlocker](/windows/security/information-protection/bitlocker/bitlocker-basic-deployment) ，或 [停用 HGS 上的 bitlocker 原則](guarded-fabric-manage-hgs.md#review-attestation-policies)。
CodeIntegrityPolicy        | 主機未設定為使用程式碼完整性原則，或未使用 HGS 伺服器所信任的原則。 確定已設定程式碼完整性原則、主機已重新開機，且該原則已向 HGS 伺服器註冊。 如需詳細資訊，請參閱 [建立及套用程式碼完整性原則](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy)。
DumpsEnabled               | 主機已設定為允許損毀傾印或即時儲存體傾印，但您的 HGS 原則不允許這種情況。 若要解決此問題，請停用主機上的傾印。
DumpEncryption             | 主機設定為允許損毀傾印或即時儲存體傾印，但不會加密這些傾印。 請停用主機上的傾印或設定傾印 [加密](../../virtualization/hyper-v/manage/about-dump-encryption.md)。
DumpEncryptionKey          | 主機設定為允許和加密傾印，但未使用 HGS 已知的憑證來加密它們。 若要解決此問題，請更新主機上的傾印 [加密金鑰](../../virtualization/hyper-v/manage/about-dump-encryption.md) ，或向 [HGS 註冊金鑰](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
FullBoot                   | 主機從睡眠狀態或休眠狀態繼續。 重新開機主機，以允許乾淨、完整的開機。
HibernationEnabled         | 主機設定為允許休眠，而不需要加密休眠檔案，但您的 HGS 原則不允許。 停用休眠並重新啟動主機，或設定傾印 [加密](../../virtualization/hyper-v/manage/about-dump-encryption.md)。
HypervisorEnforcedCodeIntegrityPolicy | 主機未設定為使用虛擬程式強制執行的程式碼完整性原則。 確認程式碼完整性已啟用、設定，並由虛擬程式強制執行。 如需詳細資訊，請參閱 [Device Guard 部署指南](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) 。
Iommu                      | 主機的虛擬化安全性功能未設定為需要 IOMMU 裝置，以保護您的 HGS 原則所需的直接記憶體存取攻擊。 確認主機具有 IOMMU、是否已啟用，而且已將裝置防護設定為在啟動 VBS 時 [需要 DMA 保護](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) 。
PagefileEncryption         | 主機上未啟用分頁檔加密。 若要解決此問題，請執行 `fsutil behavior set encryptpagingfile 1` 以啟用分頁檔加密。 如需詳細資訊，請參閱 [fsutil 行為](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc785435(v=ws.11))。
SecureBoot                 | 此主機上未啟用安全開機，或未使用 Microsoft 安全開機範本。 使用 Microsoft 安全開機範本[啟用安全開機](/windows-hardware/manufacture/desktop/disabling-secure-boot#enable_secure_boot)，以解決此問題。
SecureBootSettings         | 此主機上的 TPM 基準不符合 HGS 所信任的任何一個。 當您的 UEFI 啟動授權單位、.DBX 變數、偵測旗標或自訂安全開機原則藉由安裝新的硬體或軟體而變更時，就會發生這種情況。 如果您信任這部電腦目前的硬體、固件和軟體設定，則可以 [捕獲新的 TPM 基準](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) 並 [向 HGS 註冊](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
TcgLogVerification         | 無法取得或驗證 TCG 記錄 (TPM 基準) 。 這可能表示主機的固件、TPM 或其他硬體元件發生問題。 如果您的主機已設定為在開機 Windows 之前嘗試 PXE 開機，則過期的 Net Boot 程式 (NBP) 也會造成此錯誤。 當啟用 PXE 開機時，請確定所有 Nbp 都是最新的。
VirtualSecureMode          | 主機上未執行以虛擬化為基礎的安全性功能。 確定已啟用 VBS，且您的系統符合設定的 [平臺安全性功能](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)。 如需 VBS 需求的詳細資訊，請參閱 [Device Guard 檔](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide) 。

## <a name="modern-tls"></a>新式 TLS

如果您已部署群組原則，或未設定 Hyper-v 主機以防止使用 TLS 1.0，則您可能會在嘗試啟動受防護的 VM 時遇到「主機守護者服務用戶端無法代表呼叫進程解除金鑰保護裝置」錯誤。
這是因為 .NET 4.6 的預設行為，而在與 HGS 伺服器協商支援的 TLS 版本時，不會考慮系統預設的 TLS 版本。

若要解決此行為，請執行下列兩個命令來設定 .NET，以針對所有 .NET 應用程式使用系統預設的 TLS 版本。

```cmd
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:64
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:32
```

> [!WARNING]
> 系統預設的 TLS 版本設定將會影響您電腦上的所有 .NET 應用程式。 將登錄機碼部署到生產電腦之前，請務必先在隔離的環境中進行測試。

如需 .NET 4.6 和 TLS 1.0 的詳細資訊，請參閱 [解決 TLS 1.0 問題第二版](/security/solving-tls1-problem)。
