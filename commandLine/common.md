## .bashrc 中使用 alias(别名)

##### 使用 mvn，但是不将 mvn 放入 windows 环境变量的情况下怎么直接在命令行使用 mvn

1. 下载 mvn 解压
2. 将解压后的文件放入你想放置的文件夹，我放置路径为'C:\Program Files\apache-maven-3.8.6\bin'
3. 在 home 目录打开.bashrc `vi .bashrc`
4. 使用 vim 编辑器插入`alias mvn='C:/"Program Files"/apache-maven-3.8.6/bin/mvn'`

这样就不需要配置环境变量，可以直接在 bash 终端中使用 mvn 了
