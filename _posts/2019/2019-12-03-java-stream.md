---
layout: post
title: java Stream
copyright: java
category: java
tags: [java,github]
excerpt: 开启Stream的世界
---

## 分组统计
```
Map<Integer, IntSummaryStatistics> map = list.stream().collect(Collectors.groupingBy(e -> DateUtil.beginOfMonth(e.getInputTime()).month(), Collectors.summarizingInt(BdMpuserPointsRecords::getPointsRecords)));

```