# 分割没有规则分隔符的字符串
有些情况，需要分割字符串，用简单的split函数就可以了，但是，有些情况下，分隔符有多个，这时，需要用另外的方法：re.split

```
>>> line = 'asdf fjdk; afed, fjek,asdf, foo'
>>> import re
>>> re.split(r'[;,\s]\s*', line)
['asdf', 'fjdk', 'afed', 'fjek', 'asdf', 'foo']
```

__需要注意：不能使用捕获组的正则表达式，使用了会导致分隔符也会在结果里：__

```
>>> fields = re.split(r'(;|,|\s)\s*', line)
>>> fields
['asdf', ' ', 'fjdk', ';', 'afed', ',', 'fjek', ',', 'asdf', ',', 'foo']
>>>
```


# 去掉字符串中不需要的字符

1. 去掉字符串两头的字符： strip, lstrip, rstrip
```
>>> # Whitespace stripping
>>> s = ' hello world \n'
>>> s.strip()
'hello world'
>>> s.lstrip()
'hello world \n'
>>> s.rstrip()
' hello world'
>>>
>>> # Character stripping
>>> t = '-----hello====='
>>> t.lstrip('-')
'hello====='
>>> t.strip('-=')
'hello'
>>>
```

2. 去掉中间字符：replace, re.sub
```
>>> s = ' hello world \n'
>>> s = s.strip()
>>> s
'hello world'
>>>
>>> s.replace(' ', '')
'helloworld'
>>> import re
>>> re.sub('\s+', ' ', s)
'hello world'
>>>
```

#  对齐字符串
使用format或ljust, rjust, center方法：
```
>>> text = 'Hello World'
>>> text.ljust(20)
'Hello World '
>>> text.rjust(20)
' Hello World'
>>> text.center(20)
' Hello World '
>>>


>>> text.rjust(20,'=')
'=========Hello World'
>>> text.center(20,'*')
'****Hello World*****'
>>>


>>> format(text, '>20')
' Hello World'
>>> format(text, '<20')
'Hello World '
>>> format(text, '^20')
' Hello World '
>>>


>>> '{:>10s} {:>10s}'.format('Hello', 'World')
' Hello World'
>>>

```


# 连接字符串
使用join(), format(), +, 'xxx''xxx'


# 在指定列数显示字符串：
```
s = "Look into my eyes, look into my eyes, the eyes, the eyes, \
the eyes, not around the eyes, don't look around the eyes, \
look into my eyes, you're under."



>>> import textwrap
>>> print(textwrap.fill(s, 70))
Look into my eyes, look into my eyes, the eyes, the eyes, the eyes,
not around the eyes, don't look around the eyes, look into my eyes,
you're under.


>>> print(textwrap.fill(s, 40))
Look into my eyes, look into my eyes,
the eyes, the eyes, the eyes, not around
the eyes, don't look around the eyes,
look into my eyes, you're under.


>>> print(textwrap.fill(s, 40, initial_indent=' '))
Look into my eyes, look into my
eyes, the eyes, the eyes, the eyes, not
around the eyes, don't look around the
eyes, look into my eyes, you're under.


>>> print(textwrap.fill(s, 40, subsequent_indent=' '))
Look into my eyes, look into my eyes,
the eyes, the eyes, the eyes, not
around the eyes, don't look around
the eyes, look into my eyes, you're
under.
```


