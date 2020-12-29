# Использование C-библиотеки в программе на Python

## CLib.py

В файле CLib.py содержатся функции, требуемые по заданию лабораторной работы.

Библиотека собирается с помощью ```make dynamic_lib``` в директории c_src.

С помощью модуля ctypes описал поведение функций, написанных в библиотеке:
```pydocstring
lib = ctypes.cdll.LoadLibrary("./c_array_lib.so")

arrayAllocate = lib.array_allocate
arrayAllocate.argtypes = (ctypes.c_int,)
arrayAllocate.restype = ctypes.POINTER(ctypes.c_int)

arrayFillFibonacci = lib.array_fill_fibonacci
arrayFillFibonacci.argtypes = (ctypes.POINTER(ctypes.c_int), ctypes.c_int)
arrayFillFibonacci.restype = ctypes.c_void_p
.....
```

Затем для функций создал "обертки", чтобы удобно использовать их в Python:
```pydocstring
def fillFibonacci(len):
    pyArray = []

    array_p = arrayAllocate(len)
    if array_p is not None:
        arrayFillFibonacci(array_p, len)

        for i in range(len):
            arr_num = arrayGetter(array_p, i)
            pyArray.append(arr_num)

        arrayFree(array_p)

    return pyArray
```

## main.py

Для визуальной составляющей использовался модуль Tkinter, который мы проходили в предыдущем семестре
В main.py были импортированы две функции из CLib.py:
```pydocstring
from CLib import fillFibonacci, copyFirstCopies
```

В итоге графический интерфейс выглядит следующим образом:
[Imgur](https://imgur.com/WnYS59E)

Ситуации с некорректным вводом также предусмотрены:
[Imgur](https://imgur.com/Lzbqmdt)