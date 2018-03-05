# RenderTexture
### 概念：
  可以理解为，创建一块画布，可以将所有Node节点渲染进去，形成一张贴图，类似于openGL里面的FBO，RenderTexture的源码也是使用了FBO。

### 基本用法：
```c++
Sprite* sp = Sprite::create("HelloWorld.png");
//画布大小，像素格式
RenderTexture* rd = RenderTexture::create(200, 200, Texture2D::PixelFormat::RGBA8888);
rd->beginWithClear(1, 0, 0, 1);     //清空画布为红色
sp->visit();                        //将sp绘制上去
rd->end();                          //绘制完毕
Director::getInstance()->regetRenderernder()->render();
auto texture = rd->getSprite()->getTexture();
```

### 实用：  
+ 制作幻影
+ 截图
+ 击打特效(*类似于ClippingNode的颜色裁减效果。绘制后进行颜色混合*)

### 源码解读：
...

