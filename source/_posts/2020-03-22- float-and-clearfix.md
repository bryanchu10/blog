---
title: 鼠年全馬鐵人挑戰 01：曾經困擾我的 float 問題 
date: 2020-03-22
tags: 
  - w3HexSchool
  - CSS
---
### 前言

六角學院最近的一次課程更新，正式從基礎課程就開始介紹 flex 排版。原本就知道現在 flex 排版已經取得主流地位了，但是看到六角課程更新，還是讓我有一種 float 排版逐漸成為歷史遺跡的感覺，也回想起學習 float 排版的過程中踩到的那些坑，決定在告別 float 之前，把曾經困擾我的 float 問題記錄下來。

### 元素設定 float 前後的表現

- 區塊元素佔據的橫向空間

因為太常使用 float 了，所以很容易忘記，原本區塊元素（block element）會佔據目前身處的容器中高度範圍內所有的橫向空間。即使對區塊元素設定 width，區塊元素還是會占滿整個元素空間。設定 width，只會影響到它的 content 寬度，並沒有辦法使其不佔據所有橫向空間。如果在同一層級再擺一個元素，因為區塊元素已經佔據自身高度內的橫向空間，新元素位置會在這個區塊元素的下一行。

使用了 float 之後，元素佔據的橫向空間則會是 content 寬度 + padding 寬度 + border 寬度 + margin 寬度。

- 行內元素的 display 設定

行內元素（inline element）原本設定 width、height、margin-top、margin-bottom 不會產生效果，padding-top 與 padding-bottom 沒有辦法撐開空間。但是 float 之後會帶有區塊元素的特性，這時候上述無法發揮作用的屬性都可以運作。

這邊容易忘記的是，因為 float 讓行內元素帶有區塊元素的特性，所以在程式碼撰寫上，如果有將行內元素轉成區塊元素的需求，又剛好已經有 float 設定，就不用再寫 `display: block` 了。

### ul li 的清除浮動問題

考慮一個簡單的[網頁架構](https://codepen.io/bryanchu10/pen/yLNxLOY)，在這個 CodePen 中，因為 h1 和 ul 都使用了 float，所以在 h1 和 ul 的父層元素 header 內部結尾處要清除浮動，確定 header 內部的浮動現象不會影響到 header 之外。在這邊使用偽元素的方式，在 header 標籤內加入 clearfix 這個類別選擇器後，header 的最後會產生一個可以清除浮動的偽元素。

- 以浮動元素包裹浮動元素

注意 header 內右側的連結列，不只是 ul 有使用 float，li 也有使用 float，於是產生的一個疑問：為甚麼不需要在 ul 內部結尾清除浮動，來避免 li 的浮動現象超出 ul 呢？

原因是當父元素也使用了 float 的時候，子元素的 float 現象就不會影響到父元素之外，有人稱這個現象為「以浮制浮」。

- ul 架構規範

在 [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul) ，ul 允許的內容是 

> Zero or more `<li>`, `<script>` and `<template>` elements.

所以如果是這樣的架構

```html
<ul>
  <li><a href="#">連結一</a></li>
  <li><a href="#">連結二</a></li>
  <li><a href="#">連結三</a></li>
  <li><a href="#">連結四</a></li>
  <div class="clearfix"></div>
</ul>
```
以規範來說是有問題的。雖然我在注意到這個規範之前都是這樣寫，也沒有出過問題。不過因為這個原因，後來如果需要在 ul 清除浮動時，就改成用偽元素置入清除浮動的方式來迴避違規範了，例如 CodePen 中的 HTML 第 12-17 行。

