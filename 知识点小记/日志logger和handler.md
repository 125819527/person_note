# 日志logger和handler

## 1.典型的日志记录方法

```
创建logger
创建handler
定义formatter
给handler添加formatter
给logger添加handler
```

## 2.代码举例

```python
logger = logging.getLogger(str(level)) #获取对象,参数为name，name一样的情况，logger对象也一样，避免传递就可以使用相同logger，此处为str(level)
BASIC_FORMAT = "%(asctime)s:%(levelname)s:%(message)s"
DATE_FORMAT = '%Y-%m-%d %H:%M:%S'
formatter = logging.Formatter(BASIC_FORMAT, DATE_FORMAT)#定义格式
chlr = logging.StreamHandler()  # 输出到控制台的handler
chlr.setFormatter(formatter)	#格式设置
chlr.setLevel('INFO')	#设置保存的等级
logger.addHandler(chlr)	#对logger添加handler
logger.addHandler(handlers[level])
logger.setLevel(level)


```

常用的格式设置

```python
%(name)s: 打印收集器名称
%(levelno)s: 打印日志级别的数值
%(levelname)s: 打印日志级别名称
%(pathname)s: 打印当前执行程序的路径，其实就是sys.argv[0]
%(filename)s: 打印当前执行程序名
%(funcName)s: 打印日志的当前函数
%(lineno)d: 打印日志的当前行号
%(asctime)s: 打印日志的时间
%(thread)d: 打印线程ID
%(threadName)s: 打印线程名称
%(process)d: 打印进程ID
%(message)s: 打印日志信息
```

出去控制台的流向，还可以设置TimedRotatingFileHandler，效果为指定时间间隔后创建一个log文档

```python
handlers[level] = TimedRotatingFileHandler(path, when="D",
                                           backupCount=60,
                                           encoding='utf-8',
                                           interval=1)
handlers[level].suffix = "%Y-%m-%d.log"  #log文件加入后缀，例如：filename="test"， suffix="%Y-%m-%d.log"，生成的文件名为test.2020-08-01.log
handlers[level].extMatch = r"^\d{4}-\d{2}-\d{2}.log$" #确保后缀的suffix必须为该正则匹配式
handlers[level].extMatch = re.compile(handlers[level].extMatch)


#ptah 表示输出的方向,log文件路径
#when 相隔时间的单位 此处为天,S - Seconds,M - Minutes,H - Hours,D - Days
#backupcount 表示存储的log文档数量
#interval 相隔时间，此处与when配合表示相隔一天创建一个文档
```

