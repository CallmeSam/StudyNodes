# ClippingNode

## 基本用法：
```c++
ClippingNode* clippingNode = ClippingNode::create();
//模板
Sprite* stencil = Sprite::create("CloseNormal.png");
clippingNode->setStencil(stencil);
//是否反向裁减
clippingNode->setInverted(true);
//阈值
clippingNode->setAlphaThreshold(0.05f);
addChild(clippingNode);

Sprite* bg = Sprite::create("HelloWorld.png");
clippingNode->addChild(bg);
```

## 不同：
  cocos的3.8版本和3.15版本有很较大改变，StencilStateManager是主要改变，细节改变如：
  + setStencil(stencil)中，会对之前的_stencil进行onExit()处理(*之前遇到的问题是，莫名某个精灵就不会动了*)
  


  ## bug：
  还未完全搞清楚bug出现规律，版本为15版本，症状为：
+ 某节点因为被设置为stencil后，位置被改变了，出现在左下角，无论怎么设置position都不会发生改变。
+ 一些精灵会不可见(*有个节点有阴影，设为stencil后，显示中阴影消失，阈值怎么调节都无法显示*)

## 源码阅读

