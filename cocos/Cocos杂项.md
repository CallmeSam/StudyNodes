## ActionManager
功能：动画管理，所有继承自Action的类。  
特点：
+ cocos中有一个默认的ActionManger，可以从Director中获取，它是由scheduler驱动。  
+ 如果不手动创建ActionManager，所有的runAction都会讲该action放入默认的ActionManager中。  
+ 如果想单独控制一类动画，则new一个ActionManager后，放入schedule中，由scheduler进行驱动。

***

## FileUtils
功能：  
+ 添加搜索路径和子路径
+ 读取或者写入文件(*xml文件可以直接读取或者写入为ValueMap或者ValueVector类型*)
+ 获取绝对路径，获取cocos写入路径

***
## FastTiledMap
```c++
#include "2d\CCFastTMXLayer.h"
#include "2d\CCFastTMXTiledMap.h"
auto map = cocos2d::experimental::TMXTiledMap::create("mission1-1.tmx");
addChild(map);
```
看法：  
+ 效率比常规的TiledMap高，GL calls和GL verts都有所减少。
+ 貌似配合cocos中的camera，会导致地图渲染不全。

***

## ProfilerDisplay
功能：cocos中效率探测器，可以测试代码的运行次数和时间。
```c++
#undef CC_PROFILER_DISPLAY_TIMERS
#define CC_PROFILER_DISPLAY_TIMERS() Profiler::getInstance()->displayTimers()
#undef CC_PROFILER_PURGE_ALL
#define CC_PROFILER_PURGE_ALL() Profiler::getInstance()->releaseAllTimers()

#undef CC_PROFILER_START
#define CC_PROFILER_START(__name__) ProfilingBeginTimingBlock(__name__)
#undef CC_PROFILER_STOP
#define CC_PROFILER_STOP(__name__) ProfilingEndTimingBlock(__name__)

CC_PROFILER_START("try");
for (int i = 0; i < 4000; i++)
{
    Node* node = Node::create();
}
CC_PROFILER_STOP("try");

this->runAction(
    Sequence::create(
        DelayTime::create(2.0f), 
        CallFunc::create([]() { CC_PROFILER_DISPLAY_TIMERS(); }),
         nullptr)
    );

```

***

## ParallexNode
用法：2d游戏场景中，有多层背景层，因近景远景而速度移动不一。  
```c++
ParallaxNode* parallex = ParallaxNode::create();
addChild(parallex);

auto sp = Sprite::create("HelloWorld.png");
parallex->addChild(sp, 2, Vec2(1.f, 0), Vec2(480, 320));

sp = Sprite::create("mario.png");
parallex->addChild(sp, 1, Vec2(0.8f, 0), Vec2(480, 320));

schedule([parallex](float dt) 
    {
        parallex->setPosition(parallex->getPosition() + Vec2(5, 0));
    }, 
"");
```

***

## ProgressTimer
作用：按比例渲染精灵
```c++
auto bar = ProgressTimer::create(Sprite::create("mario.png"));
bar->setType(ProgressTimer::Type::BAR);
bar->setBarChangeRate(Vec2(0.2f, 0.3f));
bar->setMidpoint(Vec2(0.5f, 0.5f));
bar->setPercentage(50);
addChild(bar);
bar->runAction(ProgressFromTo::create(3.0f, 100, 0));
```
说明：比较难理解的是Bar类型，有两个方法：  
+ setBarChangeRate(Vec2())。参数的意思为两个方向的变化范围比例。若Vec2(0.1f, 0.1f)，即是指，该精灵只有高度\*0.1，和宽度\*0.1的部分参与percentage的变化，也可理解为，若此时setPercentage(0),精灵大部分仍旧是显示的。若某一方向为0，则该方向全部渲染。
+ setMidpoint(Vec2())。该点即是基准点，若为Vec(0.3, 0.3)，changeRate为Vec(0.1f, 0.1f)，percentage为0，则精灵在锚点Vec2(0.3, 0.3)处，水平方向上，锚点左边渲染，width \* 0.3 \* (1 - 0.1),锚点右边渲染width \* 0.7 \* (1 - 0.1f)。竖直方向类似。  

Bug：  
 貌似bug，若changeRate(Vec(0.1f, 0.1f))。创建完成后直接设置percentage为0，则什么都不显示(bug)，但是如果runAction至percentage为0，则是正常。(*初始判断percentage为0，则不渲染, 应根据changeRate和percentage一起判断*)
***

## 其他：
+ utils类中有一些通用的功能(*如capture、获取当前时间...*)
