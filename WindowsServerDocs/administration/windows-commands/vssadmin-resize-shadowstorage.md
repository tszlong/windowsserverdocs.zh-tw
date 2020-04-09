---
title: Vssadmin resize shadowstorage
description: Vssadmin resize shadowstorage 命令的描述。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 03/05/2020
ms.localizationpriority: medium
ms.openlocfilehash: 8cd3b5e5629e41702126fb42b815931ff0aea6fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830021"
---
# <a name="vssadmin-resize-shadowstorage"></a>Vssadmin resize shadowstorage

>適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

調整可用於陰影複製儲存體的最大儲存空間量。

您可以使用**MinDiffAreaFileSize**登錄值，指定可用於陰影複製儲存體的最小儲存空間數量。 如需詳細資訊，請參閱[MinDiffAreaFileSize](https://docs.microsoft.com/windows/win32/backup/registry-keys-for-backup-and-restore#mindiffareafilesize)。

> [!WARNING]
> 調整儲存體關聯的大小可能會導致陰影複製消失。

## <a name="syntax"></a>語法

```cmd
vssadmin resize shadowstorage /for=<ForVolumeSpec> /on=<OnVolumeSpec> [/maxsize=<MaxSizeSpec>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---|---|
`/for=<ForVolumeSpec>`  | 指定要調整大小的最大儲存空間量的磁片區。
`/on=<OnVolumeSpec>` | 指定存放磁片區。
`[/maxsize=<MaxSizeSpec>]` |  指定可用於儲存陰影複製的最大空間量。 如果未針對/maxsize 指定任何值，則可以使用的儲存空間數量沒有限制。  <br> <br> MaxSizeSpec 值必須是 1 MB 或更大，而且必須以下列其中一個單位表示： KB、MB、GB、TB、PB 或 EB。 如果未指定單位，MaxSizeSpec 預設會使用 bytes。

## <a name="examples"></a>範例

```cmd
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=900MB
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=UNBOUNDED
vssadmin Resize ShadowStorage /For=C: /On=C: /MaxSize=20%
```

## <a name="additional-references"></a>其他參考資料

* [命令列語法索引鍵](https://docs.microsoft.com/windows-server/administration/windows-commands/command-line-syntax-key)
* [Vssadmin](vssadmin.md)
