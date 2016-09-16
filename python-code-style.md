Python编码规范 - 小酷内部
=========================
本文主要参考《PEP8 - Python Code Style Guide》与《Google Code Style》。

一致性考虑
----------
本文旨在改善代码的可读性，进而提升代码可维护性。团队代码风格强调一致性，项目、模块或函数保持一致都很重要。

最重要的是知道何时不一致, 有时此规范并不适用。当有疑惑时运用你的最佳判断，参考其他例子并多讨论！

特别注意：不要因为遵守本规范而破坏向后兼容性！

部分可以违背指南情况：
- 遵循指南会降低可读性。
- 与周围其他代码不一致。
- 代码在引入指南完成，暂时没有理由修改。
- 旧版本兼容。

代码布局
--------

#### 空格或Tab?
- 默认选用空格进行缩进。
- Tab仅仅在已经使用tab缩进的代码中为了保持一致性而使用。

#### 缩进
每级缩进用4个空格。

好的用法：
```
# 对准左括号
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# 不对准左括号，但加多一层缩进，以和后面内容区别。                       
def long_function_name(
        var_one, var_two,
        var_three, var_four):
    print var_one

# 悬挂缩进，不需多加一层缩进。
foo = long_function_name(
    var_one, var_two, var_three,
    var_four)
```

不好的用法:
```
# 不对准左括号时，第一行不能有参数。
foo = long_function_name(var_one, var_two,
    var_three, var_four)
    
# 参数的缩进和后续内容缩进不能区别。
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print var_one
```

if语句跨行时，加多一层缩进。
```
if (this_is_one_thing
        and this_is_other_thing):
    do_something()
```

右边括号在一些情况下，可以另起一行，缩进回退。
```
# list, tuple, dict需要多行时，用这种。
my_list = [
    1, 2, 3,
    4, 5, 6,
]

# 当使用这种更清晰的时候，可以这么用。
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)

# 关键字参数（key=value）需要多行时，推荐一行一个参数。
res = dict(
    one=1,
    two=2,
)
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    d='d',
    e='e',
    f='f',
)
```

#### 最大行宽
限制所有行的最大行宽为79字符。

文本长块，比如文档字符串或注释，行长度应限制为72个字符。

续行的首选方法是使用小括号、中括号和大括号反斜线仍可能在适当的时候。其次是反斜杠。比如with语句中：
```
with open('/path/to/some/file/you/want/to/read') as file_1, \
        open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```
类似的还有assert。

注意续行要尽量不影响可读性。比如通常在二元运算符之后续行：
```
class Rectangle(Blob):

    def __init__(self, width, height,
                 color='black', emphasis=None, highlight=0):
        if (width == 0 and height == 0 and
                color == 'red' and emphasis == 'strong' or
                highlight > 100):
            raise ValueError("sorry, you lose")
        if width == 0 and height == 0 and (color == 'red' or
                                           emphasis is None):
            raise ValueError("I don't think so -- values are %s, %s" %
                             (width, height))
        Blob.__init__(self, width, height,
                      color, emphasis, highlight)
```

#### 空行
- 两行空行分割顶层函数和类的定义。
- 类的方法定义用单个空行分割。
- 额外的空行可以必要的时候用于分割不同的函数组，但是要尽量节约使用。
- 额外的空行可以必要的时候在函数中用于分割不同的逻辑块，但是要尽量节约使用。
- Python接 contol-L作为空白符；许多工具视它为分页符，这些要因编辑器而异。

#### 源文件编码
所有的源代码都必须为utf-8编码，并在文件头部加上一行`#coding: utf-8`。

#### 导入
- import使用单独一行。
- 导入始终在文件的顶部，在模块注释和文档字符串之后，在模块全局变量和常量之前。
- 导入顺序如下：标准库，相关的第三方库，本地库。各组的导入较多时，之间要有空行。同一组内，按字母顺序导入。

好的用法：
```
import os
import sys
from subprocess import Popen, PIPE
```

不好的用法:
```
import os, sys
```

推荐绝对路径导入，因为它们通常更可读，而且往往是表现更好的（或至少提供更好的错误消息。）
```
import mypkg.sibling
from mypkg import sibling
from mypkg.sibling import example
```

在绝对路径比较长的情况下，也可以使用相对路径：
```
from . import sibling
from .sibling import example
```

如果和本地名字有冲突，可以这样:
```
import myclass as _myclass
import foo.bar.yourclass
```

如果导入太长时，可以这样：
```
# 另起一行导入。
from flask import Blueprint, request, render_template
from flask import redirect, url_for

# 用括号，与上面缩进原则一样。
from flask import (
    Blueprint, request, render_template,
    redirect, url_for,
)
```

一般情况下，禁止使用通配符导入。

通配符导入(`from <module> import *`)应该避免，因为它不清楚命名空间有哪些名称存，混淆读者和许多自动化的工具。唯一的例外是重新发布对外的API时可以考虑使用（如chiki中，`from chiki.api.const import *`）。

字符串
------
Python中单引号字符串和双引号字符串都是相同的。注意尽量避免在字符串中的反斜杠以提高可读性。三个引号都使用双引号(PEP 257中提到)。

一般情况下，优先使用单引号:
```
text = 'hello world!'

# 字符串中有单引号，可用双引号。
text = "This is Tiger's pen."

# 字符串太长时，也可这么使用(不需要逗号)。
text = (
    'Hello world! '
    'My name is Tiger Tongiht.' 
)
```

表达式和语句中的空格
--------------------
括号里边避免空格：
```
# Yes
spam(ham[1], {eggs: 2})

# No
spam( ham[ 1 ], { eggs: 2 } )
```

逗号，冒号，分号之前避免空格：
```
# Yes
if x == 4: print x, y; x, y = y, x

# No
if x == 4 : print x , y ; x , y = y , x
```

索引操作中的冒号当作操作符处理前后不要留空格，其他如加号的操作符，也不要留空格：
```
# Yes
ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
ham[lower:upper], ham[lower:upper:], ham[lower::step]
ham[lower+offset:upper+offset]

# No
ham[lower + offset:upper + offset]
ham[1: 9], ham[1 :9], ham[1:9 :3]
ham[lower : : upper]
ham[ : upper]
```

函数调用、数组、字典的左括号之前不能有空格：
```
# Yes
spam(1)
dct['key'] = lst[index]

# No
spam (1)
dct ['key'] = lst [index]
```

赋值等操作符前后不能因为对齐而添加多个空格：
```
# Yes
x = 1
y = 2
long_variable = 3

# No
x             = 1
y             = 2
long_variable = 3
```

二元运算符两边放置一个空格：涉及 =、符合操作符 ( += , -=等)、比较( == , < , > , != , <> , <= , >= , in , not in , is , is not )、布尔( and , or , not )。
```
# Yes
i = i + 1
submitted += 1
x = x * 2 - 1
hypot2 = x * x + y * y
c = (a + b) * (a - b)

# No
i=i+1
submitted +=1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

关键字参数和默认值参数的前后不要加空格：
```
# Yes
def complex(real, imag=0.0):
    return magic(r=real, i=imag)

# No
def complex(real, imag = 0.0):
    return magic(r = real, i = imag)
```

函数注释中，=前后要有空格，冒号和"->"的前面无空格，后面有空格。
```
# Yes
def munge(input: AnyStr):
def munge(sep: AnyStr = None):
def munge() -> AnyStr:
def munge(input: AnyStr, sep: AnyStr = None, limit=1000):

# No
def munge(input: AnyStr=None):
def munge(input:AnyStr):
def munge(input: AnyStr)->PosInt:
```

通常不推荐复合语句(Compound statements: 多条语句写在同一行)：
```
# Yes
if foo == 'blah':
    do_blah_thing()
do_one()
do_two()
do_three()

# No
if foo == 'blah': do_blah_thing()
do_one(); do_two(); do_three()
```

尽管有时可以在if/for/while 的同一行跟一小段代码，但绝不要跟多个子句，并尽量避免换行。
```
# No
if foo == 'blah': do_blah_thing()
for x in lst: total += x
while t < 10: t = delay()
```

给点反面教材：
```
# No
if foo == 'blah': do_blah_thing()
else: do_non_blah_thing()

try: something()
finally: cleanup()

do_one(); do_two(); do_three(long, argument,
                             list, like, this)

if foo == 'blah': one(); two(); three()
```

注释
----
与代码自相矛盾的注释比没注释更差，修改代码时要优先更新注释！

一定要考虑注释写给是谁看，有必要才注释，有坑或是特别作用（代码容易让人觉得歧义）。

#### 注释块
注释块通常应用在代码前，并和这些代码有同样的缩进。每行以 '# '(除非它是注释内的缩进文本，注意#后面有空格)。

注释块内的段落用仅包含单个 '#' 的行分割。

#### 行内注释
慎用行内注释(Inline Comments) 节俭使用行内注释。

行内注释是和语句在同一行，至少用两个空格和语句分开。不要这样做：
```
x = x + 1 # Increment x
```

但有时很有必要:
```
x = x + 1 # Compensate for border
```

#### 文档字符串
文档字符串的标准参见：PEP 257。

- 为所有公共模块、函数、类和方法书写文档字符串。非公开方法不一定有文档字符串，建议有注释(出现在 def 行之后)来描述这个方法做什么。
- 更多参考：PEP 257 文档字符串约定。注意结尾的 """ 应该单独成行，例如：

    """Return a foobang
    Optional plotz says to frobnicate the bizbaz first.
    """

- 单行的文档字符串，结尾的 """ 在同一行。

命名技巧
--------

#### 命名原则
- 明确、清晰：在场景下能准确表达意思。
- 简短：用更少的单词表达。

#### 提升可读性的命名（用什么单词）技巧
- 看什么类型用什么词，如函数用动词、或副词，如calcArea计算面积，变量或参数一般用名称，如name名字。
- 看什么作用用什么词，如时间created创建，表示过去某个时间点，如开始时间，可直接用start。
- 同一场景下，如果有歧义，要明确表达:
    - 如开始人数，结束人数，可使用start_users, end_users，在这里，使用users要比count清楚的多。
    - 如参与人数，中奖人数，可使用users, luck_users。
    - 再如出现两个实例类型相同容易混时，不要用user, user2，可以使用user, another。
- 适当的时候使用缩写，如desc描述，但一定要常见或惯用的单词才能用。

命名约定
--------
#### 最重要的原则
用户可见的API命名应遵循使用约定而不是实现。

#### 描述：命名风格
有多种命名风格：
    - b(单个小写字母)
    - B(单个大写字母)
    - lowercase(小写串)
    - lower_case_with_underscores(带下划线的小写)
    - UPPERCASE(大写串)
    - UPPER_CASE_WITH_UNDERSCORES(带下划线的大写串)
    - CapitalizedWords(首字母大写的单词串或驼峰缩写）

注意: 使用大写缩写时，缩写使用大写字母更好。故 HTTPServerError 比 HttpServerError 更好。
    - mixedCase(混合大小写，第一个单词是小写)
    - Capitalized_Words_With_Underscores（带下划线，首字母大写，丑陋）

还有一种风格使用短前缀分组名字。这在Python中不常用， 但出于完整性提一下。例如，os.stat()返回的元组有st_mode, st_size, st_mtime等等这样的名字(与POSIX系统调用结构体一致)。

X11库的所有公开函数以X开头, Python中通常认为是不必要的，因为属性和方法名有对象作前缀，而函数名有模块名为前缀。

下面讲述首尾有下划线的情况:
    - _single_leading_underscore:(单前置下划线): 弱内部使用标志。 例如"from M import " 不会导入以下划线开头的对象。
    - single_trailing_underscore_(单后置下划线): 用于避免与 Python关键词的冲突。 例如：`Tkinter.Toplevel(master, class_='ClassName')`
    - __double_leading_underscore(双前置下划线): 当用于命名类属性，会触发名字重整。 (在类FooBar中，__boo变成 _FooBar__boo)。
    - __double_leading_and_trailing_underscore__(双前后下划线)：用户名字空间的魔法对象或属性。例如:__init__ , __import__ or __file__，不要自己发明这样的名字。

#### 避免采用的名字
决不要用字符'l'(小写字母el)，'O'(大写字母oh)，或 'I'(大写字母eye) 作为单个字符的变量名。一些字体中，这些字符不能与数字1和0区别。用'L' 代替'l'时。

#### 包和模块名
模块名要简短，全部用小写字母，可使用下划线以提高可读性。包名和模块名类似，但不推荐使用下划线。

另外有些模块底层用C或C++ 书写，并有对应的高层Python模块，C/C++模块名有一个前置下划线 (如：_socket)。

#### 类名
遵循CapWord（大驼峰式）。接口需要文档化并且可以调用时，可能使用函数的命名规则。注意大部分类名是单个单词（或两个），CapWord只适用于异常名称和内置的常量。

#### 异常名
如果确实是错误，需要在类名添加后缀 "Error"。

#### 全局变量名
变量尽量只用于模块内部，约定类似函数。

对设计为通过`from M import *` 来使用的模块，应采用 __all__ 机制来防止导入全局变量；或者为全局变量加一个前置下划线。

#### 函数名
函数名应该为小写，必要时可用下划线分隔单词以增加可读性。 mixedCase(混合大小写)仅被允许用于兼容性考虑(如: threading.py)。

#### 函数和方法的参数
- 实例方法第一个参数是 'self'。
- 类方法第一个参数是 'cls'。
- 如果函数的参数名与保留关键字冲突，通常在参数名后加一个下划线。

#### 方法名和实例变量
- 同函数命名规则。
- 非公开方法和实例变量增加一个前置下划线。
- 为避免与子类命名冲突，采用两个前置下划线来触发重整。类Foo属性名为__a， 不能以 Foo.__a访问。(执著的用户还是可以通过Foo._Foo__a。) 通常双前置下划线仅被用来避免与基类的属性发生命名冲突。

#### 继承设计
考虑类的方法和实例变量(统称为属性)是否公开。如果有疑问，选择不公开；把其改为公开比把公开属性改为非公开要容易。

公开属性可供所有人使用，并通常向后兼容。非公开属性不给第三方使用、可变甚至被移除。

这里不使用术语"private"， Python中没有属性是真正私有的。

另一类属性是子类API(在其他语言中通常称为 "protected")。 一些类被设计为基类，可以扩展和修改。

谨记这些Python指南：

    * 公开属性应该没有前导下划线。
    * 如果公开属性名和保留关键字冲突，可以添加后置下划线
    * 简单的公开数据属性，最好只公开属性名，没有复杂的访问/修改方法，python的Property提供了很好的封装方法。
    * 如果不希望子类使用的属性，考虑用两个前置下划线(没有后置下划线)命名。

#### 公共和内部接口
- 任何向后兼容的保证只适用于公共接口。
- 文档化的接口通常是公共的，除非明说明是临时的或为内部接口、其他所有接口默认是内部的。
- 为了更好地支持内省，模块要在__all__属性列出公共API。
- 内部接口要有前置下划线。
- 如果命名空间(包、模块或类)是内部的，里面的接口也是内部的。
- 导入名称应视为实现细节。其他模块不能间接访名字，除非在模块的API文档中明确记载，如os.path中或包的__init__暴露了子模块。

编程建议
--------
考虑多种Python实现(PyPy, Jython, IronPython,Pyrex, Psyco, 等等)。例如，CPython对a+=b或a=a+b等语句有高效的实现，但在Jython中运行很慢，尽量改用.join()。

None比较用'is'或'is not'，不要用等号。注意"if x is not None" 与"if x" 的区别。

用"is not"代替"not ... is"。前者的可读性更好。
```
# Yes
if foo is not None

# No
if not foo is None
```

使用基于类的异常。

比较排序操作最好是实现所有六个操作，而不是代码中实现比较逻辑。`functools.total_ordering()`装饰符可以生成缺失的比较方法。
```
__eq__，__ne__，__lt__，__lt__，__gt__，____
```

PEP207 比较标准表明反射规则由Python完成。因此解释器可能会交换参数的位置，比如替换y > x为x < y，所以有必要实现这5种方法。

使用函数定义def代替lambda赋值给标识符：
```
# Yes
def f(x): 
    return 2*x

# No
f = lambda x: 2*x
```

前者更适合回调和字符串表示。

异常类继承自Exception，而不是BaseException。

源于异常，而不是BaseException例外。从BaseException直接继承的例外情况追赶他们几乎总是错误的事情做保留。

要设计基于层次的异常，捕捉到需要的异常，而不是异常引发的位置。能回答：“出了什么问题？”，而不是仅仅指出“问题发生”(更多参考：PEP3151 重构OS和IO异常层次）

适当使用异常链。在Python3中"raise X from Y"明确表示更换且保留了原来的traceback。

替换内部异常(在Python2: "raise X"或"raise X from None")时，确保相关细节转移到新的异常（如转换KeyError为AttributeError保存属性名，或在新的异常中嵌入原始异常)。

Python2中用" raise ValueError('message')"代替"raise ValueError, 'message'"后者不兼容Python3语法。前者续行方便。

捕获异常时尽量指明具体异常，而不是空"except:"子句。比如：
```
# Yes
try:
    import platform_specific_module
except ImportError:
    platform_specific_module = None
```

空"except:"子句(相当于except Exception)会捕捉SystemExit和KeyboardInterrupt异常，难以用Control-C中断程序，并可掩盖其他问题。如果 你捕捉信号错误之外所有的异常，使用"except Exception"。

空"except:"子句适用的情况两种情况：

    a, 打印出或记录了traceback，至少让用户将知道已发生错误。 b, 代码需要做一些清理工作，并用 raise转发了异常。这样try...finally可以捕捉到它。

Python 2.6以后建议用as显示绑定异常名：
```
# Yes
try:
    process_data()
except Exception as exc:
    raise DataProcessingFailedError(str(exc))
```

这样才能兼容Python3语法并避免歧义。

捕捉操作系统错误时，建议使用Python 3.3引入显式异常层次，支持内省errno值。

此外所有try/except子句的代码要尽可的少，以免屏蔽其他的错误。
```
# Yes
try:
    value = collection[key]
except KeyError:
    return key_not_found(key)
else:
    return handle_value(value)

# No
try:
    # 太泛了!
    return handle_value(collection[key])
except KeyError:
    # 会捕捉到handle_value()中的KeyError
    return key_not_found(key)
```

本地资源建议使用with语句，以确保即时清理。当然try / finally语句也是可以接受的。

上下文管理器在做获取和释放资源之外的事情时，应通过独立的函数或方法。例如：
```
# Yes
with conn.begin_transaction():
    do_stuff_in_transaction(conn)

# No
with conn:
    do_stuff_in_transaction(conn)
```

后者指明enter和exit方法。

函数或者方法在没有返回时要明确返回None。
```
# Yes
def foo(x):
    if x >= 0:
        return math.sqrt(x)
    else:
        return Nonedef bar(x):
    if x < 0:
        return None
    return math.sqrt(x)# Nodef foo(x):
    if x >= 0:
        return math.sqrt(x)def bar(x):
    if x < 0:
        return
    return math.sqrt(x)
```

使用字符串方法而不是string模块。python 2.0以后字符串方法总是更快，且Unicode字符串相同的API。

使用.startswith()和.endswith()代替字符串切片来检查前缀和后缀。startswith()和endswith()更简洁，利于减少错误。例如：
```
# Yes
if foo.startswith('bar'):

# No
if foo[:3] == 'bar':
```

使用isinstance()代替对象类型的比较：
```
# Yes
if isinstance(obj, int):

# No
if type(obj) is type(1):
```

检查是否是字符串时，注意Python 2中str和unicode有公共的基类:

if isinstance(obj, basestring): 在 Python 2.2 中，types 模块为此定义了 StringTypes 类型，例如：
```
# Yes
if isinstance(obj, basestring):
```

Python3中Unicode和basestring的不再存在(只有str)和字节对象不再是字符串(是整数序列)

对序列(字符串、列表 、元组), 空序列为false:
```
# Yes
if not seq:
    pass
if seq:
    pass

# No
if len(seq):
    pass
if not len(seq):
    pass
```

字符串后面不要有大量拖尾空格。

不要用 == 进行布尔比较
```
# Yes
if greeting:
    pass

# No
if greeting == True
    pass
if greeting is True: # Worse
    pass
```

Python标准库不使用的功能注释，这块有待用户去发现和体验有用的注释风格。下面有些第三方的建议(略)。
