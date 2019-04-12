# axois获取gbk编码乱码
>>> 不能让axios的结果直接返回json string，需要对二进制数据处理
>>> 配置 responseType: 'arraybuffer'，代码如下
```
const axios = require('axios');
const iconv = require('iconv-lite');

let url = 'xxxxxxxxxxxxxxxxxx';
axios.get(url, {
    responseType: 'arraybuffer'
})
    .then(response => {
        let data = response.data;
        let con = iconv.decode(data, 'gbk');
        console.log(con)
    })
```
