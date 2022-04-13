#Экзамен по крипте

### Задача 1.  -  Китайская теорема об остатках 
x1, mod 1, x2, mod 2, x3, mod 3
**Написать исходные данные на строке 23**
``` python
class Chinese_solve:
    def __init__(self, x1, mod1, x2, mod2, x3, mod3):
        self.x1 = x1
        self.mod1 = mod1
        self.x2 = x2
        self.mod2 = mod2
        self.x3 = x3
        self.mod3 = mod3

    def get_M(self):
        return self.mod1 * self.mod2 * self.mod3

    def get_allM(self, M):
        return M // self.mod1, M // self.mod2, M // self.mod3

    def get_allY(self, allM):
        return pow(allM[0], -1, self.mod1), pow(allM[1], -1, self.mod2), pow(allM[2], -1, self.mod3)

    def solve(self, M, allM, allY):
        return pow(self.x1 * allM[0] * allY[0] + self.x2 * allM[1] * allY[1] + self.x3 * allM[2] * allY[2], 1, M)


C = Chinese_solve(3, 8, 5, 13, 2, 5)

M = C.get_M()
allM = C.get_allM(M)
allY = C.get_allY(allM)

print(C.solve(M, allM, allY))
```

### Задача 2.  Система Диффи-Хеллмана
Параметры 𝑝 и 𝑔 выбрали случайные значения 𝑥 и 𝑦.
Найти значение сформированного секретного ключа K
4 числа – p, g, x, y
**Ответ – K, если попросит A и B, вписываем их соответственно)**
``` python
def dfhllmn (p, g, x,y):
    dec = 2   #проверяем является ли p простым числом
    if p > 1:
        while p % dec != 0:
            dec += 1
        if dec == p:   #если является, выполняем след действия
            a = g**x%p  #находим A
            b = g**y%p  #находим B
            ka = b**x%p #находим K для А
            kb = a**y%p #находим K для B
            if ka == kb:   #проверяем равны ли K двух чисел
                print('A = %d'%a, 'B = %d'%b, 'K = %d'%ka)   #пишем ответ
            else:
                print('error')  #ошибка, если ответы не совпадают
        else:
            print('error')  #ошибка, если p - не является простым

dfhllmn(11, 7, 3, 6) #задаем переменные для решения
```

### Задача 3.  Шифр Шамира 
Параметр p, выбрали для передачи случайные числа x1, y1
Передали число X1
Расширенный алгоритм Евклида найти x2, y2, определить М
4 числа - p, x1, y1, X1
``` python
def shamir(p, x1, y1, X1):
    x2 = pow(x1, -1, p-1)
    y2 = pow(y1, -1, p-1)
    Y1 = pow(X1, y1, p)
    X2 = pow(Y1, x2, p)
    Y2 = pow(X2, y2, p)
    M = Y2
    return print('x2 = %d'%x2, 'y2 = %d'%y2, 'Y1 = %d'%Y1,'X2 = %d'%X2, 'Y2 = %d'%Y2, 'M = %d'%M)

shamir(23, 9, 3, 8)

```

### Задача 4. Шифр Эль-Гамаля 
Параметры p и g, открытый ключ y, сообщение М, для шифрования случ. число k
Вычислить r, s
5 чисел – p, g, y, M, k
``` python
def elgamal1(p, g, y, M, k):
    r = pow(g, k, p)
    s = pow(M * pow(y, k, p), 1, p)
    print('r = %d'%r, 's = %d'%s)

elgamal1(59, 13, 6, 7, 12)
```

### Задача 5.  Шифр Эль-Гамаля
Параметры p и g, открытый ключ x, криптограмма (r, s)
Найти y, сообщение M
5 чисел – p, g, x, r, s
``` python
def elgamal2(p, g, x, r, s):
    y = pow(g, x, p)
    M = pow(s * pow(r, p-1-x, p), 1, p)
    print('y = %d'%y, 'M = %d'%M)

elgamal2(59, 10, 7, 15, 31)
```

### Задача 6. Система RSA
Открытые ключи опубликованы в справочнике
Абонент ni желает отправить абоненту ni+1 сообщение M
Найти С
В зависимости от абонента-получателя генерируем С, т.к. используем его открытый ключ
5 чисел – N1, e1, N2, e2, M (из которых используются для вычисления только 3)
``` python
def findC(N1, e1, N2, e2, M):
    a = int(input('Введите абонента получателя '))
    if a == 1:
        C = pow(M, e1, N1)
        print('C = %d'%C)
    elif a == 2:
        C = pow(M, e2, N2)
        print('C = %d'%C)
    else:
        print('Ошибка')

findC(989, 25, 899, 121, 150)
```

### Задача 7. Система RSA
Открытые ключи опубликованы в справочнике
Абонент ni направил абоненту ni+1 криптограмму С
В зависимости от абонента-получателя вычисояем d и M, т.к. используем его закрытый ключ
Найти d, M
7 чисел – N1, e1, N2, e2, C, p, q (из которых используются для вычисления только 5)
``` python
def find_d(N1, e1, N2, e2, C, p, q):
    a = int(input('Введите абонента получателя '))
    if a == 1:
        D = pow(e1, -1, (p-1)*(q-1))
        M = pow(C, D, N1)
        print('D = %d'%D, 'M = %d'%M)
    elif a == 2:
        D = pow(e2, -1, (p-1)*(q-1))
        M = pow(C, D, N2)
        print('D = %d'%D, 'M = %d'%M)
    else:
        print('Ошибка')

find_d(899, 121, 989, 25, 744, 23, 42)
find_d(989, 25, 899, 121, 584, 31, 29)
```

### Задача 8. Система RSA (взлом)
Открытые ключи – одинаковое значение модуля 𝑁
Разные (значения простые) значения экспонент 𝑒1 и 𝑒2.
Перехвачены криптограммы 𝐶1 и 𝐶2.
Соответствуют одному и тому же открытому сообщению М
Атака методом бесключевого чтения
5 чисел - N, e1, e2, C1, C2
``` python
def rsavzlm(N, e1, e2, c1, c2):
    r = 1
    s = 1
    while (r * e1 + s * e2 != 1):  #находим r и s
        if (r * e1 + s * e2 > 1):
            s -= 1
        if (r * e1 + s * e2 < 1):
            r += 1
    mx = pow(c1, r, N) #находим м для С1
    my = pow(c2, s, N) #находим м для с2
    M = mx * my % N    #находим М

    print('r = %d'%r, 's = %d'%s, 'M = %d'%M)

rsavzlm(137759, 191, 233, 60197, 63656)

```

### Задача 9. Система RSA(взлом)
Есть экспонента открытого ключа - e
Соответствуют одному и тому же открытому сообщению М
6 чисел – N1, N2, N3, C1, C2, C3 
Атака на основе китайской теоремы об остатках
``` python
def kitrsa(N1, N2, N3, C1, C2, C3):
    Y = N1 * N2 * N3
    y1 = N2 * N3
    y2 = N1 * N3
    y3 = N1 * N2
    n1 = pow(y1, -1, N1)
    n2 = pow(y2, -1, N2)
    n3 = pow(y3, -1, N3)
    S = C1 * n1 * y1 + C2 * n2 * y2 + C3 * n3 * y3
    C = pow(S, 1, Y)
    M = round(pow(C, 1/3))
    print('n1 = %d'%n1, 'n2 = %d'%n2, 'n3 = %d'%n3, 'S = %d'%S, 'C = %d'%C, 'M = %d'%M)
kitrsa(26549, 45901, 25351, 5366, 814, 4454)

```

### Задача 10. Система RSA(взлом)
Одно число – модуль N, найти p и q.
Название кода “attck ferma”
``` python
import math
def atckferm(N):
    t = math.ceil(N ** 0.5) #находим t
    v = 0.1
    while v % 1 != 0: # делаем проверку на целое число
        t += 1 #увеличиваем t на 1
        v = (t**2 - N) ** 0.5 #находим v
        p = t + v #находим p
        q = t - v #находим q
    print('p = %d'%p, 'q = %d'%q)
atckferm(7571)
```




