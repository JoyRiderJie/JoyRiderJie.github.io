日志定义
日志是将信息写入日志文件或输出到控制台的过程，日志文件包含有关在操作系统，软件或通信中发生的各种事件的信息。
日志目的

1. **问题诊断**：当系统出现问题时，日志可以帮助快速定位问题的原因。
2. **系统监控**：通过日志可以监控系统的运行状态，了解系统是否正常运行。
3. **安全审计**：日志记录了用户的活动，有助于进行安全审计和追踪不当行为。
4. **性能分析**：日志可以提供系统性能的相关信息，帮助分析性能瓶颈。
5. **合规性记录**：某些行业标准或法规要求记录操作日志，以满足合规性要求。
6. **用户行为分析**：通过分析用户的行为日志，可以了解用户偏好，优化产品或服务。
7. **系统维护**：日志记录了系统变更和维护的历史，便于未来的系统维护。

记录内容
日志详细记录的关键事件包括输入验证失败、身份验证和授权过程中的异常、应用程序错误、配置的任何变更以及应用程序的启动与关闭情况等信息。然而，必须严格遵守数据保护原则，避免记录以下敏感信息：应用程序源代码、会话标识符、访问令牌、个人敏感数据、密码、数据库连接字符串、加密密钥、银行账户信息以及持卡人数据。这些信息的泄露可能会对系统安全和用户隐私造成严重威胁。
日志级别

- 危急（Critical）：级别最高，用于记录最严重的错误，通常意味着系统或应用程序无法继续运行。例如，应用程序崩溃或关键服务不可用。
- 错误（Error）：用于记录运行时错误，这些错误可能不会立即导致系统停止，但需要被解决。例如，文件写入失败或数据库连接错误。
- 警告（Warning）：用于记录潜在问题，这些可能不会立即影响系统运行，但需要被注意。例如，资源使用接近上限或配置错误。
- 信息（Info）：用于记录一般性信息，如程序的启动、正常运行状态或重要的生命周期事件。
- 调试（Debug）：用于记录详细的技术信息，主要用于开发过程中的调试。这些信息对于开发者理解程序的内部状态和流程非常有用。

应用
直接写入日志文件
```python
def test_log():
    # 设置日志文件路径和日志级别
    logging.basicConfig(filename='./log/test.log', level=logging.DEBUG)
    # 输出调试信息
    logging.debug('This is a debug message')
    # 输出普通信息
    logging.info('This is an info message')
    # 输出警告信息
    logging.warning('This is a warning message')
    # 输出错误信息
    logging.error('This is an error message')
    # 输出严重错误信息
    logging.critical('This is a critical message')
```
注意这种方法无法在控制台中显示日志信息。
下面是对CRITICAL:root:This is a critical message这条日志信息的解释：

- CRITICAL: 这是日志级别，表示这条日志消息的严重性最高。使用`logging.CRITICAL`可以记录危急级别的日志信息。
- root: 这是日志记录器的名称。在Python中，默认的日志记录器名称是`root`。开发者也可以创建自定义的日志记录器。
- This is a critical message: 这是实际的日志消息内容。

如果需要更改日志记录器名称可参考下面代码
```python
# 配置日志记录器，设置日志级别为CRITICAL
logging.basicConfig(level=logging.CRITICAL)
# 创建日志记录器实例
logger = logging.getLogger('root')
# 记录一条危急级别的日志消息
logger.critical('This is a critical message')
```
写入日志文件也输出到控制台
```python
def test_log():
    # 获取名为'dev'的logger
    logger = logging.getLogger('dev')
    # 设置logger的日志级别为INFO
    logger.setLevel(logging.INFO)

    # 创建一个文件处理器，将日志写入LOG_PATH指定的文件
    fileHandler = logging.FileHandler(LOG_PATH)
    # 设置文件处理器的日志级别为INFO
    fileHandler.setLevel(logging.WARNING)

    # 创建一个控制台处理器，将日志输出到控制台
    consoleHandler = logging.StreamHandler()
    # 设置控制台处理器的日志级别为INFO
    consoleHandler.setLevel(logging.WARNING)

    # 将文件处理器添加到logger
    logger.addHandler(fileHandler)
    # 将控制台处理器添加到logger
    logger.addHandler(consoleHandler)

    # 输出一条INFO级别的日志信息
    logger.info('information message')
    # 输出一条WARNING级别的日志信息
    logger.warning('warning message')
```
处理器是一个专门的组件，它的任务是根据日志信息的严重性级别，将这些信息分发到预定的目的地。这种分发遵循一种层级传播模式，类似于日志级别的扩散。如果一个记录器没有被赋予特定的处理器，系统将自动在其层级链上向上追溯，寻找可用的处理器。此外，处理器可以通过调用addHandler()方法被附加到记录器上，从而实现日志信息的管理和分发。
示例中的文件处理程序FileHandler将日志发送到test.log文件。
`FileHandler`将日志发送到`test.log`文件。
```python
consoleHandler = logging.StreamHandler()
```
`StreamHandler`将日志发送到流。 如果未指定流，则使用`sys.stderr`。
```python
logger.addHandler(fileHandler)
```
该处理程序将通过`addHandler()`添加到记录器。
日志内容格式化输出到控制台
```python
def test_log():
    # 获取名为'dev'的logger
    logger = logging.getLogger('dev')
    # 设置logger的日志级别为INFO
    logger.setLevel(logging.INFO)

    # 创建一个文件处理器，将日志写入LOG_PATH指定的文件
    fileHandler = logging.FileHandler(LOG_PATH)
    # 设置文件处理器的日志级别为INFO
    fileHandler.setLevel(logging.WARNING)

    # 创建一个控制台处理器，将日志输出到控制台
    consoleHandler = logging.StreamHandler()
    # 设置控制台处理器的日志级别为INFO
    consoleHandler.setLevel(logging.WARNING)

    # 将文件处理器添加到logger
    logger.addHandler(fileHandler)
    # 将控制台处理器添加到logger
    logger.addHandler(consoleHandler)

    # 设置日志格式
    formatter = logging.Formatter('%(asctime)s  %(name)s  %(levelname)s: %(message)s')
    # 将格式应用到控制台处理器
    consoleHandler.setFormatter(formatter)
    # 将格式应用到文件处理器
    fileHandler.setFormatter(formatter)

    # 输出一条INFO级别的日志信息
    logger.info('information message')
    # 输出一条WARNING级别的日志信息
    logger.warning('warning message')
```
该示例创建一个控制台记录器，并将格式化程序添加到其处理程序。
```python
formatter = logging.Formatter('%(asctime)s  %(name)s  %(levelname)s: %(message)s')
```
格式化程序已创建。 它包括日期时间，记录器名称，记录级别名称和记录消息。
```python
consoleHandler.setFormatter(formatter)
```
格式化程序通过`setFormatter()`设置为处理程序。
```python
$ formatter.py
2019-03-28 14:53:27,446  dev  INFO: information message
```
写入日志文件也输出到控制台
```python
def make_print_to_file(path='./'):
    '''
    path， it is a path for save your log about fuction print
    example:
    use  make_print_to_file()   and the   all the information of funtion print , will be write in to a log file
    :return:
    '''
    class Logger(object):
        def __init__(self, filename="Default.log", path="./"):
            self.terminal = sys.stdout
            self.log = open(os.path.join(path, filename), "w", encoding='utf8', )

        def write(self, message):
            self.terminal.write(message)
            self.log.write(message)

        def flush(self):
            pass

    fileName = datetime.datetime.now().strftime('%Y_%m_%d')
    sys.stdout = Logger(fileName + '.log', path=path)
    return fileName
```
这个函数可以在调用时写入日志，因为它通过重定向 `sys.stdout` 来捕获所有输出到控制台的打印操作，并将这些输出同时写入到一个日志文件中。这样，开发者就可以在不修改现有代码的情况下，获得所有打印信息的日志记录。
每次文件调用print函数时，就会将输出重定向到文件中。
```python
def tes1():
    now_time = make_print_to_file('./log')
    print(f'WARNING:{now_time} test1')
    print(f'INFO:{now_time}    test2')
    print(f'Error:{now_time}   test3')
```


