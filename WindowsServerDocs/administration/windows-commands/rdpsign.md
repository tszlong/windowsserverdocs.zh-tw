---
title: rdpsign
description: 了解如何以數位方式簽署 RDP 檔案。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 9e35d3a3e85ed046fb658bbf5a97ab5fc5eec6d3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442008"
---
# <a name="rdpsign"></a>rdpsign

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

可讓您以數位方式簽署遠端桌面通訊協定 (.rdp) 檔案。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|/sha1 \<hash>|指定憑證指紋，這是包含在憑證存放區簽章憑證的安全雜湊演算法 1 (SHA1) 雜湊。|
|/q|無訊息模式。 沒有輸出命令成功時，最少的輸出，如果命令失敗。|
|/v|詳細資訊模式。 顯示所有的警告、 訊息和狀態。|
|/l|測試的簽章和輸出的結果，而不會實際取代任何輸入檔。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   SHA1 憑證指模應該代表受信任之.rdp 檔案發行者。 若要取得憑證指紋，開啟 憑證 嵌入式管理單元中，按兩下您想要使用 （本機電腦的憑證存放區中或在您的個人憑證存放區中），按一下 憑證**詳細資料**索引標籤，然後在**欄位**清單中，按一下**指紋**。

    > [!NOTE]
    > 當您複製憑證指紋與 rdpsign.exe 工具搭配使用時，您必須移除任何空格。

-   若要使用的完整檔案名稱來簽署.rdp 檔案 （或檔案），您必須指定。 不接受萬用字元。
-   帶正負號的輸出檔案會覆寫輸入的檔。
-   如果無法讀取或寫入的任何.rdp 檔案，此工具會繼續下一個檔案中，如果指定了多個檔案。

## <a name="BKMK_examples"></a>範例
- 若要簽署的.rdp 檔，名為 File1.rdp，瀏覽至您儲存.rdp 檔案的資料夾，並接著輸入下列內容：
  ```
  rdpsign /sha1 hash file1.rdp
  ```
  > [!NOTE]
  > *雜湊*值代表 SHA1 憑證指紋，不包含任何空格。
- 若要測試是否數位簽署的.rdp 檔案成功而不會實際簽署檔案，請輸入下列命令：
  ```
  rdpsign /sha1 hash /l file1.rdp
  ```
- 登入多個.rdp 檔案，請使用空格分隔的檔案名稱。 例如，若要註冊多個名為 File1.rdp、 File2.rdp 和 File3.rdp 的.rdp 檔案，輸入下列命令：
  ```
  rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
  ```
  ## <a name="see-also"></a>另請參閱
  [命令列語法重點](command-line-syntax-key.md)
  [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
