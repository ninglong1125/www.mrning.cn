---
title: "使用ohttps自带的docker-nginx来配置https服务"
date: 2022-01-08
draft: false
description: "使用ohttps自带的docker-nginx来配置https"
summary: "使用 ohttps 官方提供的nginx容器镜像ohttps/ohttps-nginx来配置网站https服务。"
tags: ["教程"]

cascade:
  showDate: true
  showAuthor: true
  showSummary: true
  invertPagination: true
---

## 介绍

> 你的浏览器可能不支持音乐自动播放，请手动播放。

 <audio id="music-player" autoplay controls >
    <source type="audio/mp3" src="/media/test.mp3"></source>
    <p>Your browser does not support the audio element.</p>
</audio>

<script>
    var player = document.getElementById("music-player");
    player.volume = 0.2;
    player.play()
    if (player.paused) { 
        player.paused=false;
        player.play(); 
    }    
</script>