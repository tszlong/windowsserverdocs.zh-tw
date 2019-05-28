---
title: 延伸基本磁碟區
description: 本文說明如何在延伸基本磁碟區的主要及邏輯磁碟機上新增空間
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c20e2da3e629743ab4d4d4cf1da16a6e69093ecf
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192578"
---
# <a name="extend-a-basic-volume"></a>延伸基本磁碟區

> **適用於：** Windows 10，Windows 8.1、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

您可以藉由將現有的主要磁碟分割及邏輯磁碟機延伸到相同磁碟上相鄰未配置的空間，將更多空間新增至這些磁碟區。 若要延伸基本磁碟區，此磁碟區必須是未經處理 (未透過檔案系統格式化) 或是已格式化為 NTFS 檔案系統。 您可以在包含邏輯磁碟機之延伸磁碟分割的連續可用空間內延伸該磁碟機。 如果您延伸邏輯磁碟機超出延伸磁碟分割中提供的可用空間，延伸磁碟分割會擴大來包含邏輯磁碟機。

延伸邏輯磁碟機以及開機或系統磁碟區時，您只能將磁碟區延伸到連續空間，而且僅限可以升級為動態磁碟的磁碟。 其他磁碟區則可以延伸至非連續空間中，但系統會提示您將磁碟轉換成動態磁碟。

## <a name="extending-a-basic-volume"></a>延伸基本磁碟區

-   [使用 Windows 介面](#to-extend-a-basic-volume-using-the-windows-interface)
-   [使用命令列](#to-extend-a-basic-volume-using-a-command-line)

#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>若要使用 Windows 介面延伸基本磁碟區

1. 在 [磁碟管理員] 中，以滑鼠右鍵按一下您想要延伸的基本磁碟區。

2. 按一下 **\[延伸磁碟區\]** 。

3. 遵循畫面上的指示操作。

#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>若要使用命令列延伸基本磁碟區

1. 開啟命令提示字元，然後輸入 `diskpart`：

2. 在 **DISKPART** 提示中輸入 `list volume`。 記下您要延伸的基本磁碟區。

3. 在 **DISKPART** 提示中輸入 `select volume <volumenumber>`。 這樣就會選取您要延伸至相同磁碟中連續空白空間的基本磁碟區 *volumenumber*。

4. 在 **DISKPART** 提示中輸入 `extend [size=<size>]`。 這樣就會以*大小*單位為 MB 的幅度延伸選取的磁碟區。

| 值 | 描述 |
| --- | --- |
| <p>**清單中的磁碟區**</p> | <p>顯示所有磁碟上的基本和動態磁碟區。</p> |
| <p>**選取磁碟區**</p> | <p>選取指定的磁碟區 (其中 <em>volumenumber</em> 是磁碟區編號)，並讓它成為焦點。 如果沒有指定磁碟區，**select** 會命令列出焦點所在的目前磁碟區。 您可以用編號、磁碟機代號或掛接點路徑來指定磁碟區。 在基本磁碟上，選取磁碟區也會讓對應的磁碟分割成為焦點。</p> |
| <p>**extend**</p> | <p><ul><li>將焦點所在磁碟區延伸到下一個連續未配置的空間。 延伸基本磁碟區時，未配置的空間必須同在一個磁碟上，並且必須跟隨在具有焦點的磁碟分割之後 (磁區位移較高)。 動態的簡單磁碟區或合併磁碟區可以延伸到任何動態磁碟區上的任何空白空間。 您可以使用這個命令，將現有的磁碟區延伸到新建立的空間。</p></li ><p><li>如果先前已透過 NTFS 檔案系統將磁碟分割格式化，檔案系統會自動進行延伸以佔用更大的磁碟分割。 不會有任何資料遺失。 如果磁碟分割先前是使用 NTFS 以外的任何檔案系統格式來格式化，命令會失敗，但不會變更磁碟分割。</p></li></ul>|
| <p>**size=** <em>size</em></p> | <p>要新增至目前磁碟分割的空間數量，以 MB 為單位。 如果未指定大小，則會將磁碟延伸到佔用所有的連續未配置空間。</p> |

## <a name="additional-considerations"></a>其他考量

-   如果磁碟沒有包含開機或系統磁碟分割，就可以將磁碟區延伸到其他非開機或非系統磁碟，但該磁碟將會轉換成動態磁碟 (若磁碟可升級)。

## <a name="see-also"></a>另請參閱

-   [命令列語法標記法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)
