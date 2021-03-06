## やったこと

throttleとdebounceについて

### [What is API Throttling?](https://www.tibco.com/reference-center/what-is-api-throttling#:~:text=API%20throttling%20is%20the%20process,click%20triggers%20an%20API%20call.)  

API throttling is the process of limiting the number of API requests a user can make in a certain period.   

API throttling also helps to fight back denial of service (DoS) attack, where a malicious user sends enormous volumes of requests to bring down a website or a mobile application. As the number of online users increases, businesses need to implement API throttling mechanisms to ensure fair usage, data security, and prevent malicious attacks.

API throttlingはDOS攻撃(一度に大量のリクエストを投げる攻撃)にも対策を打ってくれる

#### How Does API Throttling Work?
While there are various algorithms for API throttling, here are the basic steps in any API throttling algorithm:

1. A client/user calls an API that interfaces with a web service or application.
2. The API throttling logic checks if the current request exceeds the allowed number of API calls.
3. If the request is within limits, the API performs as usual and completes the user’s task.
4. If the request exceeds the limit, the API returns an error response to the user.
5. The user will have to wait for a pre-agreed time period, or pay to make any more API calls.

#### What Are the Benefits of API Throttling?
API throttling is a technique essential to all organizations that expose their services through APIs.

**Performance**
API throttling prevents the degradation of system performance by limiting the excess usage of an API. If an application has millions of users, a system might get a huge number of API requests per second. Servicing all those API requests will slow down the system and affect its performance. API throttling ensures that every user receives the performance ensured in the service level agreement (SLA).

サービスの品質向上につながる  

**Security**
An API throttling system acts as a gateway to an API. It helps to prevent the denial of service (DoS) attacks. In DoS, an attacker issues a massive number of service requests so that the service becomes unavailable to the legitimate users. By limiting the total number of service requests, API throttling helps to prevent DoS attacks.

Dos攻撃の対策になりうる

**Metering and Monetization**
APIs are one of the biggest assets of organizations. Monetizing the API usage contributes a significant share to their profit. API throttling helps organizations to meter the usage of their APIs. For example, a web service may offer 1000 free API calls per hour, but if users need more requests per hour, they have to pay for it.


### [What is API Throttling and Rate Limiting?](https://beabetterdev.com/2020/12/12/what-is-api-throttling-and-rate-limiting/)

**Throttling is a policy that the Server enforces and the Client respects.**  

## JSにおける間引き処理について

**debounce**
指定時間内に何度イベントが発生しても、最後の1回だけが実行される。
指定時間内にイベントが発生すると、先のイベントをキャンセルして最後のイベントからまた指定時間待って、その間にイベントが発生しなければ処理を実行する感じ。

**throttle**
一定時間内に1回だけイベントを実行させる。
時間を区切って、指定時間内に何度イベントが発生しても最後の1回だけを実行する。インターバルのようなイメージ。

### わかったこと
Web APIがrate limitの制限をかけるのは、Dos攻撃などの対策やパフォーマンス向上のため。  
クライアント側では、APIへのcallを最小限にする努力が必要。  

その方法としてJSの間引き処理(throttle, debounce)がある。  

参考
- [Implement Debouncing in React in 3 Different Ways](https://javascript.plainenglish.io/implementing-debouncing-in-react-f3316ef344f5)  






