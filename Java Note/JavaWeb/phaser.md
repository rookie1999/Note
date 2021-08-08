# Phaser

## 1、介绍

```txt
Phaser是一款专门用于桌面及移动HTML5 2D游戏开发的开源免费框架，提供JavaScript和TypeScript双重支持，内置游戏对象的物理属性，采用Pixi.js引擎以加快Canvas和WebGL渲染，基于浏览器支持可自由切换。
快速、免费、易于维护，使用Phaser来开发2D小游戏的优势显而易见。一方面，开发者可以直接通过Koding平台上的VM开发系统进行代码编写及预览。另一方面，也可以在支持Canvas的浏览器中直接安装Phaser来进行游戏开发。
```

## 2、创建游戏和添加环境

1. 在html文件中导入`phaser.js`

   html网页页面的作用就是为游戏提供一个容器，实现游戏的代码全部由Javascript实现

2. Phaser.Game实例化对象来创建游戏

3. 场景通过Phaser.Game.state添加和启动，同一时间只能存在一个游戏场景

4. 游戏资源的添加和对象的创建

   ```js
   // 加载图片
   game.load.image(key, url);
   
   // 加载精灵图
   game.load.spritesheet(key, url, framewidth, frameHeight, frameMaxAmount);
   
   // 
   ```

   

5. 

