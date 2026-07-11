## launch.json编写

configurations 是数组，原因是可以编写多个调试配置，数组的每一个元素都是一个调试配置

每一个元素都是一个对象，这个对象必须要有type、name、request

configurations 数组中参数解释：
- type：编程语言，例如 go、java、c
- name：这个配置的名称，自定义
- request：指定调试模式，vscode只有launch、attach两种模式
  - 程序有两种启动方式：
    - 开发者手动输入启动命令，如：node app.js / go run app.go 这种方式不支持打断点
    - launch模式，vscode会根据配置生成启动命令并运行，给程序添加调试器，然后运行，这种方式支持打断点
    - attach模式，attach模式可以生成一个调试器，然后挂载到一个正在运行的程序上，让正在运行的程序支持调试
- program:要调试的程序的路径
- args:传递给程序的命令行参数
- cwd:程序的当前工作目录
- preLaunchTask:在启动调试前要运行的任务，通常是编译任务，需要与 tasks.json 文件中的任务名称匹配
- console:调试控制台类型，如"internalConsole" (内部控制台) 或"externalTerminal" (外部终端)

## task.json 编写

tasks.json 文件是一个JSON 文件，用于定义各种任务。

tasks 数组中包含一个或多个任务定义。

每个任务定义包含以下主要属性：
- label: 任务的名称，用于在VS Code 中标识和显示任务。
- type: 任务的类型，可以是 shell (使用shell 执行) 或 process (使用进程执行)。
- command: 要执行的命令，例如 gcc、make、npm 等。
- args: 传递给命令的参数，可以是一个字符串数组。
- options: 任务的选项，例如 cwd (当前工作目录)。
- problemMatcher: 用于解析命令输出，检测和报告错误的匹配器。
- group: 定义任务所属的组，例如 build (构建任务)。
- isDefault: 指定是否为默认构建任务。