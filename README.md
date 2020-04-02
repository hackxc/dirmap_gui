# dirmap_gui

觉得dirmap挺不错的 添加Gui界面，更加方便使用

支持拖拽目标文件，进行批量扫描，结果自动保存

![demo](https://github.com/hackxc/dirmap_gui/raw/master/img/demo.png)

源地址：https://github.com/H4ckForJob/dirmap

一个高级web目录扫描工具，功能将会强于DirBuster、Dirsearch、cansina、御剑

# 环境准备
python3 -m pip install -r requirement.txt

# 默认字典文件
字典文件存放在项目根目录中的data文件夹中

1.dict_mode_dict.txt “字典模式”字典，使用dirsearch默认字典
2.crawl_mode_suffix.txt “爬虫模式”字典，使用FileSensor默认字典
3.fuzz_mode_dir.txt “fuzz模式”字典，使用DirBuster默认字典
4.fuzz_mode_ext.txt “fuzz模式”字典，使用常见后缀制作的字典
5.dictmult 该目录为“字典模式”默认多字典文件夹，包含：BAK.min.txt(备份文件小字典)，BAK.txt(备份文件大字典)，LEAKS.txt(信息泄露文件字典)
6.fuzzmult 该目录为“fuzz模式”默认多字典文件夹，包含：fuzz_mode_dir.txt(默认目录字典)，fuzz_mode_ext.txt(默认后缀字典)

# 高级使用

自定义dirmap配置，开始探索dirmap高级功能

暂时采用加载配置文件的方式进行详细配置，不支持使用命令行参数进行详细配置！

编辑项目根目录下的dirmap.conf，进行配置

dirmap.conf配置详解

```
#递归扫描处理配置
[RecursiveScan]
#是否开启递归扫描:关闭:0;开启:1
conf.recursive_scan = 0
#遇到这些状态码，开启递归扫描。默认配置[301,403]
conf.recursive_status_code = [301,403]
#URL超过这个长度就退出扫描
conf.recursive_scan_max_url_length = 60
#这些后缀名不递归扫
conf.recursive_blacklist_exts = ["html",'htm','shtml','png','jpg','webp','bmp','js','css','pdf','ini','mp3','mp4']
#设置排除扫描的目录。默认配置空。其他配置：e.g:['/test1','/test2']
#conf.exclude_subdirs = ['/test1','/test2']
conf.exclude_subdirs = ""

#扫描模式处理配置(4个模式，1次只能选择1个)
[ScanModeHandler]
#字典模式:关闭:0;单字典:1;多字典:2
conf.dict_mode = 1
#单字典模式的路径
conf.dict_mode_load_single_dict = "dict_mode_dict.txt"
#多字典模式的路径，默认配置dictmult
conf.dict_mode_load_mult_dict = "dictmult"
#爆破模式:关闭:0;开启:1
conf.blast_mode = 0
#生成字典最小长度。默认配置3
conf.blast_mode_min = 3
#生成字典最大长度。默认配置3
conf.blast_mode_max = 3
#默认字符集:a-z。暂未使用。
conf.blast_mode_az = "abcdefghijklmnopqrstuvwxyz"
#默认字符集:0-9。暂未使用。
conf.blast_mode_num = "0123456789"
#自定义字符集。默认配置"abc"。使用abc构造字典
conf.blast_mode_custom_charset = "abc"
#自定义继续字符集。默认配置空。
conf.blast_mode_resume_charset = ""
#爬虫模式:关闭:0;开启:1
conf.crawl_mode = 0
#用于生成动态敏感文件payload的后缀字典
conf.crawl_mode_dynamic_fuzz_suffix = "crawl_mode_suffix.txt"
#解析robots.txt文件。暂未实现。
conf.crawl_mode_parse_robots = 0
#解析html页面的xpath表达式
conf.crawl_mode_parse_html = "//*/@href | //*/@src | //form/@action"
#是否进行动态爬虫字典生成。默认配置1，开启爬虫动态字典生成。其他配置：e.g:关闭:0;开启:1
conf.crawl_mode_dynamic_fuzz = 1
#Fuzz模式:关闭:0;单字典:1;多字典:2
conf.fuzz_mode = 0
#单字典模式的路径。
conf.fuzz_mode_load_single_dict = "fuzz_mode_dir.txt"
#多字典模式的路径。默认配置:fuzzmult
conf.fuzz_mode_load_mult_dict = "fuzzmult"
#设置fuzz标签。默认配置{dir}。使用{dir}标签当成字典插入点，将http://target.com/{dir}.php替换成http://target.com/字典中的每一行.php。其他配置：e.g:{dir};{ext}
#conf.fuzz_mode_label = "{ext}"
conf.fuzz_mode_label = "{dir}"

#处理payload配置。暂未实现。
[PayloadHandler]

#处理请求配置
[RequestHandler]
#自定义请求头。默认配置空。其他配置：e.g:test1=test1,test2=test2
#conf.request_headers = "test1=test1,test2=test2"
conf.request_headers = ""
#自定义请求User-Agent。默认配置chrome的ua。
conf.request_header_ua = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
#自定义请求cookie。默认配置空，不设置cookie。其他配置e.g:cookie1=cookie1; cookie2=cookie2;
#conf.request_header_cookie = "cookie1=cookie1; cookie2=cookie2"
conf.request_header_cookie = ""
#自定义401认证。暂未实现。因为自定义请求头功能可满足该需求(懒XD)
conf.request_header_401_auth = ""
#自定义请求方法。默认配置get方法。其他配置：e.g:get;head
#conf.request_method = "head"
conf.request_method = "get"
#自定义每个请求超时时间。默认配置3秒。
conf.request_timeout = 3
#随机延迟(0-x)秒发送请求。参数必须是整数。默认配置0秒，无延迟。
conf.request_delay = 0
#自定义单个目标，请求协程线程数。默认配置30线程
conf.request_limit = 30
#自定义最大重试次数。暂未实现。
conf.request_max_retries = 1
#设置持久连接。是否使用session()。暂未实现。
conf.request_persistent_connect = 0
#302重定向。默认False，不重定向。其他配置：e.g:True;False
conf.redirection_302 = False
#payload后添加后缀。默认空，扫描时，不添加后缀。其他配置：e.g:txt;php;asp;jsp
#conf.file_extension = "txt"
conf.file_extension = ""

#处理响应配置
[ResponseHandler]
#设置要记录的响应状态。默认配置[200]，记录200状态码。其他配置：e.g:[200,403,301]
#conf.response_status_code = [200,403,301]
conf.response_status_code = [200]
#是否记录content-type响应头。默认配置1记录
#conf.response_header_content_type = 0
conf.response_header_content_type = 1
#是否记录页面大小。默认配置1记录
#conf.response_size = 0
conf.response_size = 1
#是否自动检测404页面。默认配置True，开启自动检测404.其他配置参考e.g:True;False
#conf.auto_check_404_page = False
conf.auto_check_404_page = True
#自定义匹配503页面正则。暂未实现。感觉用不着，可能要废弃。
#conf.custom_503_page = "page 503"
conf.custom_503_page = ""
#自定义正则表达式，匹配页面内容
#conf.custom_response_page = "([0-9]){3}([a-z]){3}test"
conf.custom_response_page = ""
#跳过显示页面大小为x的页面，若不设置，请配置成"None"，默认配置“None”。其他大小配置参考e.g:None;0b;1k;1m
#conf.skip_size = "0b"
conf.skip_size = "None"

#代理选项
[ProxyHandler]
#代理配置。默认设置“None”，不开启代理。其他配置e.g:{"http":"http://127.0.0.1:8080","https":"https://127.0.0.1:8080"}
#conf.proxy_server = {"http":"http://127.0.0.1:8080","https":"https://127.0.0.1:8080"}
conf.proxy_server = None

#Debug选项
[DebugMode]
#打印payloads并退出
conf.debug = 0

#update选项
[CheckUpdate]
#github获取更新。暂未实现。
conf.update = 0
```
