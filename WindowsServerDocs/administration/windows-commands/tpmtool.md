---
title: tpmtool
description: Tpmtool 的參考文章，可取得信賴平臺模組的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: 843361a9b3844ecb29e2f9ac723d22e3fc14730f
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955020"
---
# <a name="tpmtool"></a>tpmtool

這個公用程式可以用來取得[信賴平臺模組（TPM）](/windows/security/information-protection/tpm/trusted-platform-module-overview)的相關資訊。

>[!IMPORTANT]
>預先發行的產品在正式發行前可能會大幅度修改，本文提供有關該產品的一些資訊。 Microsoft 對於此處提供的資訊不做任何明示或暗示保證。

如需如何使用此命令的範例，請參閱[範例](#tpmtool_examples)。

## <a name="syntax"></a>語法

```
tpmtool /parameter [<arguments>]
```
### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|getdeviceinformation|顯示 TPM 的基本資訊。 資訊旗標值的意義可以在[這裡](/windows/desktop/secprov/win32-tpm-isreadyinformation#parameters)找到。|
|gatherlogs [輸出目錄路徑]|會收集 TPM 記錄，並將它們放在指定的目錄中。 如果該目錄不存在，則會建立它。 根據預設，它們會放在目前的目錄中。 可能產生的檔案包括： </br>-TpmEvents .evtx</br>-TpmInformation.txt</br>-SRTMBoot .dat</br>-SRTMResume .dat</br>-DRTMBoot .dat</br>-DRTMResume .dat</br>|
|drivertracing [啟動/停止]|啟動/停止收集 TPM 驅動程式追蹤。 追蹤記錄 TPMTRACE 會產生並放在目前的目錄中。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a><a name=tpmtool_examples></a>範例

若要顯示 TPM 的基本資訊，請輸入：
```
tpmtool getdeviceinformation
```
若要收集 TPM 記錄檔，並將它們放在目前的目錄中，請輸入：
```
tpmtool gatherlogs
```
若要收集 TPM 記錄並將它們放入 `C:\Users\Public` ，請輸入：
```
tpmtool gatherlogs C:\Users\Public
```
若要收集 TPM 驅動程式追蹤，請輸入：
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```

## <a name="decoding-error-codes"></a>解碼錯誤碼

TPM 特定的錯誤碼記載于[此處](/windows/desktop/com/com-error-codes-6)。
