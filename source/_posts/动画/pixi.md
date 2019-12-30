---
title: pixi
date: 2019-01-14 20:13:48
categories:
- 动画
tags:
comments: false
---

### 介绍
Pixi是一个超快的2D渲染引擎。

### 安装
需要在你项目的根目录运行一个web服务器
#### 安装http-server

```js
npm install http-server -g
```

使用http-server打开

#### 安装 Pixi
Pixi的主要发布页面中获取最新版本。

创建一个基础的HTML页面，用一个<script>标签去加载你刚刚下载的pixi.min.js文件。

#### 测试用

```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Hello World</title>
</head>
  <script src="pixi/pixi.min.js"></script>
<body>
  <script type="text/javascript">
    let type = "WebGL"
    if(!PIXI.utils.isWebGLSupported()){
      type = "canvas"
    }

    PIXI.utils.sayHello(type)
  </script>
</body>
</html>
```


如果Pixi连接成功，一些这样的东西会在你的浏览器控制台里显示：

```js
PixiJS 4.4.5 - * canvas * http://www.pixijs.com/  ♥♥♥
```


### 创建Pixi应用和 舞台


```js
//Create a Pixi Application
let app = new PIXI.Application({width: 256, height: 256});

//Add the canvas that Pixi automatically created for you to the HTML document
document.body.appendChild(app.view);
```


第一步就是去创建一个可以显示图片的矩形显示区。Pixi拥有一个Pixi应用对象来帮助你创建它。它会自动创建一个<canvas>HTML标签并且计算出怎么去让你的图片在这个标签中显示。你现在需要创建一个特殊的Pixi容器对象，他被称作舞台。正如你所见，这个舞台对象将会被当作根容器而使用，它将包裹所有你想用Pixi显示的东西。

PIXI.Application算出了应该使用Canvas还是WebGL去渲染图象，它取决于你正在使用的浏览器支持哪一个。


```js
let app = new PIXI.Application({
    width: 256,         // default: 800
    height: 256,        // default: 600
    antialias: true,    // default: false 圆滑边界，使得字体的边界和几何图形更加圆滑
    transparent: false, // default: false 透明度
    resolution: 1       // default: 1 分辨率
  }
);
```


Pixi的画布对象将会默认选择WebGL引擎渲染模式，但是如果你需要强制使用Canvas引擎绘制而抛弃WebGL
forceCanvas: true,


```js
app.renderer.backgroundColor = 0x061639;  //改变背景颜色

app.renderer.autoResize = true; //确认宽高的格式正确
app.renderer.resize(512, 512); //改变大小

//让canvas占据整个窗口
app.renderer.view.style.position = "absolute";
app.renderer.view.style.display = "block";
app.renderer.autoResize = true;
app.renderer.resize(window.innerWidth, window.innerHeight);
<style>* {padding: 0; margin: 0}</style>
```


### Pixi 精灵
Pixi拥有一个精灵类来创建游戏精灵。有三种主要的方法来创建它：
- 用一个单图像文件创建。
- 用一个 雪碧图 来创建。雪碧图是一个放入了你游戏所需的所有图像的大图。
- 从一个纹理贴图集中创建。（纹理贴图集就是用JSON定义了图像大小和位置的雪碧图）

将图片加载到纹理缓存中
因为Pixi用WebGL和GPU去渲染图像，所以图像需要转化成GPU可以处理的版本。可以被GPU处理的图像被称作 纹理 。在你让精灵显示图片之前，需要将普通的图片转化成WebGL纹理。

Pixi强大的loader对象可以加载任何你需要种类的图像资源。


```js
PIXI.loader
  .add([
  "images/imageOne.png",
  "images/imageTwo.png",
  "images/imageThree.png"
])
  .load(setup);
```


#### 显示精灵

```js
app.stage.addChild(cat);

//Create a Pixi Application
let app = new PIXI.Application({
  width: 256,
  height: 256,                       
  antialias: true,
  transparent: false,
  resolution: 1
}
                              );

//Add the canvas that Pixi automatically created for you to the HTML document
document.body.appendChild(app.view);

//load an image and run the `setup` function when it's done
PIXI.loader
  .add("images/cat.png")
  .load(setup);

//This `setup` function will run when the image has loaded
function setup() {

  //Create the cat sprite
  let cat = new PIXI.Sprite(PIXI.loader.resources["images/cat.png"].texture);

  //Add the cat to the stage
  app.stage.addChild(cat);
}
```

如果你想把一个精灵从舞台上挪走，可以把精灵的visible属性设置成false来让精灵简单的隐藏。
anySprite.visible = false;

#### 使用别名
来看看怎么将所有的Pixi对象和方法改成别名之后，来重写加载和显示图像的代码。

```js
//Aliases
let Application = PIXI.Application,
    loader = PIXI.loader,
    resources = PIXI.loader.resources,
    Sprite = PIXI.Sprite;

//Create a Pixi Application
let app = new Application({
  width: 256,
  height: 256,                       
  antialias: true,
  transparent: false,
  resolution: 1
}
                         );

//Add the canvas that Pixi automatically created for you to the HTML document
document.body.appendChild(app.view);

//load an image and run the `setup` function when it's done
loader
  .add("images/cat.png")
  .load(setup);

//This `setup` function will run when the image has loaded
function setup() {

  //Create the cat sprite
  let cat = new Sprite(resources["images/cat.png"].texture);

  //Add the cat to the stage
  app.stage.addChild(cat);
}
```


#### 一些关于加载的其他知识
- 使用普通的javaScript 

Img对象或canvas创建一个精灵
如果因为某些原因你需要从JavaScript的Image对象之中创建，你可以使用Pixi的BaseTexture和Texture类：

```js
let base = new PIXI.BaseTexture(anyImageObject),
    texture = new PIXI.Texture(base),
    sprite = new PIXI.Sprite(texture);
```

- 你可以使用BaseTexture.fromCanvas从任何已经存在canvas标签中创建纹理：

```js
let base = new PIXI.BaseTexture.fromCanvas(anyCanvasElement),
```

- 如果你想改变已经显示的精灵的纹理，使用texture属性，可以设置任何Texture对象，像下面这样：

```js
anySprite.texture = PIXI.utils.TextureCache["anyTexture.png"];
```

- 你可以使用这个技巧在游戏发生一些重大变化时交互式的改变精灵的纹理。

监视加载进程

```js
PIXI.loader.on("progress", loadProgressHandler);

PIXI.loader
  .add([
  "images/one.png",
  "images/two.png",
  "images/three.png"
])
  .on("progress", loadProgressHandler)
  .load(setup);

function loadProgressHandler() {
  console.log("loading");
}

function setup() {
  console.log("setup");
}
loading
loading
loading
setup

PIXI.loader
  .add([
  "images/one.png",
  "images/two.png",
  "images/three.png"
])
  .on("progress", loadProgressHandler)
  .load(setup);

function loadProgressHandler(loader, resource) {

  //Display the file `url` currently being loaded
  console.log("loading: " + resource.url);

  //Display the percentage of files currently loaded
  console.log("progress: " + loader.progress + "%");

  //If you gave your files names as the first argument
  //of the `add` method, you can access them like this
  //console.log("loading: " + resource.name);
}

function setup() {
  console.log("All files loaded");
}
loading: images/one.png
progress: 33.333333333333336%
  loading: images/two.png
progress: 66.66666666666667%
  loading: images/three.png
progress: 100%
  All files loaded
  //resource.error会告诉你有哪些加载时候的错误
```


add 方法有四个基础参数:

```js
add(name, url, optionObject, callbackFunction)
```

这里有文档里面对这些参数的描述：
> name (string): 加载源文件的别名,如果没设置，url就会被放在这.

> url (string): 源文件的地址，是加载器 baseUrl的相对地址.

> options (object literal): 加载设置.

> options.crossOrigin (Boolean): 源文件请求跨域不？默认是自动设定的。

> options.loadType: 源文件是怎么加载进来的？默认是Resource.LOAD_TYPE.XHR。 options.xhrType: 用XHR的时候该怎么处理数据？ 默认是Resource.XHR_RESPONSE_TYPE.DEFAULT。

> callbackFunction: 当这个特定的函数加载完，这个特定的函数将会被执行。

只有url必填（你总得加载个文件吧。）

第一个就是文档里所谓的“正常语法”：

```js
.add('key', 'http://...', function () {})
.add('http://...', function () {})
.add('http://...')
```


第二个就是所谓“对象语法”啦：

```js
.add({
  name: 'key2',
  url: 'http://...'
}, function () {})

.add({
  url: 'http://...'
}, function () {})

.add({
  name: 'key3',
  url: 'http://...'
  onComplete: function () {}
})

.add({
  url: 'https://...',
  onComplete: function () {},
  crossOrigin: true
})
```


第三个可以给add方法传一个对象的数组，或者既使用对象数组，又使用链式加载：

```js
.add([
  {name: 'key4', url: 'http://...', onComplete: function () {} },
  {url: 'http://...', onComplete: function () {} },
  'http://...'
]);
```


（注意：如果你需要重新加载一批文件，调用加载器的reset方法：PIXI.loader.reset();）

### 精灵位置

```js
cat.x = 96;
cat.y = 96;

sprite.position.set(x, y)
```


### 大小和比例

```js
cat.width = 80;
cat.height = 120;

cat.scale.x = 0.5;
cat.scale.y = 0.5;
cat.scale.set(0.5, 0.5);
```


### 旋转

```js
cat.rotation = 0.5;
// 旋转是相对于锚点的，有两种方法设置锚点
cat.anchor.x = 0.5;
cat.anchor.y = 0.5;
cat.anchor.set(x, y)
```


### 原点

```js
cat.pivot.set(32, 32)
```

假设精灵图是64x64像素，它将绕着它的中心点旋转。但是记住：你如果改变了精灵的pivot属性，你也就改变了它的原点位置。

anchor改变了精灵纹理的图像原点，用0到1的数据来填充。pivot则改变了精灵的原点，用像素的值来填充。

从精灵图（雪碧图）中创建精灵


```js
loader
  .add("images/tileset.png")
  .load(setup);
function setup() {

  //Create the `tileset` sprite from the texture
  let texture = TextureCache["images/tileset.png"];

  //Create a rectangle object that defines the position and
  //size of the sub-image you want to extract from the texture
  //(`Rectangle` is an alias for `PIXI.Rectangle`)
  let rectangle = new Rectangle(192, 128, 64, 64); //Pixi内置了一个通用的Rectangle对象 (PIXI.Rectangle)，他是一个用于定义矩形形状的通用对象。他需要一些参数，前两个参数定义了x 和y轴坐标位置，后两个参数定义了矩形的width 和 height

  //Tell the texture to use that rectangular section
  texture.frame = rectangle; //Pixi的纹理中有一个叫做frame的很有用的属性，它可以被设置成任何的Rectangle对象。frame将纹理映射到Rectangle的维度。

  //Create the sprite from the texture
  let rocket = new Sprite(texture);

  //Position the rocket sprite on the canvas
  rocket.x = 32;
  rocket.y = 32;

  //Add the rocket to the stage
  app.stage.addChild(rocket);

  //Render the stage   
  renderer.render(stage);
}
```

### 使用一个纹理贴图集
一个纹理贴图集就是一个JSON数据文件，它包含了匹配的PNG雪碧图的子图像的大小和位置。如果你使用了纹理贴图集，那么想要显示一个子图像只需要知道它的名字就行了。你可以任意的排序你的排版，JSON文件会保持他们的大小和位置不变。这非常方便，因为这意味着图片的位置和大小不必写在你的代码里。如果你想要改变纹理贴图集的排版，类似增加图片，修改图片大小和删除图片这些操作，只需要修改那个JSON数据文件就行了，你的游戏会自动给程序内的所有数据应用新的纹理贴图集。你没必要在所有用到它代码的地方修改它。

Pixi兼容著名软件Texture 

Packer输出的标准纹理贴图集格式。


如果你正在用免费版的Texture Packer，把 Algorithm 选项设为Basic，把 Trim mode 选项设为None，把 Extrude 选项设为0，把 Size constraints 选项设为 Any size ，把 PNG Opt Level 中所有的东西都滑到左边的 0位置。这就可以使得Texture Packer正常的输出你的纹理贴图集。
如果你做完了，点击 Publish 按钮。选择输出文件名和存储地址，把生成文件保存起来。你将会获得两个文件：一个叫做treasureHunter.json，另外一个就是treasureHunter.png。为了让目录干净些，我们把他俩都放到一个叫做images的文件夹里面去。（你可以认为那个json文件是图片文件的延伸，所以把他们放进一个文件夹是很有意义的。）那个JSON文件里面写清楚了每一个子图像的名字，大小和位置。下面描述了“泡泡怪”这个怪物的子图像的信息。

```js
"blob.png":
{
	"frame": {"x":55,"y":2,"w":32,"h":24},
	"rotated": false,
	"trimmed": false,
	"spriteSourceSize": {"x":0,"y":0,"w":32,"h":24},
	"sourceSize": {"w":32,"h":24},
	"pivot": {"x":0.5,"y":0.5}
},
```

这些子图像每一个都被叫做 帧 ,有了这些数据你就不用去记每一个图片的大小和位置了，你唯一要做的就只是确定精灵的 帧ID 即可。帧ID就是那些图片的原始名称，类似"blob.png"或者 "explorer.png"这样。

#### 加载纹理贴图集
建议给纹理贴图集的textures对象创建一个叫做id的别名

```js
let id = PIXI.loader.resources["images/treasureHunter.json"].textures;
let id = PIXI.loader.resources["images/treasureHunter.json"].textures;

//Define variables that might be used in more
//than one function
let treasure, id;

function setup() {

  //Create an optional alias called `id` for all the texture atlas
  //frame id textures.
  id = PIXI.loader.resources["images/treasureHunter.json"].textures;

  //Make the treasure box using the alias
  treasure = new Sprite(id["treasure.png"]);
  app.stage.addChild(treasure);

  //Position the treasure next to the right edge of the canvas
  treasure.x = app.stage.width - treasure.width - 48;
  treasure.y = app.stage.height / 2 - treasure.height / 2;
  app.stage.addChild(treasure);
}


let numberOfBlobs = 6,
    spacing = 48,
    xOffset = 150;

//Make as many blobs as there are `numberOfBlobs`
for (let i = 0; i < numberOfBlobs; i++) {

  //Make a blob
  let blob = new Sprite(id["blob.png"]);

  //Space each blob horizontally according to the `spacing` value.
  //`xOffset` determines the point from the left of the screen
  //at which the first blob should be added.
  let x = spacing * i + xOffset;

  //Give the blob a random y position
  //(`randomInt` is a custom function - see below)
  let y = randomInt(0, app.stage.height - blob.height);

  //Set the blob's position
  blob.x = x;
  blob.y = y;

  //Add the blob sprite to the stage
  app.stage.addChild(blob);
}
}
//The `randomInt` helper function
function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

randomInt是一个很好的用来做游戏的工具函数

### 移动精灵
使用Pixi的ticker。这被称为 游戏循环 。任何在游戏循环里的代码都会1秒更新60次。

### 游戏状态

```js
//Set the game state
state = play;

//Start the game loop
app.ticker.add(delta => gameLoop(delta));

function gameLoop(delta){

  //Update the current game state:
  state(delta);
}

function play(delta) {

  //Move the cat 1 pixel to the right each frame
  cat.vx = 1
  cat.x += cat.vx;
}
```

delta的值代表帧的部分的延迟。你可以把它添加到cat的位置，让cat的速度和帧率无关。下面是代码:
cat.x += 1 + delta;
是否加进去这个delta的值其实是一种审美的选择。它往往只在你的动画没法跟上60帧的速率时候出现（比如你的游戏运行在很老旧的机器上）。

 速度属性 ：vx和 vy去控制精灵的运动速度。

### 键盘移动


```js
function keyboard(keyCode) {
  let key = {};
  key.code = keyCode;
  key.isDown = false;
  key.isUp = true;
  key.press = undefined;
  key.release = undefined;
  //The `downHandler`
  key.downHandler = event => {
    if (event.keyCode === key.code) {
      if (key.isUp && key.press) key.press();
      key.isDown = true;
      key.isUp = false;
    }
    event.preventDefault();
  };

  //The `upHandler`
  key.upHandler = event => {
    if (event.keyCode === key.code) {
      if (key.isDown && key.release) key.release();
      key.isDown = false;
      key.isUp = true;
    }
    event.preventDefault();
  };

  //Attach event listeners
  window.addEventListener(
    "keydown", key.downHandler.bind(key), false
  );
  window.addEventListener(
    "keyup", key.upHandler.bind(key), false
  );
  return key;
}
```

用的方法：

```js
let keyObject = keyboard(asciiKeyCodeNumber);
keyObject.press = () => {
  //key object pressed
};
keyObject.release = () => {
  //key object released
};
```


例子

```js
//Define any variables that are used in more than one function
let cat, state;

function setup() {

  //Create the `cat` sprite
  cat = new Sprite(resources["images/cat.png"].texture);
  cat.y = 96;
  cat.vx = 0;
  cat.vy = 0;
  app.stage.addChild(cat);

  //Capture the keyboard arrow keys
  let left = keyboard(37),
      up = keyboard(38),
      right = keyboard(39),
      down = keyboard(40);

  //Left arrow key `press` method
  left.press = () => {
    //Change the cat's velocity when the key is pressed
    cat.vx = -5;
    cat.vy = 0;
  };

  //Left arrow key `release` method
  left.release = () => {
    //If the left arrow has been released, and the right arrow isn't down,
    //and the cat isn't moving vertically:
    //Stop the cat
    if (!right.isDown && cat.vy === 0) {
      cat.vx = 0;
    }
  };

  //Up
  up.press = () => {
    cat.vy = -5;
    cat.vx = 0;
  };
  up.release = () => {
    if (!down.isDown && cat.vx === 0) {
      cat.vy = 0;
    }
  };

  //Right
  right.press = () => {
    cat.vx = 5;
    cat.vy = 0;
  };
  right.release = () => {
    if (!left.isDown && cat.vy === 0) {
      cat.vx = 0;
    }
  };

  //Down
  down.press = () => {
    cat.vy = 5;
    cat.vx = 0;
  };
  down.release = () => {
    if (!up.isDown && cat.vx === 0) {
      cat.vy = 0;
    }
  };

  //Set the game state
  state = play;

  //Start the game loop
  app.ticker.add(delta => gameLoop(delta));
}

function gameLoop(delta){

  //Update the current game state:
  state(delta);
}

function play(delta) {

  //Use the cat's velocity to make it move
  cat.x += cat.vx;
  cat.y += cat.vy
}
```
