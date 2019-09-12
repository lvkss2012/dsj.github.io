RestSharp中访问https方式url报错，报错信息如下所示：
```
"StatusCode: 0, Content-Type: , Content-Length: 0)"
"基础连接已经关闭: 发送时发生错误。"
```
需要再全局中设置
```
ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
```
示例代码：
```
using RestSharp;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new RestClient("https://ip:port/api/v1/");

            ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
            var request = new RestRequest("token", Method.POST);
            request.AddParameter("appKey", ""); // adds to POST or URL querystring based on Method
            request.AddParameter("appSecret", ""); // replaces matching token in request.Resource

            // execute the request
            IRestResponse response = client.Execute(request);
            var content = response.Content; // raw content as string
            Console.WriteLine(content);
        }
    }
}
```
