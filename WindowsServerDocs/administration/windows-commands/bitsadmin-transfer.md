---
title: bitsadmin Transfer
description: Bitsadmin 傳輸的 Windows 命令主題，它會傳送一或多個檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e960b4d94416d57e6c42ec27057dafef5e44516
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849001"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

傳送一或多個檔案。 若要傳送多個檔案，請指定多個 \<RemoteFileName\>-\<LocalFileName\> 配對。 配對會以空格分隔。

## <a name="syntax"></a>語法

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|名稱|作業的名稱。 不同于大部分的命令，**名稱**只可以是名稱，而不是 GUID。|
|類型|選擇性：指定作業的類型。 針對上傳作業使用下載作業或 **/UPLOAD**的 **/DOWNLOAD** （預設值）。|
|優先順序|選擇性-將 job_priority 設定為下列其中一個值：</br>-前景</br>-高</br>-正常</br>-低|
|ACLFlags|選擇性-表示您想要在要下載的檔案中維護擁有者和 ACL 資訊。 例如，若要以檔案維護擁有者和群組，請將旗標設定為 `OG`。 指定下列一個或多個旗標：</br>-O：使用檔案複製擁有者資訊。</br>-G：使用 file 複製群組資訊。</br>-D：使用 file 複製 DACL 資訊。</br>-S：使用 file 複製 SACL 資訊。|
|/DYNAMIC|使用[**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id)設定作業，以放寬伺服器端需求。|
|RemoteFileName|傳送到伺服器時的檔案名。|
|LocalFileName|位於本機的檔案名。|

## <a name="remarks"></a>備註

根據預設，BITSAdmin 服務會建立以**一般**優先權執行的下載作業，並以進度資訊更新命令視窗，直到傳輸完成或發生嚴重錯誤為止。 如果作業成功傳輸所有檔案，且在發生嚴重錯誤時取消作業，服務就會完成工作。 如果無法將檔案加入至作業，或如果您為*類型*或*Job_Priority*指定了不正確值，服務就不會建立作業。 若要傳送一個以上的檔案，請指定多個*RemoteFileName*-*LocalFileName*組。 配對會以空格分隔。

> [!NOTE]
> 如果發生暫時性錯誤，BITSAdmin 命令會繼續執行。 若要結束命令，請按 CTRL + C。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會啟動名為*myDownloadJob*的傳送工作。
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)