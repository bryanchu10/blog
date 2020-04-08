---
title: 鼠年全馬鐵人挑戰 03：網頁橫幅文字的半透明背景問題
date: 2020-04-05
tags: 
  - w3HexSchool
  - CSS
---
### 前言

之前在練習網頁排版的時候，碰過一個小問題。雖然是小問題，但當時卻花了不少時間解決。

例如像是這樣子的橫幅，該如何讓文字部分背景呈現半透明效果呢？

![Alt text](https://firebasestorage.googleapis.com/v0/b/blog-f0135.appspot.com/o/2020-04-05-banner-text-2.png?alt=media&token=cf651aa5-db54-4a52-830b-059c23b55564)

文字部分是用 h1 標籤撰寫，我第一個想到的作法是對 h1 設定 opacity。結果

![Alt text](https://firebasestorage.googleapis.com/v0/b/blog-f0135.appspot.com/o/2020-04-05-banner-text-3.png?alt=media&token=c735b290-334c-4cb3-9e5d-2cd2e2356c2c)

連文字部分也跟著半透明啦！

接下來就是誤入歧途的開始，我瞭解上面的問題是因為 opaicity 調整了整個物件的透明度，於是我把 h1 用一個新的 div 包裹，在這個 div 設定白色背景和透明度，但是呈現畫面和上面一模一樣。

真是令人氣餒，不過意外的知道 opacity 不只會影響被設定的物件的透明度，還會連帶的影響子元素的視覺呈現效果，也算是有所收穫。

### rgba( )

折騰了半天，才知道在 `background-color` 使用 `rgba()` 的寫法，就可以解決這個問題了。寫法是

```css
background: rgba(red, green, blue, alpha);
```
首先依序填入紅、綠、藍三原色的色值，可以使用 0 ~ 255 的整數亮度寫法，或是用百分比 0% ~ 100% 來表示亮度。

接著 alpha 代表透明度，範圍從 0.0 ~ 1.0，0 是完全穿透，1 是不透明。

### linear-gradient( )

後來瞭解到越來越多的 CSS 屬性設定和它們之間的關係，終於可以做出像這樣的橫幅了。

![alt text](https://firebasestorage.googleapis.com/v0/b/blog-f0135.appspot.com/o/2020-04-05-banner-text-1.png?alt=media&token=8020e6fb-6ab2-4033-89ef-61dbcf8ecff6)

在接近狐狸頭部的部份讓底色消失，使整體更美觀。

這是在 `background-image` 使用 `linear-gradient()` 的效果。寫法是

```css
background-image: linear-gradient(方向, 第一種顏色 範圍, 第二種顏色 範圍,...)
```

以我這個橫幅來說

```css
background-image: linear-gradient(to right, rgba(255,255,255,0.5) 40%, rgba(255,255,255,0) 60%);
```

就是方向由左至右，到達漸變距離距離 40% 處會是 rgba(255,255,255,0.5) 這個顏色，到 60% 處會是 rgba(255,255,255,0.5) 這個顏色。

由於顏色線性漸變的方向不一定是水平或是垂直，也可以設定 deg 來呈現傾斜的線性漸變效果，因此顏色變化的範圍也不一定是元素的寬度或高度，而這個顏色變化的範圍被稱為漸變距離。

詳細的漸變距離計算方式可以參考 MDN web docs 在 linear-gradient 條目的 [Composition of a linear gradient](https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient#Composition_of_a_linear_gradient) 這一段落。

最後，使用 linear-grdient 要注意一點，linear-gradient( ) 不能作用在 `background-color` 屬性，過去我因為習慣都用簡寫 `background` 屬性來設定各種背景資訊，以至於在不能使用簡寫的時候常犯這個錯誤。