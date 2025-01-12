# Modules (Modüller)
Bazı işlevleri kolaylıkla yerine getirmemizi sağlayan birtakım function'ları ve attribute'ları içinde barındıran araçlardır/kütüphanelerdir. Bir fonksiyonu farklı Python dosyalarında kullanabilmek için o fonksiyonu her dosyada tekrardan tanımlamak yerine modül olarak kullanılabilir. Kullanacağınız fonksiyonları bir Python dosyasına tanımladıktan sonra o dosyayı `import` ederek içindeki fonksiyonları kullanabilirsiniz. Yani Python ile yazdığınız her dosya, potansiyel bir modüldür. İmport ettiğin dosya **Module (Modül)** olarak isimlendirilir. Python3'deki standart modüller için [tıklayınız](https://docs.python.org/3/library/).

## `import` Statement
Modülleri içe aktarmak için `import` keyword'ünü kullanırız.
```py
import random # random modülündeki her şeyi içe aktarır.
print(dir(random)) # random ile ilgili bütün fonksiyon ve methodları yazdırır.
```
Bu şekilde import edilen modüller `module_name.function_or_attribute` syntax'ıyla kullanılır. Örnek:
```py
import random
print(random.randint(3, 9)) #  Output: 4
```
Bir modülü, function ya da class gibi bir local scope'da da import edebiliriz. Örnek:
```py
def module_imported():
    import random
```
**Not:** Bir modül dosyasını programınızın içine import edebilmeniz için 3 seçeneğiniz vardır.
1. Modül dosyasıyla program dosyası aynı path'da (dizin, dosyaların konumu.) olmalıdır.
2. Modül dostası Python Lib'de olmalıdır. Bu klasör HP bilgisayarlarda `C:\Users\HP\AppData\Local\Programs\Python\Python39\Lib` path'ında bulunabilir. Buradaki `Python39`, Python sürümünüze göre değişiklik gösterebilir.
3. `sys.path`, Python, import edilmeye çalışılan bir modül dosyasını ararken baktığı path'ların tuttuğu, `sys` modülünde bulunan bir listedir. Modül dosyası ile program dosyası, `D:\main_dosya\modules\modul.py` ve `D:\main_dosya\program\main.py` gibi birbiriyle alakasız iki klasör içindeyse `sys` modülündeki `sys.path` methoduna, modülünüzün bulunduğu path'ı `sys.path.append(r"D:\main_dosya\modules")` örneğindeki gibi eklerseniz, daha sonra o path'da bulunan modülünüzü normal `import modul` şeklinde import edip kullanabilirsiniz. Pylance gibi extension'lar bu yöntemi algılayamayıp, `import modul` kodundaki `modul` modülünü bulamadığı için `Import "modul" could not be resolved` hatası verebilirler. Bunlara aldırmanıza gerek yoktur çünkü kodunuz çalışır. Yine de her ihtimale karşı `print(sys.path)` kodunun döndürdüğü liste'de, `sys.path.append()` ile eklediğiniz path varmı diye kontrol etmekte fayda var.

**Not:** Bir modülü import ettikten sonra, modül dosyasında bir değiliklik yaparsanız (Örneğin yeni bir fonksiyon eklerseniz), modülü import ettiğiniz dosya bu değişiklikleri tanımaz (yani yeni eklediğiniz fonksiyonu kullanamazsınız). Bunun önüne geçmek için, `importlib.reload(modül_adı)` modül methodunu kullanarak modülünüzü **reload** yapabilirsiniz. Modülü tekrardan `import modul` şeklinde import etmek işe yaramaz. `importlib.reload(modül_adı)` kullanın.

## `as` Keyword'ü
Genel kullanım olarak, bir şeyi başka bir isimle kullanmak için `as` keyword'ünden yararlanılır. Bir modülü, programınız içerisinde başka bir isimle kullanmak için `as` keyword'ü kullanılır.
```py
import random as sallama # artık 'random' modülünü 'sallama' adıyla kullanabiliriz.
print(sallama.randint(3, 9)) # Output: 6
```

## `from` Keyword'ü
Bir modüldeki spesifik bir function ya da attribute'ü import etmek için kullanılır. Bu şekilde import edilen function ya da attribute, direkt kullanılabilir. Örnek:
```py
from random import randint # random modülündeki randint() function'ını içe aktarır.
print(randint(3, 9)) # Output: 7
```
İmport ettiğiniz function ya da attribute'ün ismini değiştirmek için:
```py
from random import randint as sayi_sec # random modülündeki randint() function'ını, 'sayi_sec' adıyla içe aktarır.
print(sayi_sec(3, 9)) # Output: 5
```
Bir modülden, birden fazla spesifik function ya da attribute import etmek için:
```py
from os import name, listdir, getcwd # os modülündeki name attribute'ünü, listdir ve getcwd function'larını içe aktarır.
```
Modüldeki her şeyi aktarmak için:
```py
from random import *

print(random()) # Output: 0.4278806407713517
print(uniform(2.965, 3.117)) # Output: 2.995864919051282
print(randint(3,9)) # Output: 8
print(randrange(3,9)) # Output: 7 
```
Modüldeki her şeyi, function ya da class gibi bir local scope'da import etmeye çalışırsak `SyntaxError: import * only allowed at module level` hatası alırız çünkü yıldızlı içe aktarma işlemleri ancak modül seviyesinde, yani global scope'da gerçekleştirilebilir. Örnek:
```py
def module_imported():
    from random import *
```
**Not:** Bu tavsiye edilen bir yöntem değildir çünkü ihtiyacınız olan olmayan her şeyi Python dosyasının içine aktardığınız için performans dostu bir yöntem sayılmaz. Ek olarak, `from modül_adı import *` şeklinde import ettiğiniz modülün function ve attribute'leri ile, sizin yazdığınız function ve attribute'ler çakışabilir. Örnek:
```py
from random import randint

def randint(p1,p2):
    return p1+p2

print(randint(3,9)) # Output: 12
```
```py
def randint(p1,p2):
    return p1+p2

from random import randint

print(randint(3,9)) # Output: 6
```

# Bazı Attribute'ler

## `__all__` Attribute
Python'da `import modul` ile `from modul import *` komutlarının içeri aktardıkları, içinde function ve attribute'ların bulunduğu grup birbirinden farklıdır.
```py
import_modul = ['__builtins__', '__cached__', '__doc__', '__file__', '__fonk7', '__loader__', '__name__', '__package__', '__spec__', '_fonk6', 'fonk1', 'fonk2', 'fonk3', 'fonk4', 'fonk5', 'fonk8_']
form_modul_import =['__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'fonk1', 'fonk2', 'fonk3']
for i in import_modul:
    if not i in form_modul_import: # import_modul'de olup, form_modul_import'de olmayan. 
        print(i, end=" , ") 
```
**Output:**
```
__cached__ , __file__ , __fonk7 , _fonk6 , fonk4 , fonk5 , fonk8_
```
`import modül` de her şeyi içeri import ederken, `from modül import *` da test ve private olanları (yani `_` ile başlayanları) import etmez. Spesifik olarak import etmek istediklerinizi `from modül import _örnek` şeklinde import edebilirsiniz. Tabi böyle yapabilmek için fonksiyonun ismini biliyor olmalısınız. `__all__` , import edilmesini istediğiniz ve istemediğiniz fonksiyonları belirlemenize izin verir. Örneğin `modül.py`'nin en başına `__all__ = ['fonk1', 'fonk2', 'fonk3']` eklerseniz, `from modül import *` yaptığınızda `form_modul_import` listesindekiler import edilir. `__all__ = []` kullanımında, modülün kendi varsayılan fonksiyonları hariç hiçbir fonksiyon içe aktarılmaz.

**Not:** Örneğin Bir fonksiyon yazdınız. Bu fonksiyonu öyle yazıyorsunuz ki, başka birisi bu fonksiyonu sadece kopyala yapıştır yaparak kendi programına entegre edebiliyor. Buna **code reusability** denir. Yani kodların yeniden kullanılabilir özellikte olmasına **code reusability** denir. Bu kodların kolayca test edilebilmesine de **code testability** denir. Daha fazla bilgi için [tıklayınız](https://medium.com/aykiri-yazilimcilar/kaliteli-yazılım-tasarımı-ve-anti-patternler-üzerine-notlar-a8f9ccfb6847)

## `__import__(name, globals=None, locals=None, fromlist=(), level=0)` Attribute
`__import__`, modül adını `name` parametresine girerek, herhangi bir modülü içe aktarmamızı sağlayan bir araçtır. Örneğin `vrb = __import__('random')` yaptıktan sonra random modülünün fonksiyonlarını ve attribute'lerini `vrb.randint(45, 500)` örneğindeki gibi kullanabilirsiniz. Bir variable'a atamadan kullanmak istiyorsanız `__import__('random').randint(45, 500)` şeklinde kullanabilirsiniz. Daha fazla bilgi için [tıklayınız](https://docs.python.org/3/library/functions.html#__import__)

## `__doc__` Attribute
**Ön Bilgi:** Teknik dilde, üç tırnak `""" Falan Filan """` içinde gösterilen karakter dizilerine belge dizisi (docstring) veya belgelendirme dizisi (documentation string) adı verilir.

Modüllerin `__doc__` niteliğini kullanarak, bir modül dosyasının en başında bulunan belgelendirme dizilerine erişebiliriz. Bu belgelendirme dizileri, modülle ilgili açıklamalar içerir. Aynı şeye `help()` fonksiyonunu kullanarak da erişebilirsiniz. Örneğin `help(os)` kullanarak `os` modulündeki bu bilgilendirme belgesine ulaşabilirsiniz. Bu belgelendirme dizileri, üç tırnak içinde belirtilir. Örnek:
```py
# Modül Dosyası
"""
Falan
Filan
"""
```
```py
# Program Dosyası
import modul
print(modul.__doc__)
```
**Output:**
```
Falan
Filan
```
çift veya tek tırnak ile belirlediğimiz karakter dizilerine __doc__ ile erişmek istersek sadece ilk satırdaki karakter dizisine erişebiliriz. Örnek:
```py
# Modül Dosyası
"Falan"
"Filan"
```
```py
# Program Dosyası
import modul
print(modul.__doc__)
```
**Output:**
```
Falan
```

## `__name__` Attribute
Her fonksiyon ve modül `__name__` attribute'sine sahiptir. Bu basitçe o fonksiyon ya da modülün ismi ya da spesifik bir şey olabilir.
```py
import modul
print(modul.__name__) # Output: modul
```
**Not:** Bir Python programı bağımsız bir program olarak çalıştırıldığında `__name__` attribute'unun değeri `__main__` olur. Örneğin bir xxx.py Python dosyasının `__name__` attribute'unu değeri `__main__` ise, xxx.py dosyasını herhangi bir dosyaya import ederek değil, diğer dosyalardan bağımsız olarak, doğrudan (üzerine çift tıklayarak çalıştırmak gibi)çalıştırmışsınız demektir.

## `__loader__` Attribute
Bu attribute, ilgili modülü içe aktaran mekanizma hakkında bize çeşitli bilgiler veren birtakım araçlar sunar. Örnek:
```py
import os
yükleyici = os.__loader__
print(dir(yükleyici))
```
**Output:**
```
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__',
 '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__',
 '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__',
 '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__',
 '__str__', '__subclasshook__', '__weakref__', '_cache_bytecode',
 'exec_module', 'get_code', 'get_data', 'get_filename', 'get_source',
 'is_package', 'load_module', 'name', 'path', 'path_mtime', 'path_stats',
 'set_data', 'source_to_code']
```

## `__spec__` Attribute
`__spec__` attribute, modüller hakkında çeşitli bilgiler sunan birtakım araçları içinde barındırır.
```py
import random
print(dir(random.__spec__))
```
**Output:**
```
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__',
 '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__',
 '__init__', '__init_subclass__', '__le__', '__lt__', '__module__',
 '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',
 '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__',
 '_cached', '_initializing', '_set_fileattr', 'cached', 'has_location',
 'loader', 'loader_state', 'name', 'origin', 'parent', 'submodule_search_locations']
```
Mesela bir modülün ad ve konum bilgilerine ulaşmak için bu niteliği kullanabiliriz.
```py
import random
print(random.__spec__.name, random.__spec__.origin, sep="\n")
```
**Output:**
```
random
C:\Users\XXX\AppData\Local\Programs\Python\Python39\lib\random.py
```

## `__file__` Attribute
Import edilmiş modülün path'ını içeren bir attribute'dur.
```py
import random
print(random.__file__) # Output: C:\Users\HP\AppData\Local\Programs\Python\Python39\lib\random.py
```

## `__cached__` Attribute
Modülle ilişkili önbelleğe alınmış (cached) dosyanın adını be konumunu (path) içeren bir attribute'dur. Konum bilgisi (path) mevcut Python dizinine (current Python directory) göredir.
```py
import random
print(random.__cached__) # Output: C:\Users\HP\AppData\Local\Programs\Python\Python39\lib\__pycache__\random.cpython-39.pyc
```

## `__builtins__` Attribute
Modülleden erişilebilen tüm build-in attribute'ların bir listesini içeren bir attribute'dur. Python bu attribute'ları sizin için otomatik olarak ekler.
```py
import random
print(random.__cached__)
```
**Output:**
```
{'__name__': 'builtins', '__doc__': "Built-in functions, exceptions, and other objects.\n\nNoteworthy: None is the `nil' object; Ellipsis represents `...' in slices.", '__package__': '', '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': ModuleSpec(name='builtins', loader=<class '_frozen_importlib.BuiltinImporter'>, origin='built-in'), '__build_class__': <built-in function __build_class__>, '__import__': <built-in function __import__>, 'abs': <built-in function abs>, 'all': <built-in function all>, 'any': <built-in function any>, 'ascii': <built-in function ascii>, 'bin': <built-in function bin>, 'breakpoint': <built-in function breakpoint>, 'callable': <built-in function callable>, 'chr': <built-in function chr>, 'compile': <built-in function compile>, 'delattr': <built-in function delattr>, 'dir': <built-in function dir>, 'divmod': <built-in function divmod>, 'eval': <built-in function eval>, 'exec': <built-in function exec>, 'format': <built-in function format>, 'getattr': <built-in function getattr>, 'globals': <built-in function globals>, 'hasattr': <built-in function hasattr>, 'hash': <built-in function hash>, 'hex': <built-in function hex>, 'id': <built-in function id>, 'input': <built-in function input>, 'isinstance': <built-in function isinstance>, 'issubclass': <built-in function issubclass>, 'iter': <built-in function iter>, 'len': <built-in function len>, 'locals': <built-in function locals>, 'max': <built-in function max>, 'min': <built-in function min>, 'next': <built-in function next>, 'oct': <built-in function oct>, 'ord': <built-in function ord>, 'pow': <built-in function pow>, 'print': <built-in function print>, 'repr': <built-in function repr>, 'round': <built-in function round>, 'setattr': <built-in function setattr>, 'sorted': <built-in function sorted>, 'sum': <built-in function sum>, 'vars': <built-in function vars>, 'None': None, 'Ellipsis': Ellipsis, 'NotImplemented': NotImplemented, 'False': False, 'True': True, 'bool': <class 'bool'>, 'memoryview': <class 'memoryview'>, 'bytearray': <class 'bytearray'>, 'bytes': <class 'bytes'>, 'classmethod': <class 'classmethod'>, 'complex': <class 'complex'>, 'dict': <class 'dict'>, 'enumerate': <class 'enumerate'>, 'filter': <class 'filter'>, 'float': <class 'float'>, 'frozenset': <class 'frozenset'>, 'property': <class 'property'>, 'int': <class 'int'>, 'list': <class 'list'>, 'map': <class 'map'>, 'object': <class 'object'>, 'range': <class 'range'>, 'reversed': <class 'reversed'>, 'set': <class 'set'>, 'slice': <class 'slice'>, 'staticmethod': <class 'staticmethod'>, 'str': <class 'str'>, 'super': <class 'super'>, 'tuple': <class 'tuple'>, 'type': <class 'type'>, 'zip': <class 'zip'>, '__debug__': True, 'BaseException': <class 'BaseException'>, 'Exception': <class 'Exception'>, 'TypeError': <class 'TypeError'>, 'StopAsyncIteration': <class 'StopAsyncIteration'>, 'StopIteration': <class 'StopIteration'>, 'GeneratorExit': <class 'GeneratorExit'>, 'SystemExit': <class 'SystemExit'>, 'KeyboardInterrupt': <class 'KeyboardInterrupt'>, 'ImportError': <class 'ImportError'>, 'ModuleNotFoundError': <class 'ModuleNotFoundError'>, 'OSError': <class 'OSError'>, 'EnvironmentError': <class 'OSError'>, 'IOError': <class 'OSError'>, 'WindowsError': <class 'OSError'>, 'EOFError': <class 'EOFError'>, 'RuntimeError': <class 'RuntimeError'>, 'RecursionError': <class 'RecursionError'>, 'NotImplementedError': <class 'NotImplementedError'>, 'NameError': <class 'NameError'>, 'UnboundLocalError': <class 'UnboundLocalError'>, 'AttributeError': <class 'AttributeError'>, 'SyntaxError': <class 'SyntaxError'>, 'IndentationError': <class 'IndentationError'>, 'TabError': <class 'TabError'>, 'LookupError': <class 'LookupError'>, 'IndexError': <class 'IndexError'>, 'KeyError': <class 'KeyError'>, 'ValueError': <class 'ValueError'>, 'UnicodeError': <class 'UnicodeError'>, 'UnicodeEncodeError': <class 'UnicodeEncodeError'>, 'UnicodeDecodeError': <class 'UnicodeDecodeError'>, 'UnicodeTranslateError': <class 'UnicodeTranslateError'>, 'AssertionError': <class 'AssertionError'>, 'ArithmeticError': <class 'ArithmeticError'>, 'FloatingPointError': <class 'FloatingPointError'>, 'OverflowError': <class 'OverflowError'>, 'ZeroDivisionError': <class 'ZeroDivisionError'>, 'SystemError': <class 'SystemError'>, 'ReferenceError': <class 'ReferenceError'>, 'MemoryError': <class 'MemoryError'>, 'BufferError': <class 'BufferError'>, 'Warning': <class 'Warning'>, 'UserWarning': <class 'UserWarning'>, 'DeprecationWarning': <class 'DeprecationWarning'>, 'PendingDeprecationWarning': <class 'PendingDeprecationWarning'>, 'SyntaxWarning': <class 'SyntaxWarning'>, 'RuntimeWarning': <class 'RuntimeWarning'>, 'FutureWarning': <class 'FutureWarning'>, 'ImportWarning': <class 'ImportWarning'>, 'UnicodeWarning': <class 'UnicodeWarning'>, 'BytesWarning': <class 'BytesWarning'>, 'ResourceWarning': <class 'ResourceWarning'>, 'ConnectionError': <class 'ConnectionError'>, 'BlockingIOError': <class 'BlockingIOError'>, 'BrokenPipeError': <class 'BrokenPipeError'>, 'ChildProcessError': <class 'ChildProcessError'>, 'ConnectionAbortedError': <class 'ConnectionAbortedError'>, 'ConnectionRefusedError': <class 'ConnectionRefusedError'>, 'ConnectionResetError': <class 'ConnectionResetError'>, 'FileExistsError': <class 'FileExistsError'>, 'FileNotFoundError': <class 'FileNotFoundError'>, 'IsADirectoryError': <class 'IsADirectoryError'>, 'NotADirectoryError': <class 'NotADirectoryError'>, 'InterruptedError': <class 'InterruptedError'>, 'PermissionError': <class 'PermissionError'>, 'ProcessLookupError': <class 'ProcessLookupError'>, 'TimeoutError': <class 'TimeoutError'>, 'open': <built-in function open>, 'quit': Use quit() or Ctrl-Z plus Return to exit, 'exit': Use exit() or Ctrl-Z plus Return to exit, 'copyright': Copyright (c) 2001-2021 Python Software Foundation.       
All Rights Reserved.

Copyright (c) 2000 BeOpen.com.
All Rights Reserved.

Copyright (c) 1995-2001 Corporation for National Research Initiatives.
All Rights Reserved.

Copyright (c) 1991-1995 Stichting Mathematisch Centrum, Amsterdam.
All Rights Reserved., 'credits':     Thanks to CWI, CNRI, BeOpen.com, Zope Corporation and a cast of thousands
    for supporting Python development.  See www.python.org for more information., 'license': Type license() to see the full license text, 'help': Type help() for interactive help, or help(object) for help about object.}
```

## `__spec__` Attribute
Ayrıntılı bilgi için Python doc'daki [The import system](https://docs.python.org/3/reference/import.html) kısmını inceleyebilirsiniz. Buradaki ModuleSpec ile ilgili bilgi için [tıklayınız](https://docs.python.org/3/library/importlib.html#importlib.machinery.ModuleSpec).
```py
import random
print(random.__spec__) # Output: ModuleSpec(name='random', loader=<_frozen_importlib_external.SourceFileLoader object at 0x000002165D61D730>, origin='C:\\Users\\HP\\AppData\\Local\\Programs\\Python\\Python39\\lib\\random.py')
print(type(random.__spec__)) # Output: <class '_frozen_importlib.ModuleSpec'>
```




