---
title: 管理基本磁碟區
description: 本文說明如何管理基本磁碟區
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c75d887a6427673319999522b890d523f4276871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870959"
---
# <a name="manage-basic-volumes"></a>管理基本磁碟區

> **適用於：** Windows 10，Windows 8.1、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

基本磁碟是包含主要磁碟分割、延伸磁碟分割或邏輯磁碟機的實體磁碟。 基本磁碟上的磁碟分割和邏輯磁碟機即稱為基本磁碟區。 您只能在基本磁碟上建立基本磁碟區。

您可以藉由將現有的主要磁碟分割及邏輯磁碟機延伸到相同磁碟上相鄰的連續未配置空間，將更多空間新增至這些磁碟區。 若要延伸基本磁碟區，必須以 NTFS 檔案系統來格式化該磁碟區。 您可以在包含邏輯磁碟機之延伸磁碟分割的連續可用空間內延伸該磁碟機。 如果您延伸邏輯磁碟機超出延伸磁碟分割中提供的可用空間，只要延伸磁碟分割後面接著連續未配置的空間，延伸磁碟分割就會擴大來包含邏輯磁碟機。

## <a name="see-also"></a>另請參閱

-   [指派磁碟機的掛接點資料夾路徑](assign-a-mount-point-folder-path-to-a-drive.md)
-   [延伸基本磁碟區](extend-a-basic-volume.md)
-   [壓縮基本磁碟區](shrink-a-basic-volume.md)