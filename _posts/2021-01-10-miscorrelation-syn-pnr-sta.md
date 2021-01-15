---
layout: post
title: Miscorrelation btw syn pnr and sta
---

Let us discuss various scenarios where we migh see miscorrelation btw the diff tool results in this article.

## Btw PnR and STA
**Correlation checklist**
- Different Settings
- SPEF: Ensure same spef/rc are loaded in PnR and STA tool, that is , avoid calling extraction to prevent potential RC differences.
- SDC: Ensure same constraints are used in both tools. Incorrect timing constraints are a common cause of mismatch.

## Debugging timing issues in syn tool
CASE 1: Instance or net is preserved.
CASE 2: when instances are not placed closely.
CASE 3: Make sure syn and pnr tool derates are same.
CASE 4: SPEF
