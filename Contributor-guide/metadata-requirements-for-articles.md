---
title: 將必要的中繼資料標記新增至您的 Windows Server 相關文章
description: 資訊清單您必須將中繼資料標記為加入您的 Windows Server 相關文章的頂端。 所需的標記會有所變更，根據您報告和小組的需求。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: f7c514def1353d44386b1bc53c8cabffe1e31fda
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461637"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>將必要的中繼資料標記新增至您的 Windows Server 相關文章

在每篇文章的頂端，沒有特定的中繼資料必須包含的追蹤和 SEO 用途。 所需的標記會有所變更，根據報告需求。 不過，應該會通知您若要新增/移除的任何欄位。

它看起來應該像這樣，包括三個連字號 （-） 在頂端和底部：

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they’re looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager’s Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>範例

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server-threshold
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```