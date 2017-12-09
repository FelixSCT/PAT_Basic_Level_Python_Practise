
# PAT Basic Level Practise 1001-1025

### 为啥是PAT Basic Level Practise
&emsp;&emsp;当然是准备考[PAT](https://www.patest.cn/p/index#)。打算是一边看算法导论一边刷PAT的题。刚开始看算法导论，还不打算直接做Advanced Level Practise，加上略有强迫症，以及时间尚早，就先做做[Basic Level](https://www.patest.cn/contests/pat-b-practise)练练手，顺便练习一下Python。就当是消遣吧，毕竟真上Advanced Level的时候肯定还是C++的。  
### 关于Python使用体验
&emsp;&emsp;总的来说就是让简单的题变得更简单，运行时间要求苛刻的题变得……非常苛刻。  
&emsp;&emsp;用Python做题最致命的缺点就是时间开销大，据说比C++慢10到50倍（就我在1007题上的观察来看是50倍）。于是遇到运行时间要求苛刻的题的时候，要么就降低算法时间复杂度，要么就……用C++好了。  
&emsp;&emsp;当然Python的好处也是有很多，比如手感很好，可读性更强（如果不写函数式编程的话），语句更简单灵活 blah blah…… 有时候还能发现一些奇特方法，比如某些数的问题（甚至大数问题）使用字符串操作解决，如1006题。因为Python3的输入默认都是字符串类型，有很好的提示作用，而且用Python字符串和列表自带的函数操作起来可以减少很多代码量，有时还会让简单题毫无难度，如1009题。另外，Python3可以使用中文定义变量也很有趣，有时候不知道变量英文叫啥的时候可以直接用中文了，比拼音更直观。

---
&emsp;&emsp;那么代码来了，没什么问题就不多废话了，一些需要注意的点会加以说明（一般是一次没有通过的）。

### 1001. 害死人不偿命的(3n+1)猜想 (15)


```python
# 1001
n = int(input())
i = 0
while n != 1:
    if n % 2 == 0:
        n = int(n / 2)
    else:
        n = int((3 * n + 1) / 2)
    i += 1
print(i)
```

### 1002. 写出这个数 (20)


```python
# 1002
py_dict = {'0':'ling', '1':'yi', '2':'er', '3':'san', '4':'si', '5':'wu', 
           '6':'liu', '7':'qi', '8':'ba', '9':'jiu'}
n = input()
num = 0
for ch in n:
    num += int(ch)
num_str = str(num)
py_str = ''
for ch in num_str:
    py_str += py_dict[ch]
    py_str += ' '
py_str = py_str[:-1]
print(py_str)
```

### 1003. 我要通过！(20)
&emsp;&emsp;题目理解有点费劲，条件3中 **_如果 aPbTc 是正确的，那么 aPbATca 也是正确的_** 乍一看好像很多很难找的样子，但其实这是建立在条件2的基础之上的，也就是 **_aPbTc_** 必须由条件2或条件3判断为正确，而最初符合 **_aPbTc_** 的一定是 **_xPATx_** 此时 **_a = c, b = 1_** 即 **_a * b = c_**，又由 **_aPbATca_** 可得出 **_new-a * new-b = new-c (new-a = a, new-b = b + 1, new-c = a + c)_**  
&emsp;&emsp;因此，条件3即为：**_P左边A的个数 * PT之间A的个数 = T右边A的个数_** ，但需要注意为0的情况，如PT，PA。  
> &emsp;&emsp;原本代码稍长一点，看到某论坛里也有人用Python写，贴的代码里用一个 _def isOK_ 做判断，于是照着写了一个 _def YES_ ，现在回去找不着那位老哥了。


```python
# 1003
n = int(input())
str_list = []
for i in range(n):
    str_list.append(input())

def YES(string):    
    num_a = [0, 0, 0]
    abc = 0
    num_p = 0
    num_t = 0    
    for ch in string:        
        if ch == 'A':            
            num_a[abc] += 1        
        elif ch == 'P':            
            if num_p > 0:                
                return False            
            num_p += 1
            abc += 1        
        elif ch == 'T':            
            if num_p == 0 or num_t > 0:                
                return False               
            num_t += 1
            abc += 1        
        else:            
            return False    
    if num_a[0] * num_a[1] == num_a[2] and num_a[1] > 0 and abc == 2:        
        return True    
    return False

for s in str_list:
    if YES(s):
        print('YES')
    else:
        print('NO')
```

### 1004. 成绩排名 (20)


```python
# 1004
n = int(input())
name_list, id_list, score_list = [], [], []
for i in range(n):
    s = input().split()
    name_list.append(s[0])
    id_list.append(s[1])
    score_list.append(int(s[2]))
max_score = -1
min_score = 101
max_idx, min_idx = 0, 0
for i, score in enumerate(score_list):
    if score > max_score:
        max_score = score
        max_idx = i
    if score < min_score:
        min_score = score
        min_idx = i
print(name_list[max_idx], id_list[max_idx])
print(name_list[min_idx], id_list[min_idx])
#Joe Math990112 89
#Mike CS991301 100
#Mary EE990830 95
```

### 1005. 继续(3n+1)猜想 (25)


```python
# 1005
def cover(n, n_list):
    coverd_idx = []
    while n != 1:
        if n % 2 == 0:
            n = int(n / 2)
            if n in n_list:
                coverd_idx.append(n_list.index(n))
        else:
            n = int((3 * n + 1) / 2)
            if n in n_list:
                coverd_idx.append(n_list.index(n))
    return coverd_idx
    
k = int(input())
n_list = input().split()
for i, n in enumerate(n_list):
    n_list[i] = int(n)   
coverd_idx = []
for i, n in enumerate(n_list):
    if i in coverd_idx:
        continue
    else:
        new_coverd = cover(n, n_list)
        for nc in new_coverd:
            if nc not in coverd_idx:
                coverd_idx.append(nc)

key_num = []
for i in range(k):
    if i not in coverd_idx:
        key_num.append(n_list[i])
key_num = sorted(key_num, reverse=True)
key_str = ''
for kn in key_num:
    key_str += str(kn) + ' '
key_str = key_str[:-1]    
print(key_str)
```

### 1006. 换个格式输出整数 (15)


```python
# 1006
n = input()
n_len = len(n)
n_str = ''
for i, s in enumerate(n):
    if i == n_len - 1:
        for j in range(int(s)):
            n_str += str(j + 1)
    elif n_len == 3 and i == 0:
        for j in range(int(s)):
            n_str += ('B')
    else:
        for j in range(int(s)):
            n_str += ('S')
            
print(n_str)
```

### 1007. 素数对猜想 (20)
&emsp;&emsp;Python程序如果按照 从2到sqrt(n)找因子的方法判断素数 会在最后一个测试点运行超时（C/C++可以运行通过）。因此采用了**快速线性筛法求素数**。  
  
**快速线性筛法求素数**大致原理为：
- 每一个合数都可以分解成一个素数乘以一个数
- 如果一个合数能分解成一个素数乘以一个合数
    - 那么分解出来的合数又可以分解成一个素数乘以一个数
    - 原来的那个合数就可能表示成另一个素数乘以另一个合数
- 因此，一个合数的表示可以用其最小质因数与其他数的乘积确定

>另可参见 [快速线性筛法求素数](http://blog.csdn.net/nuanxin_520/article/details/41207145)


```python
%%time
# 1007
#n = int(input())
n = 99999
couple_prime = 0
num_prime = 0
prime = []
is_not_prime = []
for i in range(n + 1):
    prime.append(0)
    is_not_prime.append(0)

for i in range(2, n+1):
    if is_not_prime[i] == 0:
        prime[num_prime] = i
        num_prime += 1
    for j in range(num_prime):
        not_prime = i * prime[j]
        if not_prime > n - 1:
            break
        is_not_prime[not_prime] = 1
        if i % prime[j] == 0:
            break
for i in range(1, n):
    if prime[i] == 0:
        break
    if prime[i] - prime[i - 1] == 2:
        couple_prime += 1
print(couple_prime)

# n = 99999
# Wall time: 172 ms
```

### 1008. 数组元素循环右移问题 (20)


```python
# 1008
input_raw = input().split()
n = int(input_raw[0])
m = int(input_raw[1])
num_list = input().split()
new_list = num_list.copy()
for i in range(n):
    ni = i + m
    while ni > n - 1:
        ni -= n
    new_list[ni] = num_list[i]
num_str = ''
for num in new_list:
    num_str += num + ' '

num_str = num_str[:-1]
print(num_str)
```

### 1009. 说反话 (20)


```python
# 1009
words = input().split()
words.reverse()
words_str = ''
for word in words:
    words_str += word + ' '
words_str = words_str[:-1]
print(words_str)
```

### 1010. 一元多项式求导 (25)
&emsp;&emsp;零多项式即为 _f(x) = a0 = 0_


```python
# 1010
input_raw = input().split()
result = []
compute = False
for i, num in enumerate(input_raw):
    input_raw[i] = int(num)
    if compute:
        new_num = input_raw[i] * input_raw[i - 1]
        if new_num != 0:
            result.append(new_num)
            result.append(input_raw[i] - 1)
        compute = False
    else:
        compute = True
result_str = ''
if result == []:
    result = [0, 0]
for r in result:
    result_str += str(r) + ' '
result_str = result_str[:-1]
print(result_str)
```

### 1011. A+B和C (15)


```python
# 1011
t = int(input())
result = []
for i in range(t):
    abc = input().split()
    a = int(abc[0])
    b = int(abc[1])
    c = int(abc[2])
    s = 'Case #{}: {}'.format(i + 1, a + b > c)
    s = s.replace('T', 't')
    s = s.replace('F', 'f')
    result.append(s)
    
for s in result:
    print(s)
```

### 1012. 数字分类 (20)


```python
# 1012
num = input().split()
a1, a2, a3, a4, a5 = 0, 0, 0, 0, 0
flag_1, flag_2 = False, False
flag = 1
count = 0
for i, n in enumerate(num):
    if i == 0:
        continue
    n = int(n)
    num[i] = n
    if n % 10 == 0:
        flag_1 = True
        a1 += n
    if n % 5 == 1:
        flag_2 = True
        a2 += n * flag
        flag *= -1
    if n % 5 == 2:
        a3 += 1
    if n % 5 == 3:
        count += 1
        a4 += n
    if n % 5 == 4:
        if n > a5:
            a5 = n

if not flag_1:
    a1 = 'N'
if not flag_2:
    a2 = 'N'
if a3 == 0:
    a3 = 'N'
if count == 0:
    a4 = 'N'
else:
    a4 = format(a4 / count, '.1f')
if a5 == 0:
    a5 = 'N'

print(a1, a2, a3, a4, a5)
```

### 1013. 数素数 (20)
&emsp;&emsp;还是时间问题。C/C++依然可以任性地用 从2到sqrt(n)找因子的方法判断素数。但是Python就要另谋出路了。  
&emsp;&emsp;折腾了一下最后发现这题Python无解，找到第10000个素数勉强还能在100ms左右（已经是我能找到最快的方法了），但是print1000次就需要70ms左右，如何去面对100ms的时间限制。如果尝试用换行符换行，而只print字符串一次，也依旧是运行超时（100ms以上150ms以内）。所以这题用Python的话我只能拿到19分，如果想要20分满分的话还得用C/C++。  
  
&emsp;&emsp;这里找素数的方法是  
- 找到一个**可能是下一个素数的数**
- 判断这个数是否为素数
- 通过将 **_当前_**&ensp;**可能是下一个素数的数** + 2 或 + 4 （交替进行，可以排除2和3的倍数）来确定 **_下一个_**&ensp;**可能是下一个素数的数**

  
&emsp;&emsp;另外还有一个发现就是将找素数的方法写在函数里（***def get_prime***），然后调用函数，比不用函数要快80-90ms（找到第10000个素数）。


```python
%%time
def get_prime(m, n):
    next_prime = 7
    delta = 2
    new_line = 0
    line_str = ''
    if m < 4:
        for i in range(m - 1, 3):
            if i >= n:
                break
            new_line += 1
            line_str += str(prime[i]) + ' '
    while len(prime) < n:
        is_prime = True
        for i in prime:
            if i * i > next_prime:
                break
            if next_prime % i == 0:
                is_prime = False
                break
        if is_prime:
            prime.append(next_prime)
            if len(prime) >= m:
                new_line += 1
                line_str += str(next_prime) + ' '
                if new_line == 10:
                    print(line_str[:-1])
                    line_str = ''
                    new_line = 0
        delta = 6 - delta
        next_prime += delta
    if line_str != '':
        print(line_str[:-1])

m, n = input().split()
m, n = int(m), int(n)
#m, n = 1, 10000
prime = [2, 3, 5]
get_prime(m, n)

# m, n = 1, 10000
# Wall time: 203 ms
```

### 1014. 福尔摩斯的约会 (20)
&emsp;&emsp;感觉题目表述有问题，题目给出示例里面前两串字符串**第2对相同的字符**应该是是'D'而不是'E'，因为第一对是'8'。  
&emsp;&emsp;按题意和最后提交结果，应该是想表达在**第1对相同的大写英文字母**之后的**（第1对）相同的字符**。


```python
# 1014
weekday = {'A':'MON', 'B':'TUE', 'C':'WED', 'D':'THU', 'E':'FRI', 'F':'SAT', 'G':'SUN'}
clock = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F', 'G',
         'H', 'I', 'J', 'K', 'L', 'M', 'N']
alphabet = ['A', 'a', 'B', 'b', 'C', 'c', 'D', 'd', 'E', 'e', 'F', 'f', 'G', 'g',
            'H', 'h', 'I', 'i', 'J', 'j', 'K', 'k', 'L', 'l', 'M', 'm', 'N', 'n', 
            'O', 'o', 'P', 'p', 'Q', 'q', 'R', 'r', 'S', 's', 'T', 't',
            'U', 'u', 'V', 'v', 'W', 'w', 'X', 'x', 'Y, ''y', 'Z', 'z']
code1, code2, code3, code4 = input(), input(), input(), input()
decode = ''
get_week = False
get_clock = False
n1 = min(len(code1), len(code2))
n2 = min(len(code3), len(code4))
for i in range(n1):
    if code1[i] == code2[i]:
        if not get_week:
            if code1[i] in weekday:
                decode += weekday[code1[i]] + ' '
                get_week = True
        elif not get_clock:
            if code1[i] in clock:
                clock_point = clock.index(code1[i])
                if clock_point < 10:
                    decode += '0'
                decode += str(clock_point) + ':'
                get_clock = True
                break
for i in range(n2):
    if code3[i] == code4[i]:
        if code3[i] in alphabet:
            if i < 10:
                decode += '0'
            decode += str(i)
            break
print(decode)
```

### 1015. 德才论 (25)
&emsp;&emsp;还是运行超时问题，print次数太多，Python无解。用C++做应该问题不大（不过看到网上也有说C++超时的，据说是因为用了iostream，换成cstdio就可以了）。所以，干脆就做一个Python面向对象练习好了。另外用中文变量名也比较接地气，毕竟题目是资治通鉴的德才论。


```python
# 1015
# Python class practice version
class student:
    stu_num = 0
    def __init__(self, sid, 德, 才):
        student.stu_num += 1
        self.sid = sid
        self.德 = 德
        self.才 = 才
        self.德才 = 德 + 才
    def __lt__(self, other):
        if self.德才 != other.德才:
            return self.德才 < other.德才
        if self.德 != other.德:
            return self.德 < other.德
        return self.sid > other.sid
    def __repr__(self):
        return "{} {} {}".format(self.sid, self.德, self.才)
        

n, l, h = input().split()
n, l, h = int(n), int(l), int(h)
才德全尽, 德胜才, 才德兼亡但尚有德胜才, 德不胜才 = [], [], [], []
for i in range(n):
    stu = input().split()
    sid = int(stu[0])
    德 = int(stu[1])
    才 = int(stu[2])
    if 德 >= l and 才 >= l:
        if 德 >= h:
            if 才 >= h:
                才德全尽.append(student(sid, 德, 才))
            else:
                德胜才.append(student(sid, 德, 才))
        elif 德 >= 才:
            才德兼亡但尚有德胜才.append(student(sid, 德, 才))
        else:
            德不胜才.append(student(sid, 德, 才))
print(student.stu_num)
for stu_list in [才德全尽, 德胜才, 才德兼亡但尚有德胜才, 德不胜才]:
    stu_list = sorted(stu_list, reverse=True)
    for i in stu_list:
        print(i)
```

### 1016. 部分A+B (15)


```python
# 1016
a, da, b, db = input().split()
pa, pb = '', ''
for ch in a:
    if ch == da:
        pa += da
for ch in b:
    if ch == db:
        pb += db
if pa == '':
    pa = 0
if pb == '':
    pb = 0
print(int(pa) + int(pb))
```

### 1017. A除以B (20)


```python
# 1017
a, b = input().split()
a = int(a)
b = int(b)
print(a // b, a % b)
```

### 1018. 锤子剪刀布 (20)
&emsp;&emsp;又超时了！print不多但是input多，看到Java也超时了，好像Java解决超时的办法是换了一个输入函数，然而Python3不知道除了input还能怎么样。限制是100ms，据说C++只要10ms。


```python
# 1018
n = int(input())
gesture = ['B', 'C', 'J']
win, draw, lose = 0, 0, 0
win_time = [0, 0, 0]
lose_time = [0, 0, 0]
for i in range(n):
    a, b = input().split()
    an = gesture.index(a)
    bn = gesture.index(b)
    fight = an - bn
    if fight == -1 or fight == 2:
        win += 1
        win_time[an] += 1
    elif fight == -2 or fight == 1:
        lose += 1
        lose_time[bn] += 1
    else:
        draw += 1
print(win, draw, lose)
print(lose, draw, win)
print(gesture[win_time.index(max(win_time))], gesture[lose_time.index(max(lose_time))])
```

### 1019. 数字黑洞 (20)
&emsp;&emsp;注意输入是0000和6174的问题


```python
# 1019
n = input()
if n == '6174':
    n = '1746'
if len(n) < 4:
    n = (4 - len(n)) * '0' + n
next_break = False
while n != '6174':
    if next_break:
        break
    n1_list, n2_list = sorted(n, reverse=True), sorted(n)
    n1, n2 = '', ''
    for i in range(4):
        n1 += n1_list[i]
        n2 += n2_list[i]
    n = int(n1) - int(n2)
    if n == 0:
        n = '0000'
        next_break = True
    else:
        n = str(n)
    if len(n) < 4:
        n = (4 - len(n)) * '0' + n
    print(n1, '-', n2, '=', n)
```

### 1020. 月饼 (25)
&emsp;&emsp;注意库存量和总售价是正数而不是正整数。


```python
# 1020
n, d = input().split()
n, d = int(n), int(d)
库存量 = input().split()
总售价 = input().split()
单价 = []
最大收益 = 0
for i in range(n):
    库存量[i] = float(库存量[i])
    总售价[i] = float(总售价[i])
    单价.append(0.0 if 库存量[i] == 0 else 总售价[i] / 库存量[i])
while d > 0:
    最大单价 = max(单价)
    if 最大单价 == 0:
        break
    num = 库存量[单价.index(最大单价)]
    if d <= num:
        最大收益 += d * 最大单价
        库存量[单价.index(最大单价)] -= d
    else:
        最大收益 += num * 最大单价
        库存量[单价.index(最大单价)] = 0
        单价[单价.index(最大单价)] = 0
    d -= num
print(format(最大收益, '.2f'))
```

### 1021. 个位数统计 (15)


```python
# 1021
n = input()
num_list = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
for ch in n:
    num_list[int(ch)] += 1
for i, num in enumerate(num_list):
    if num == 0:
        continue
    print('{}:{}'.format(i, num))
```

### 1022. D进制的A+B (20)


```python
# 1022
a, b, d = input().split()
c = int(a) + int(b)
if c == 0:
    print(0, end='')
d = int(d)
d_num = ''
while c != 0:
    d_num += str(c % d)
    c = c // d
d_num = d_num[::-1]
print(d_num)
```

### 1023. 组个最小数 (20)


```python
# 1023
num = input().split()
min_num = ''
for i in range(10):
    num[i] = int(num[i])
for i in range(1, 10):
    if num[i] > 0:
        num[i] -= 1
        min_num += str(i)
        break        
for i in range(10):
    if num[i] > 0:
        min_num += str(i) * num[i]
        num[i] = 0
if min_num =='':
    min_num = '0'
print(min_num)
```

### 1024. 科学计数法 (20)


```python
# 1024
#sci_num = '+1.23400E-03'
#sci_num = '-1.2E+10'
#sci_num = '+1.234567E+111'
sci_num = input()
sign = sci_num[0]
E_idx = sci_num.index('E')
E_idx_reverse = len(sci_num) - E_idx
num = sci_num[1:-E_idx_reverse]
num = num[:1] + num[2:]
E_sign = sci_num[-(E_idx_reverse - 1)]
E_exp = int(sci_num[-(E_idx_reverse - 2):])
new_num = ''
if E_sign == '-':
    new_num = '0.' + '0' * (E_exp - 1)
    new_num += num
else:
    deci_len = len(num) - 1
    if deci_len > E_exp:
        new_num = num[:E_exp + 1] + '.' + num[E_exp + 1:]
    else:
        new_num = num + '0' * (E_exp - deci_len)
    pass
if sign == '-':
        new_num = '-' + new_num

print(new_num)
```

### 1025. 反转链表 (25)
&emsp;&emsp;又双叕超时了！如果按照n最大的情况print100000次就要花掉大概10s了😑😵。  
&emsp;&emsp;还有测试点1到底是什么鬼！搞了半天也没通过 = = 居然就这样在乙级的题目上卡死了。


```python
# 1025
current_addr, n, k = input().split()
current_addr, n, k = int(current_addr), int(n), int(k)
l = []
node = [[0] * 2 for i in range(100000)]

for i in range(n):
    addr, data, next_addr = input().split()
    node[int(addr)] = [data, int(next_addr)]
while True:
    data, next_addr = node[current_addr]
    l.append([current_addr, data, next_addr])
    if next_addr != -1:
        current_addr = next_addr
    else:
        break
divide_l = [l[i: i + k] for i in range(0, len(l), k)]
reverse_l = []
for i in divide_l:
    if len(i) < k:
        reverse_l.append(i)
        break
    hole_next_addr = i[k - 1][2]
    i.reverse()
    for j in range(k):
        if j == k - 1:
            i[j][2] = hole_next_addr
            break
        i[j][2] = i[j + 1][0]
    reverse_l.append(i)
for i in reverse_l:
    for j in i:
        print('%05d' % j[0], j[1], -1 if j[2] == -1 else '%05d' % j[2])
```
