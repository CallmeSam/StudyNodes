### 待补充！

代码：
```c++
#include "network\HttpRequest.h"
#include "network\HttpClient.h"
#include "network\HttpResponse.h"

auto callback = [](network::HttpClient* client, network::HttpResponse* response)
    {
        log("response code  is %d", response->getResponseCode());
        if (response->isSucceed())
        {
            auto str = response->getResponseData();
            std::string context;
            for (const auto& alphabet : *str)
            {
                context.push_back(alphabet);
            }
            log("%s", context.c_str());
        }
    };

auto request = new network::HttpRequest();
request->setUrl("http://cl.mobile.ufo78.com/show.aspx?Key=100_201709070410553210_283590");
request->setRequestType(network::HttpRequest::Type::GET);
//char data[50] = "abcd";
//data[0] = 'c';
//request->setRequestData(data, strlen(data));
request->setResponseCallback(callback);


//Client
auto client = network::HttpClient::getInstance();
client->setTimeoutForConnect(30);
client->setTimeoutForRead(100);
client->send(request);
request->release();

auto postReq = new network::HttpRequest();
postReq->setUrl("http://httpbin.org/post");
postReq->setRequestType(network::HttpRequest::Type::POST);
const char* postData = "controller=Joe&action=testHttpPost";
postReq->setRequestData(postData, strlen(postData));
postReq->setResponseCallback(callback);

//client->send(postReq);
postReq->release();

```