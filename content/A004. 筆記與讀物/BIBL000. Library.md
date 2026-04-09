---
cssclasses:
  - center-titles
  - cards
  - cards-align-bottom
  - cards-2-3
  - table-100
banner_icon: 📚
banner: "https://sorry-im-booked.com/wp-content/uploads/2019/11/5-relatable-reader-gifs-2.gif?w=500"
banner_y: 0.668
---
# **Library**
```dataviewjs
dv.table(["封面","书名", "作者", "类型"], dv.pages("#reading")
    .map(b => [("![](" + b.cover + ")"), b.aliases, b.file.link, b.author, b.genre]))
```
