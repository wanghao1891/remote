# 1、2015-05-11
## 1.1、使用ab进行post压力测试
    以测试美术名师搜索接口/v2/teachersearch为例：
      ab -n 1 -c 1 -v 10 -H 'userid: 543c9ed10dc897135076360c' -p post-form-data/v2-teachersearch -T 'application/x-www-form-urlencoded' http://127.0.0.1:3000/v2/teachersearch

    其中-v表示是否显示请求/回应信息，可以用来确定请求是否正确
    -v verbosity
            Set verbosity level - 4 and above prints information on headers, 3 and above  prints  response  codes  (404,  200,
            etc.), 2 and above prints warnings and info.
