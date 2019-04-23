---
title: bitsadmin Transfer
description: 適用於 Windows 命令主題**bitsadmin 傳輸**-傳送一個或多個檔案。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef29a242a834fae42d1de3994a82aedcf87ec2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852459"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

傳送一或多個檔案。 若要傳送多個檔案，指定多個\<RemoteFileName\>-\<Fileandpath\>組。 配對會以空格分隔。

## <a name="syntax"></a>語法

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|名稱|作業名稱。 與大部分的命令，不同**名稱**只能名稱並不是 GUID。|
|類型|選擇性，指定作業的類型。 使用 **/下載**（預設值） 供下載作業或 **/上傳**針對上傳作業。|
|Priority|選擇性： 將 job_priority 設定為下列值之一：</br>-前景</br>-高</br>-標準</br>-低|
|ACLFlags|選擇性 — 表示您想要維護所下載之檔案的擁有者和 ACL 資訊。 例如，為了維持與檔案的擁有者和群組，將旗標設定為`OG`。 指定一或多個下列旗標：</br>-O:複製檔案的擁有者資訊。</br>-G:複製檔案群組資訊。</br>-D:複製檔案的 DACL 資訊。</br>-S:SACL 資訊複製檔案。|
|\/DYNAMIC|設定的工作[ **BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id)，它會放寬伺服器端需求。|
|RemoteFileName|當傳送到伺服器的檔案名稱。|
|LocalFileName|位於本機的檔案名稱。|

## <a name="remarks"></a>備註

根據預設，BITSAdmin 服務會建立在執行時的下載工作**一般**優先權並更新進度資訊的命令視窗，直到傳送已完成或發生重大錯誤。 如果順利傳輸所有的檔案，並取消作業，如果發生嚴重錯誤，服務就會完成的工作。 服務不會建立作業，如果無法將檔案新增至作業，或如果您指定的值無效*型別*或是*Job_Priority*。 若要傳送多個檔案，指定多個*RemoteFileName*-*Fileandpath*組。 配對會以空格分隔。

> [!NOTE]
> BITSAdmin 命令會繼續執行，如果發生暫時性錯誤。 若要結束命令，請按 CTRL + C。

## <a name="BKMK_examples"></a>範例

傳輸作業與下列範例會啟動名為*myDownloadJob*。
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)