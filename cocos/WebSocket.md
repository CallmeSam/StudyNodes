### 待补充！

代码：
```c++
#include "network/WebSocket.h"

class Snake : public network::WebSocket::Delegate
{
public :
    virtual void onOpen(network::WebSocket* ws)
    {
        log("websocket open %p", ws);
    }
    virtual void onMessage(network::WebSocket* ws, const network::WebSocket::Data& data)
    {
        log("websocket send message %p", ws);
        if (!data.isBinary)
        {
            std::string str(data.bytes);
            log("%s", str.c_str());
        }
    }
    virtual void onClose(network::WebSocket* ws)
    {
        log("websocket close %p", ws);
    }
    virtual void onError(network::WebSocket* ws, const network::WebSocket::ErrorCode& error)
    {
        log("websocket oerror %p", ws);
    }
};

void LearnScene::AboutWebSocket()
{
    auto snake = new Snake();
    auto websocket = new network::WebSocket();

    auto menu = Menu::create();
    addChild(menu);

    auto label = MenuItemLabel::create(Label::create("init socket", "Arial", 40), [websocket, snake](cocos2d::Ref*) {
        if (!websocket->init(*snake, "ws://echo.websocket.org"))
        {
            delete websocket;
            //delete snake;
        }
    });
    menu->addChild(label);

    label = MenuItemLabel::create(Label::create("send Mes", "Arial", 40), [websocket](cocos2d::Ref*) {
            websocket->send("Hello world");
    });
    menu->addChild(label);

    menu->alignItemsVertically();
}

```