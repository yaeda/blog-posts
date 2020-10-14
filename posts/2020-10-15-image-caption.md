---
title: 画像にキャプションを付けた
date: 2020-10-15
---

画像にキャプションを付けた．

```md:Markdownでこう書くと
![これは黒毛和牛ビビンバです](../images/2020-10-15-bibimbap.jpg)
```

```html:こういうHTMLになっていたところを
<p>
  <img
    src="../images/2020-10-15-bibimbap.jpg"
    alt="これは黒毛和牛ビビンバです"
  />
</p>
```

```html:こういうHTMLになるようにした
<figure>
  <img src="../images/2020-10-15-bibimbap.jpg" alt="" />
  <figcaption>これは黒毛和牛ビビンバです</figcaption>
</figure>
```

で，こうなる

![これは黒毛和牛ビビンバです](../images/2020-10-15-bibimbap.jpg)

厳密には画像の alt と caption は別のものだが，現実には内容が被ることが多く，caption を付ける場合は alt を空にするのを推奨している例もあるので採用した．

> NOTE: when adding a caption instead of alt text, the alt text attribute should be null (Alt=””).
>
> [Best Practices for Accessible Images \| California State University, Northridge](<https://www.csun.edu/universal-design-center/best-practices-accessible-images#:~:text=Images%20that%20have%20a%20caption,(%20Alt%3D%E2%80%9D%E2%80%9D%20).>)
