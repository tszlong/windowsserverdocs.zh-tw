---
title: tpmtool
description: Tpmtool 的 Windows 命令主題-取得可信賴平臺模組的相關資訊。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: 3967136bc64d1e06425a019466dea15ddce3a563
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385723"
---
# <a name="tpmtool"></a>tpmtool

這個公用程式可以用來取得[信賴平臺模組（TPM）](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview)的相關資訊。

>[!IMPORTANT]
>預先發行的產品在正式發行前可能會大幅度修改，本文提供有關該產品的一些資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

如需如何使用此命令的範例，請參閱[範例](#tpmtool_examples)。

## <a name="syntax"></a>語法

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|getdeviceinformation|顯示 TPM 的基本資訊。 資訊旗標值的意義可以在[這裡](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters)找到。|
|gatherlogs [輸出目錄路徑]|會收集 TPM 記錄，並將它們放在指定的目錄中。 如果該目錄不存在，則會建立它。 根據預設，它們會放在目前的目錄中。 可能產生的檔案包括： </br>-TpmEvents .evtx</br>-TpmInformation .txt</br>-SRTMBoot .dat</br>-SRTMResume .dat</br>-DRTMBoot .dat</br>-DRTMResume .dat</br>|
|drivertracing [啟動/停止]|啟動/停止收集 TPM 驅動程式追蹤。 追蹤記錄 TPMTRACE 會產生並放在目前的目錄中。|
|parsetcglogs [-validate （-v）]|顯示已剖析的 TCG 記錄檔（也稱為 Windows 開機設定記錄檔（WBCL））。 您可以在[TCG 網站](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/)的**事件描述**下找到最新的事件描述。 如果設定了 `-validate` 旗標，會驗證 TPM 上的平臺設定暫存器（PCR）值是否符合記錄檔中的值。|
|/?|在命令提示字元顯示說明。|

## <a name="tpmtool_examples"></a>典型

若要顯示 TPM 的基本資訊，請輸入：
```
tpmtool getdeviceinformation
```
若要收集 TPM 記錄檔，並將它們放在目前的目錄中，請輸入：
```
tpmtool gatherlogs
```
若要收集 TPM 記錄檔，並將它們放在 `C:\Users\Public`，請輸入：
```
tpmtool gatherlogs C:\Users\Public
```
若要收集 TPM 驅動程式追蹤，請輸入：
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
若要剖析 TCG 記錄檔：
```
tpmtool parsetcglogs
```
剖析 TCG 記錄檔並驗證 PCRs：
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>解碼錯誤碼

TPM 特定的錯誤碼記載于[此處](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6)。
