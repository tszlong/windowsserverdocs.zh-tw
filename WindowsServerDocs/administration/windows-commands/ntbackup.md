---
title: ntbackup
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b68b4cca579a5fc27f921ce2f4fcc6976d8e5600
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838011"
---
# <a name="ntbackup"></a>ntbackup



Windows Vista 或 Windows Server 2008 不提供**ntbackup**命令。 相反地，您應該使用**wbadmin**命令和子命令，從命令提示字元備份和還原您的電腦和檔案。

您無法使用**wbadmin**復原透過**ntbackup**所建立的備份。 不過，若要復原使用**ntbackup**所建立的備份，windows Server 2008 和 windows Vista 使用者可以下載一版的**ntbackup** 。 這個可下載的**ntbackup**版本可讓您只執行舊版備份的復原，而不能在執行 Windows Server 2008 或 Windows Vista 的電腦上使用它來建立新的備份。 若要下載此版本的**ntbackup**，請參閱[https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917)。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

[Restore](wbadmin.md)