---
title: 關於傾印加密
description: 描述如何加密傾印檔案，以及疑難排解加密。
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: e0ca8829aa8f93e7543b4183539beade3b4aeb47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812389"
---
# <a name="about-dump-encryption"></a>關於傾印加密
傾印加密可用來加密損毀傾印和即時系統產生的傾印。 傾印會加密使用對稱式加密金鑰所產生的每個傾印。 然後使用主應用程式 （損毀傾印加密金鑰保護裝置） 的受信任的系統管理員指定的公開金鑰來加密此金鑰本身。 這可確保只有人具有對應的私密金鑰可以解密，並因此存取傾印的內容。 這項功能會利用受防護網狀架構中。
注意：如果您設定傾印加密，也停用 Windows 錯誤報告。 WER 無法讀取加密的損毀傾印。

# <a name="configuring-dump-encryption"></a>設定傾印加密
## <a name="manual-configuration"></a>手動設定
若要開啟傾印加密使用登錄，設定下的下列登錄值 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| 值名稱 | 類型 | 值 |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 若要啟用傾印加密，0 表示停用傾印加密 1 |
| EncryptionCertificates\Certificate.1::PublicKey | Binary | 公用金鑰 （RSA，2048 位元），其應該用於加密傾印。 此值必須格式化為[BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx)。 |
| EncryptionCertificates\Certificate.1::Thumbprint | 字串 | 以解密損毀傾印時允許自動查閱的本機憑證存放區中的私用金鑰的憑證指紋。 |


## <a name="configuration-using-script"></a>使用指令碼組態
若要簡化設定，[的範例指令碼](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption)啟用根據憑證的公開金鑰傾印加密功能。

1. 在受信任的環境中：建立以 2048 位元 RSA 金鑰的憑證，並且匯出公開憑證
2. 在 目標主機：匯入至本機憑證存放區的公開憑證
3. 執行範例設定指令碼 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>解密加密的傾印
若要解密現有已加密的傾印檔案，您需要下載並安裝偵錯工具的 Windows。 此工具組包含 KernelDumpDecrypt.exe 可用來解密加密的傾印檔案。
包括私密金鑰的憑證是否存在於目前的使用者憑證存放區，可以藉由呼叫解密傾印檔案

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
在解密後 WinDbg 等工具可以開啟已解密的傾印檔案。

# <a name="troubleshooting-dump-encryption"></a>針對傾印加密進行疑難排解
如果在系統上啟用傾印加密，但會產生任何傾印，請檢查系統`System`事件記錄檔`Kernel-IO`1207年事件。 無法初始化傾印加密，就會建立此事件和傾印會停用。

| 詳細的錯誤訊息 | 解決步驟 |
| ---------------------- | ----------------- |
| 公開索引鍵遺漏或指紋登錄類別 | 檢查這兩個登錄值是否存在於預期的位置 |
| 不正確的公開金鑰 | 請確定儲存 PublicKey 登錄值中的公用金鑰會儲存為[BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx)。 |
| 不支援的公開金鑰大小 | 目前，支援只有 2048 位元 RSA 金鑰。 設定索引鍵符合此需求 |

也請檢查該值`GuardedHost`下`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled`設為 0 以外的值。 這完全停用損毀傾印。 如果發生這種情況，請將它設定為 0。
