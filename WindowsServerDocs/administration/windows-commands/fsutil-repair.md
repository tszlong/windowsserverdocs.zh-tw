---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: fsutil 修復
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 18c06b1f426105b098a5dc7e992b1e3becd3a4ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846679"
---
# <a name="fsutil-repair"></a>fsutil 修復
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理和監視 NTFS 自我修復的修復作業。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil repair [enumerate] <volumepath> [<LogName>]
fsutil repair [initiate] <VolumePath> <FileReference>
fsutil repair [query] <VolumePath>
fsutil repair [set] <VolumePath> <Flags>
fsutil repair [wait][<WaitType>] <VolumePath>

```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|列舉|列舉磁碟區的損毀記錄檔項的目。|
|\<volumepath>|指定的磁碟區做為磁碟機名稱，後面接著冒號。|
|\<LogName>|$ 損毀的-已確認的損毀磁碟區中的集合。<br />$ 驗證-一組磁碟區中的潛在、 未損毀。|
|起始|起始 NTFS 的自我修復。|
|\<FileReference>|指定的 NTFS 磁碟區特定的檔案識別碼 （檔案參考編號）。 檔案參考包含檔案的區段數目。|
|查詢|查詢自我修復 NTFS 磁碟區的狀態。|
|設定|設定磁碟區的自我修復的狀態。|
|\<旗標 >|指定時，將磁碟區的自我修復的狀態設定所要使用的修復方法。<br /><br />**旗標**參數可以設定為三個值：<br /><br />-   **0x01**:可讓一般的修復。<br />-   **0x09**:警告而不需要修復的潛在資料遺失。<br />-   **0x00**:停用 NTFS 自我修復的修復作業。|
|狀態|查詢的系統或指定的磁碟區損毀狀態。|
|等候|若要完成 repair(s) 等候。 如果 NTFS 已偵測到它在其執行修復磁碟區上的問題，此選項可讓系統在等候修復完成之前執行任何擱置中的指令碼。|
|[WaitType {0&#124;1}]|指出是否要等候目前的修復來完成，或等候完成的所有修復。 *Sysprocesses*可以設定為下列值：<br /><br />-   **0**:等候完成的所有修復。 （預設值）<br />-   **1**:等候完成目前的修復。|

## <a name="remarks"></a>備註

-   自我修復 NTFS 會嘗試修正損毀的 NTFS 檔案系統上線，而不需要**Chkdsk.exe**執行。 Windows Server 2008 中引進這項功能。 如需詳細資訊，請參閱 <<c0> [ 自我修復 NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)。

## <a name="BKMK_examples"></a>範例

若要列舉已確認的損毀的磁碟區，請輸入：

```
fsutil repair enumerate C: $Corrupt 
```

若要啟用自我修復磁碟機 C 上的修復，請輸入：

```
fsutil repair set c: 1
```

若要停用自我修復磁碟機 C 上的修復，請輸入：

```
fsutil repair set c: 0
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[自我修復 NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


