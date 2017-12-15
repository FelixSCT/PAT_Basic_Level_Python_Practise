
# PAT Basic Level Practise 1026-1050

### 第二周体验
&emsp;&emsp;超时都不想说了，平平淡淡才是真。  
&emsp;&emsp;确定了一下测试服务器没有numpy（至少Python3没有）。

---
&emsp;&emsp;那么代码来了，没什么问题就不多废话了，一些稍稍重要的点会加以说明（一般是超时的）。

### 1026. 程序运行时间(15)
&emsp;&emsp;Python3里面 **round** 取整的判断方法在大多数情况都和四舍五入一样，取更接近的整数。但是遇到和两边距离一样的时候，比如 1.5 或 0.5 ，是取到偶数一侧的。于是 **round(1.5) = 2** ，而 **round(0.5) = 0** 。要让** round(n)** 实现真正的四舍五入的话，可以让 **n = n + 0.001** (在精度为两位小数时)，这样，在出现 x.5 的情况时会变成 x.51 ，与较大数更接近。


```python
# 1026
c1, c2 = input().split()
c1, c2 = int(c1), int(c2)
time = round((c2 - c1) / 100 + 0.001)
hh = time // 3600
time -= hh * 3600
mm = time // 60
ss = time - mm * 60
time_str = '%02d' % hh + ':' + '%02d' % mm + ':' + '%02d' % ss
print(time_str)
```

### 1027. 打印沙漏(20)


```python
# 1027
n, ch = input().split()
n = int(n)
n -= 1
t = 3
s = ch
while n >= 2 * t:
    s = t * ch + '\n' + s + t * ch
    s = s.replace('\n', '\n ')
    s = s[:-t] + '\n' + s[-t:]
    n -= 2 * t
    t += 2
print(s)
print(n)
```

### 1028. 人口普查(20)
&emsp;&emsp;最后一个测试点运行超时，应该是输入次数太多。据说Python2可以通过，IO模块更快一些。


```python
# 1028
n = int(input())
valid = 0
oldest, youngest = '', ''
oldest_age = -1
youngest_age = 73001
for i in range(n):
    name, birth = input().split()
    year, month, date = birth.split('/')
    year, month, date = int(year), int(month), int(date)
    age = 365 * (2014 - year) + 30 * (9 - month) + (6 - date)
    if age < 0 or age > 73000:
        continue
    valid += 1
    if age > oldest_age:
        oldest_age = age
        oldest = name
    if age < youngest_age:
        youngest_age = age
        youngest = name
if oldest == '':
    print(0)
else:
    print(valid, oldest, youngest)
```

### 1029. 旧键盘(20)


```python
# 1029
real_str = input()
input_str = input()
invalid_keys = []
for ch in real_str:
    if ch not in input_str and ch.upper() not in invalid_keys:
        invalid_keys.append(ch.upper())
for key in invalid_keys:
    print(key, end='')
```

### 1030. 完美数列(25)
&emsp;&emsp;运行超时，嵌套 for 循环遍历1200000次（100000 * 12），每次进行一次 if 判断和赋值 差不多就要300ms了。


```python
# 1030
n, p = input().split()
n, p = int(n), int(p)
num_list = input().split()
for i in range(n):
    num_list[i] = int(num_list[i])
num_list.sort()
max_num = 0
max_len = 0
for i in range(n):
    for j in range(max_num, n):
        if num_list[j] <= num_list[i] * p:
            max_len = j - i if j - i > max_len else max_len
            max_num = num_list[i]
        else:
            break
print(max_len + 1)
```

### 1031. 查验身份证(15)
&emsp;&emsp;貌似还要注意前17位里面有 X 的情况。


```python
# 1031
weight = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]
m = ['1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2']
n = int(input())
invalid = []
for i in range(n):
    id_num = input()
    z = 0
    skip = False
    for j in range(17):
        if id_num[j] not in m or id_num[j] == 'X':
            skip = True
            invalid.append(id_num)
            break
        z += int(id_num[j]) * weight[j]
    if skip:
        continue
    z = z % 11
    if id_num[-1] != m[z]:
        invalid.append(id_num)
if len(invalid) > 0:
    for i in invalid:
        print(i)
else:
    print('All passed')
```

### 1032. 挖掘机技术哪家强(20)
&emsp;&emsp;超时（明明 0ns 就可以知道答案了 =0=）。


```python
# 1032
schools = [0 for i in range(100001)]
n = int(input())
for i in range(n):
    school, score = input().split()
    school, score = int(school), int(score)
    schools[school] += score
best_school = max(schools)
print(schools.index(best_school), best_school)
```

### 1033. 旧键盘打字(20)
&emsp;&emsp;(⊙﹏⊙)忘了要说啥了…… 反正是和大写键有关的。


```python
# 1033
invalid_keys = input().lower()
real_str = input()
input_str = ''
no_caps = '+' in invalid_keys
for ch in real_str:
    if ch in invalid_keys or ch.lower() in invalid_keys:
        continue
    if no_caps and ch >= 'A' and ch <= 'Z':
        continue
    input_str += ch
print(input_str)
```

### 1034. 有理数四则运算(20)
&emsp;&emsp;计算最大公约数的函数 gcd 在 from math import gcd 的时候居然返回非零，只好自己写一个 def gcd 。


```python
# 1034
#from math import gcd
def gcd(x, y):
    if x % y == 0:
        return y
    else:
        return gcd(y, x % y)

n1, n2 = input().split()
a1, b1 = n1.split('/')
a2, b2 = n2.split('/')
a1, b1, a2, b2 = int(a1), int(b1), int(a2), int(b2)
add_result = [a1 * b2 + a2 * b1, b1 * b2]
sub_result = [a1 * b2 - a2 * b1, b1 * b2]
mul_result = [a1 * a2, b1 * b2]
if a2 > 0:
    div_result = [a1 * b2, b1 * a2]
else:
    div_result = [-a1 * b2, -b1 * a2]
crude = [[a1, b1], [a2, b2], add_result, sub_result, mul_result, div_result]
result = []
for a, b in crude:
    if b == 0:
        result.append('Inf')
        continue
    if a == 0:
        result.append('0')
        continue
    ab_gcd = gcd(a, b)
    a //= ab_gcd
    b //= ab_gcd
    negtive = a < 0
    result_str = ''
    times = abs(a) // b
    a = abs(a) % b
    if times > 0 and a > 0:
        result_str += str(times) + ' ' + str(a) + '/' + str(b)
    elif times > 0 and a == 0:
        result_str += str(times)
    elif times == 0 and a > 0:
        result_str += str(a) + '/' + str(b)
    if negtive:
        result_str = '(-' + result_str + ')'
    result.append(result_str)
print(result[0], '+', result[1], '=', result[2])
print(result[0], '-', result[1], '=', result[3])
print(result[0], '*', result[1], '=', result[4])
print(result[0], '/', result[1], '=', result[5])
```

### 1035. 插入与归并(25)
&emsp;&emsp;插入可以直接进行下一步，归并要从头开始。


```python
# 1035
n = int(input())
raw = input().split()
semi = input().split()
for i in range(n):
    raw[i], semi[i] = int(raw[i]), int(semi[i])
flag = False
for i in range(1, n):
    if semi[i] < semi[i - 1]:
        idx = i
        for j in range(i, n):
            if raw[j] != semi[j]:
                flag = True
                break
        break
    else:
        continue 
if flag:
    print('Merge Sort')
    time = 1
    finish = False
    while True:
        new_list = []
        epb = time * 2
        separate = [raw[i: i + epb] for i in range(0, n, epb)]
        for i in separate:
            new_list += sorted(i)
        time = epb
        raw = new_list
        if finish:
            break
        if raw == semi:
            finish = True
    for i in raw[:-1]:
        print(i, end=' ')
    print(raw[-1])
else:
    print('Insertion Sort')
    result = sorted(semi[:idx + 1]) + semi[idx + 1:]
    for i in result[:-1]:
        print(i, end=' ')
    print(result[-1])
```

### 1036. 跟奥巴马一起编程(15)
&emsp;&emsp;round 的问题同1026题，这里只要 + 0.1 即可。


```python
# 1036
n, ch = input().split()
n = int(n)
row = round(n / 2 + 0.1)
col = n
print(ch * col)
for i in range(row - 2):
    row_str = ch + ' ' * (col - 2) + ch
    print(row_str)
print(ch * col)
```

### 1037. 在霍格沃茨找零钱（20）


```python
# 1037
p, a = input().split()
p_g, p_s, p_k = p.split('.')
a_g, a_s, a_k = a.split('.')
c_g = int(a_g) - int(p_g)
c_s = int(a_s) - int(p_s)
c_k = int(a_k) - int(p_k)
if c_g < 0 or (c_g == 0 and c_s < 0) or (c_g == 0 and c_s == 0 and c_k < 0):
    print('-', end='')
    c_g *= -1
    c_s *= -1
    c_k *= -1
if c_k < 0:
    c_k += 29
    c_s -= 1
if c_s < 0:
    c_s += 17
    c_g -= 1
print('{}.{}.{}'.format(c_g, c_s, c_k))
```

### 1038. 统计同成绩学生(20) 
&emsp;&emsp;运行超时。


```python
# 1038
n = int(input())
scores = input().split()
search = input().split()
k = int(search[0])
result = [0 for i in range(k)]
for i in range(1, k + 1):
    result[i - 1] = str(scores.count(search[i]))
print(' '.join(result))  
```

### 1039. 到底买不买（20）


```python
# 1039
present = input()
wanted = input()
lack = 0
for i in wanted:
    if i in present:
        idx = present.index(i)
        present = present[:idx] + present[idx + 1:]
    else:
        lack += 1
buy = lack == 0
print('Yes' if buy else 'No', end=' ')
if buy:
    print(len(present))
else:
    print(lack)
```

### 1040. 有几个PAT（25）
&emsp;&emsp;运行超时。  
&emsp;&emsp;有效的PAT集中在第一个P与最后一个T之间。  
&emsp;&emsp;对于每个A来说，能够组成的PAT的个数等于其左边P的个数乘以其右边T的个数。


```python
# 1040
pat = input()
len_pat = len(pat)
lp, rt = 0, 0
p = False
for i in range(len_pat):
    if pat[i] == 'P':
        lp = i
        break
for i in range(len_pat)[::-1]:
    if pat[i] == 'T':
        rt = i
        break
if rt > lp:
    focus = pat[lp:rt + 1]
else:
    focus = ''
n = 0
for i in range(len(focus)):
    if focus[i] == 'A':
        np = focus[:i].count('P')
        nt = focus[i:].count('T')
        n += np * nt
print(n % 1000000007)
```

### 1041. 考试座位号(15)


```python
# 1041
n = int(input())
l = [[0] * 2 for i in range(1001)]
for i in range(n):
    sid, seat1, seat2 = input().split()
    l[int(seat1)] = [sid, seat2]
m = int(input())
search = input().split()
for i in search:
    idx = int(i)
    print(l[idx][0], l[idx][1])
```

### 1042. 字符统计(20)


```python
# 1042
input_str = input().lower()
ch, max_times = '', 0
for i in range(97, 123):
    times = input_str.count(chr(i))
    if times > max_times:
        ch = chr(i)
        max_times = times
print(ch, max_times)
```

### 1043. 输出PATest(20)


```python
# 1043
input_str = input()
nP = input_str.count('P')
nA = input_str.count('A')
nT = input_str.count('T')
ne = input_str.count('e')
ns = input_str.count('s')
nt = input_str.count('t')
n = nP + nA + nT + ne + ns + nt
out_str = ''
for i in range(n):
    out_str += 'P' if nP > 0 else ''
    out_str += 'A' if nA > 0 else ''
    out_str += 'T' if nT > 0 else ''
    out_str += 'e' if ne > 0 else ''
    out_str += 's' if ns > 0 else ''
    out_str += 't' if nt > 0 else ''
    nP -= 1
    nA -= 1
    nT -= 1
    ne -= 1
    ns -= 1
    nt -= 1
print(out_str)
```

### 1044. 火星数字(20)


```python
# 1044
mars_code = {'tret':'0', 'jan':'1', 'feb':'2', 'mar':'3', 'apr':'4', 'may':'5', 'jun':'6', 'jly':'7',
             'aug':'8', 'sep':'9', 'oct':'10', 'nov':'11', 'dec':'12', 'tam':'1*13', 'hel':'2*13',
             'maa':'3*13', 'huh':'4*13', 'tou':'5*13', 'kes':'6*13', 'hei':'7*13', 'elo':'8*13',
             'syy':'9*13', 'lok':'10*13', 'mer':'11*13', 'jou':'12*13'}
mars_code_list1 = ['tret', 'jan', 'feb', 'mar', 'apr', 'may', 'jun', 'jly', 'aug', 'sep', 'oct', 'nov', 'dec']
mars_code_list2 = ['', 'tam', 'hel', 'maa', 'huh', 'tou', 'kes', 'hei', 'elo', 'syy', 'lok', 'mer', 'jou']
def earth2mars(num):
    num = int(num)
    mn = ''
    c1, c2 = num % 13, num // 13
    mn += mars_code_list2[num // 13]
    if c2 == 0:
        mn += mars_code_list1[num % 13]
    elif c1 > 0:
        mn += ' '
        mn += mars_code_list1[num % 13]
    return mn

def mars2earth(num):
    en = 0
    for i in num.split():
        en += eval(mars_code[i])
    return en

n = int(input())
out = []
for i in range(n):
    num = input()
    if num[0] >= '0' and num[0] <= '9':
        out.append(earth2mars(num))
    else:
        out.append(mars2earth(num))
for i in out:
    print(i)
```

### 1045. 快速排序(25)
&emsp;&emsp;运行超时。  
&emsp;&emsp;最慢的一个测试点大概在250ms左右。  
&emsp;&emsp;主元在排列前后的**位置不变**。


```python
# 1045
n = int(input())
num = input().split()
for i in range(n):
    num[i] = int(num[i])
sorted_num = sorted(num)
principal = []
for i in range(n):
    if num[i] == sorted_num[i] and max(num[:i + 1]) <= num[i] and min(num[i: n]) <= num[i]:
        principal.append(num[i])
print(len(principal))
out_str = ''
for i in principal:
    out_str += str(i) + ' '
print(out_str[:-1])
```

### 1046. 划拳(15)


```python
# 1046
n = int(input())
甲喝, 乙喝 = 0, 0
for i in range(n):
    甲喊, 甲划, 乙喊, 乙划 = input().split()
    s = str(int(甲喊) + int(乙喊))
    if 甲划 == s and 乙划 != s:
        乙喝 += 1
    if 甲划 != s and 乙划 == s:
        甲喝 += 1
print(甲喝, 乙喝)
```

### 1047. 编程团体赛(20)


```python
# 1047
n = int(input())
team = [0 for i in range(1001)]
for i in range(n):
    player, score = input().split()
    player = player.split('-')
    team[int(player[0])] += int(score)
max_score = max(team)
print(team.index(max_score), max_score)
```

### 1048. 数字加密(20)


```python
# 1048
a, b = input().split()
n = max(len(a), len(b))
a, b = a.zfill(n), b.zfill(n)
a, b = a[::-1], b[::-1]
code = ''
poker_point = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'J', 'Q', 'K']
flag = True
for i in range(n):
    if flag:
        code += poker_point[(int(a[i]) + int(b[i])) % 13]
    else:
        delta = int(b[i]) - int(a[i])
        code += str(delta) if delta >= 0 else str(delta + 10)
    flag = not flag
print(code[::-1])
```

### 1049. 数列的片段和(20)
&emsp;&emsp;在求和的过程中，每个元素被加的次数等于**它前面的元素个数 + 1 乘以 它后面的元素个数 + 1**。


```python
# 1049
n = int(input())
array = input().split()
s = 0
for i in range(n):
    idx = i + 1
    s += idx * (n - idx + 1) * float(array[i])
print('%.2f' % s)
```

### 1050. 螺旋矩阵(25)
&emsp;&emsp;哇，没有超时。 1046-1050 得了**100分**不容易啊。


```python
# 1050
from math import sqrt
n = int(input())
num = input().split()
for i in range(n):
    num[i] = int(num[i])
num.sort()
num.reverse()
w = int(sqrt(n))
h = w
for i in range(1, w + 1)[::-1]:
    if n % i == 0:
        w = i
        h = n // i
        break
out = [[0] * w for i in range(h)]
row, col = 0, w
idx = 0
while idx < n:
    for i in range(w):
        if out[row][i] == 0:
            out[row][i] = num[idx]
            idx += 1
    row += 1
    for i in range(h):
        if out[i][col - 1] == 0:
            out[i][col - 1] = num[idx]
            idx += 1
    col -= 1
    for i in range(w)[::-1]:
        if out[-row][i] == 0:
            out[-row][i] = num[idx]
            idx += 1
    for i in range(h)[::-1]:
        if out[i][w - col - 1] == 0:
            out[i][w - col - 1] = num[idx]
            idx += 1
for i in out:
    for j in i[:-1]:
        print(j, end=' ')
    print(i[-1])
```
