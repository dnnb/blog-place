---

title: 'python之禅'
date: 2018-6-11 
categories: python
description: 在Python shell中输入import this就会显示Tim Peters的《The Zen of python》,读一读Python之禅,你就会知道Python为什么如此迷人！然后再查看一下this模块源码，一起来回忆一下加密与解密...的简单实现方式。   

---

### 《python之禅》及翻译

在Python shell中输入`import this`:

```
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!

```

中文翻译如下：

```
	《python之禅》，Tim Peters著

	优美胜于丑陋
	明了胜于晦涩
	简洁胜于复杂 
	复杂胜于凌乱
	扁平胜于嵌套 
	间隔胜于紧凑
	可读性很重要 
	即便假借特例的实用性之名，也不可违背这些规则 
	不要包容所有错误，除非你确定需要这样做 
	当存在多种可能，不要尝试去猜测 
	而是尽量找一种，最好是唯一一种明显的解决方案
	虽然这并不容易，因为你不是 Python 之父
	做也许好过不做，但不假思索就动手还不如不做
	如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然
	命名空间是一种绝妙的理念，我们应当多加利用
```




再在Python shell中输入`this.__file__`找到this.py文件的路径:

```python
>>> this.__file__
'e:\\python\\Lib\\this.py'
```

根据路径打开this.py文件，显示如下：

```python

s = """Gur Mra bs Clguba, ol Gvz Crgref

Ornhgvshy vf orggre guna htyl.
Rkcyvpvg vf orggre guna vzcyvpvg.
Fvzcyr vf orggre guna pbzcyrk.
Pbzcyrk vf orggre guna pbzcyvpngrq.
Syng vf orggre guna arfgrq.
Fcnefr vf orggre guna qrafr.
Ernqnovyvgl pbhagf.
Fcrpvny pnfrf nera'g fcrpvny rabhtu gb oernx gur ehyrf.
Nygubhtu cenpgvpnyvgl orngf chevgl.
Reebef fubhyq arire cnff fvyragyl.
Hayrff rkcyvpvgyl fvyraprq.
Va gur snpr bs nzovthvgl, ershfr gur grzcgngvba gb thrff.
Gurer fubhyq or bar-- naq cersrenoyl bayl bar --boivbhf jnl gb qb vg.
Nygubhtu gung jnl znl abg or boivbhf ng svefg hayrff lbh'er Qhgpu.
Abj vf orggre guna arire.
Nygubhtu arire vf bsgra orggre guna *evtug* abj.
Vs gur vzcyrzragngvba vf uneq gb rkcynva, vg'f n onq vqrn.
Vs gur vzcyrzragngvba vf rnfl gb rkcynva, vg znl or n tbbq vqrn.
Anzrfcnprf ner bar ubaxvat terng vqrn -- yrg'f qb zber bs gubfr!"""

d = {}
for c in (65, 97):
    for i in range(26):
        d[chr(i+c)] = chr((i+13) % 26 + c)

print "".join([d.get(c, c) for c in s])


```

嗯...

### this.py代码分析

来分析一些这段代码：

- 先是声明了一个字符串变量`s`，字符串是乱的~，从格式上看跟输入`import this`显示的一样，感觉上就是把字符串里的每个字符调换了而已；
- 然后是声明了一个空字典`d`，然后写了两层循环对字典进行赋值，chr（）函数是输入一个整数（0，255）返回其对应的ascii值,65对应的ascii码值是'A',97对应的是'a',97+26-1=122对应的值是'z'；
- 最后是遍历一开始声明的`s`，把`s`中的每个字符作为key去取字典`d`中的value生成一个新的列表，再把这个列表join空字符串""生成一个新的字符串，打印新的字符串，这个新的字符串就是我们输入`import this`显示出来的《python之禅》。

嗯，基本上已经知道是怎么实现的了！

然后打印一下生成后的字典`d`看一下：

```
{'A': 'N', 'C': 'P', 'B': 'O', 'E': 'R',
 'D': 'Q', 'G': 'T', 'F': 'S', 'I': 'V',
 'H': 'U', 'K': 'X', 'J': 'W', 'M': 'Z',
 'L': 'Y', 'O': 'B', 'N': 'A', 'Q': 'D',
 'P': 'C', 'S': 'F', 'R': 'E', 'U': 'H',
 'T': 'G', 'W': 'J', 'V': 'I', 'Y': 'L',
 'X': 'K', 'Z': 'M', 'a': 'n', 'c': 'p',
 'b': 'o', 'e': 'r', 'd': 'q', 'g': 't',
 'f': 's', 'i': 'v', 'h': 'u', 'k': 'x',
 'j': 'w', 'm': 'z', 'l': 'y', 'o': 'b',
 'n': 'a', 'q': 'd', 'p': 'c', 's': 'f',
 'r': 'e', 'u': 'h', 't': 'g', 'w': 'j',
 'v': 'i', 'y': 'l', 'x': 'k', 'z': 'm'}
```

确实是就一个普通的字典。

### this.py模块s字符串生成

经过分析：python之禅是由字符串`s`通过字典`d`的（key,value）映射而来的；那么再写一个字典，由字典`d`的（value,key）组成，应该就能把python之禅变成代码中的字符串`s`了。

把this.py里的代码拿过来改一下：

```python

s = """
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
"""

d = {}
for c in (65, 97):
    for i in range(26):
        # d[chr(i + c)] = chr((i + 13) % 26 + c)    # 这个是原来的
        d[chr((i + 13) % 26 + c)] = chr(i + c)   

print "".join([d.get(c, c) for c in s])

```

结果如下：

```
Gur Mra bs Clguba, ol Gvz Crgref

Ornhgvshy vf orggre guna htyl.
Rkcyvpvg vf orggre guna vzcyvpvg.
Fvzcyr vf orggre guna pbzcyrk.
Pbzcyrk vf orggre guna pbzcyvpngrq.
Syng vf orggre guna arfgrq.
Fcnefr vf orggre guna qrafr.
Ernqnovyvgl pbhagf.
Fcrpvny pnfrf nera'g fcrpvny rabhtu gb oernx gur ehyrf.
Nygubhtu cenpgvpnyvgl orngf chevgl.
Reebef fubhyq arire cnff fvyragyl.
Hayrff rkcyvpvgyl fvyraprq.
Va gur snpr bs nzovthvgl, ershfr gur grzcgngvba gb thrff.
Gurer fubhyq or bar-- naq cersrenoyl bayl bar --boivbhf jnl gb qb vg.
Nygubhtu gung jnl znl abg or boivbhf ng svefg hayrff lbh'er Qhgpu.
Abj vf orggre guna arire.
Nygubhtu arire vf bsgra orggre guna *evtug* abj.
Vs gur vzcyrzragngvba vf uneq gb rkcynva, vg'f n onq vqrn.
Vs gur vzcyrzragngvba vf rnfl gb rkcynva, vg znl or n tbbq vqrn.
Anzrfcnprf ner bar ubaxvat terng vqrn -- yrg'f qb zber bs gubfr!

```

跟this.py里的字符串`s`一致。

### 结论

作者是先写下《python之禅》，然后再对其加密生成密文，再写一个解密算法将密文解密显示原文。作者是个老司机！

### 扩展

《python之禅》作者用的加密算法是一种简易的替换式密码算法叫做ROT13（回转13位），把字母表中的每个字母用其后的第13 个字母来代替，特征是字母表中前半部分字母将被映射到后半部分，而后半部分字母将被映射到前半部分，大小写保持不变；ROT13 也是过去在古罗马开发的凯撒密码的一种变体。

其实这种加密算法早就有现成的轮子，要使用可直接用以下方式：

```
>>> import codecs
>>> codecs.encode('The Zen of Python', 'rot_13')
'Gur Mra bs Clguba'
```

ROT5/13/18/47加密方式：

	ROT5：只对数字进行编码，用当前数字往前数的第5个数字替换当前数字，例如当前为0，编码后变成5，当前为1，编码后变成6，以此类推顺序循环。
	ROT13：只对字母进行编码，用当前字母往前数的第13个字母替换当前字母，例如当前为A，编码后变成N，当前为B，编码后变成O，以此类推顺序循环。
	ROT18：这是一个异类，本来没有，它是将ROT5和ROT13组合在一起，为了好称呼，将其命名为ROT18。
	ROT47：对数字、字母、常用符号进行编码，按照它们的ASCII值进行位置替换，用当前字符ASCII值往前数的第47位对应字符替换当前字符，例如当前为小写字母z，编码后变成大写字母K，当前为数字0，编码后变成符号_。

编码和加密相关的可参考以下链接：
[https://www.cnblogs.com/mq0036/p/6544055.html](https://www.cnblogs.com/mq0036/p/6544055.html)
