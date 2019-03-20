编辑并执行文件
=====


## MaixPy 内置了文件系统

支持 Flash 使用的 `SPIFFS`（目前不支持创建目录）， 默认分配了 `3MB` 给 `SPIFF`（从`flash` `0xD00000`地址开始 `3M`）， 开机自动挂在到 `/flash` 目录下

也支持 FAT 格式的 SD （TF）卡 ，开机自动挂在到 `/sd` 目录下

需要注意的是， 根目录只是用来挂载 Flash 或者 SD 卡， 具体的文件在 `/flash` 或者 `/sd` 目录下

## 为什么需要编辑并执行文件

在前面 [点亮 LED](led_blink.md) 的实验中，我们直接在终端里面敲代码来一句一句执行，这样也简单方便，我们输入命令后会立即执行并及时得到返回的结果，这种交互方式称为 `REPL（Read Eval Print Loop：交互式解释器）`, 
这种方式的好处就是简单方便，使用起来和 Linux 终端十分相似，只是使用的语法换成了 MaixPy（Micropython） 的语法。

 但实际大多数情况下我们更想将脚本写到文件中，然后执行文件，这样我们不用每次都敲代码，减少了很多麻烦


## 编辑并保存文件

在 MaixPy 中， 我们内置了一款编开源编辑器 [Micropython Editor(pye)](https://github.com/robert-hh/Micropython-Editor)

使用 `os.listdir()` 可以查看当前目录下的文件，

使用 `pye("hello.py")` 可以创建文件并进入编辑模式， 快捷键等使用说明可以在[这里查看](https://github.com/robert-hh/Micropython-Editor/blob/master/Pyboard%20Editor.pdf)

比如我们写入代码

```python
print("hello maixpy")
```

然后按 `Ctrl+S` 按 `Enter` 键保存， 按 `Ctrl+Q` 退出编辑

**注意**： 使用这款编辑器对使用的串口工具有一定要求， 必须将 `BackSpace` 按键设置为 `DEL` 功能， 否则按 `BackSpace` 调用的是 `Ctrl+H` 一样的功能（即字符替换）。

Linux 下推荐使用 `minicom`， 需要使用 `sudo minicom -s` 来设置，参考[前面的教程](power_on.md)

Windows 下也一样， 根据自己使用的工具上网搜设置方法， 比如 `xshell` 搜 `xshell如何设置backspace为del` 得到结果：

```
文件→属性→终端→键盘，
把delete和backspace序列改为 ASCII 127即可。
```


## 执行文件

使用 `os.chdir()` 切换当前目录到文件的目录，比如 `os.chdir("/flash")`

### 方法一： `import`

然后执行 `import hello`

即可看到输出 `hello maixpy`

使用此方法简单易用，但是需要注意的是， 目前 `import` 只能使用一次， 如果第二次 `import`， 则文件不会再执行， 如果需要多次执行，建议使用下面的方法

### 方法二： `exec()`

使用 `exec()` 函数来执行

```python
with open("hello.py") as f:
    exec(f.read())

```



