---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: Fsutil repair
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6de102daed2bedfeb41da16e06b7db483af70da0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844191"
---
# <a name="fsutil-repair"></a>Fsutil repair
>適用于： Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

管理和監視 NTFS 自我修復修復作業。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil repair [enumerate] <volumepath> [<LogName>]
fsutil repair [initiate] <VolumePath> <FileReference>
fsutil repair [query] <VolumePath>
fsutil repair [set] <VolumePath> <Flags>
fsutil repair [wait][<WaitType>] <VolumePath>

```

### <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|列出|列舉磁片區損毀記錄檔的 entires。|
|\<volumepath >|將磁片區指定為磁片磁碟機名稱，後面接著冒號。|
|\<LogName >|$Corrupt-磁片區中已確認的損毀集。<br />$Verify-磁片區中的一組潛在、未經驗證的損毀。|
|初始|起始 NTFS 自我修復。|
|\<FileReference >|指定 NTFS 磁片區特定的檔案識別碼（檔案參考編號）。 檔案參考包含檔案的區段編號。|
|query|查詢 NTFS 磁片區的自我修復狀態。|
|設定|設定磁片區的自我修復狀態。|
|\<旗標 >|指定在設定磁片區的自我修復狀態時要使用的修復方法。<p>**Flags**參數可以設定為三個值：<p>-   **0x01**：啟用一般修復。<br />-   **0x09**：警告不進行修復的潛在資料遺失。<br />-   **0x00**：停用 NTFS 自我修復修復作業。|
|State|查詢系統的損毀狀態或指定的磁片區。|
|等候|等待修復完成。 如果 NTFS 在執行修復的磁片區上偵測到問題，此選項可讓系統等到修復完成後，再執行任何擱置中的腳本。|
|[WaitType {0&#124;1}]|指出是否等待目前的修復完成，或等候所有修復完成。 *WaitType*可以設定為下列值：<p>-   **0**：等候所有修復完成。 （預設值）<br />-   **1**：等候目前的修復完成。|

## <a name="remarks"></a>備註

-   自我修復 NTFS 會嘗試在線上更正 NTFS 檔案系統的損毀，而不需要執行**chkdsk.exe** 。 這項功能是在 Windows Server 2008 中引進。 如需詳細資訊，請參閱[自我修復 NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)。

## <a name="examples"></a><a name="BKMK_examples"></a>典型

若要列舉磁片區的已確認損毀，請輸入：

```
fsutil repair enumerate C: $Corrupt 
```

若要在 C 磁片磁碟機上啟用自我修復修復，請輸入：

```
fsutil repair set c: 1
```

若要停用 C 磁片磁碟機上的自我修復修復，請輸入：

```
fsutil repair set c: 0
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[自我修復 NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


