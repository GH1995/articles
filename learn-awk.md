---
title: learn awk
date: 2018-01-21 16:24:32
tags:
    - Linux
---

# 模式扫描与处理语言 awk

```
awk '程序' 文件名
```

```
程序：
    模式 {操作}
    模式 {操作}
    模式 {操作}
    模式 {操作}
```

awk 一次一行地读文件名中的输入，依次将每行与每个模式相比较，对每个与行相匹配的模式，执行其对应的操作。

```
awk `/regex/ { print }` 文件名
```


省略动作，默认动作是打印相匹配的行
```
awk '/regex/' 文件名
```

省略模式，被作用到任何输入行
```
awk '{ print }' 文件名
```

从一个文件提交程序
```
awk -f 命令文件 文件名
```

## 字段
```
$1, $2, ..., $NF
```

`NF`是一个变量，被设置为字段的个数。

```
du -a | awk '{ print $2 }'
```


```
who | awk '{ print $1, $5 }'
```

```
who | awk '{ print $5, $1 }'
```

分隔符可以不是空白，`-F`指定
```
sed 3q /etc/passwd | awk -F: '{ print $1 }'
```

## 打印

内置变量`NR`是当前输入记录或行的总数。
`$0`是整个输入行，未加更改。
```
awk '{ print NR, $0}'
```

`printf`控制输出格式
```
awk '{ printf "%4d %s\n", NR, $0 }'
```

## 模式

```
awk -F: '$2 == ""' /etc/passwd
```

- `$2 == ""`
- `$2 ~ /^$/`
- `$2 !~ /./`
- `lenght($2) == 0`

```
!($2 == "")
NF % 2 != 0 # 若为奇数则打印
lenght($0) > 72 { print "Line", NR, "too long:", substr($0, 1, 60) }
# substr(s,m,n) 起于m且具有n个字符长，若n被忽略，则使用从m到末尾的子串
```

```
date | awk '{ print substr($4, 1, 5) }'
```

## BEGIN 与 END 模式


`BEGIN`在第一个输入行之前就被执行：可以用`BEGIN`模式初始化变量，打印标题头或通过指定变量`FS`设置字段分隔符：

```
awk 'BEGIN { FS = ":" } $2 == "" ' /etc/passwd
```

`END`在处理完最后一行后执行：

```
awk 'END { print NR }'
```

## 算术运算与变量

求第一列中所有数字之和/平均数

```
{ s = s + $1 }      # 不必考虑初始化问题
END { print s, s/NR}
```

```
    { s += $1 }     # 类似c的速记
END { print s }
```

对输入行计数

```
    {
        nc += lenght($0)+ 1     # number of chars, 1 for \n
        nw += NF                # number of words
    }
END { print NR, nw, nc }
```

计算文件页数，每页66行

```
wc $* |
awk '!/totoal$/ { n += int(($1+55) / 56) }
    END     { print n }'
```

字符串变量被初始化为空字符串。

|   内置变量 | 说明                               |
|-----------:|:-----------------------------------|
| `FILENAME` | 当前输入文件名                     |
|       `FS` | 域分隔符（默认为空格和Tab）        |
|       `NF` | 输入记录中域的个数                 |
|       `NR` | 输入记录数                         |
|     `OFMT` | 数字的输出格式（默认为%g）         |
|      `OFS` | 输出域分隔符串（默认为空格）       |
|      `ORS` | 输出记录分隔符串（默认为换行符）   |
|       `RS` | 输入记录分隔符字符（默认为换行符） |

## 控制流

**example:** 查找相邻的、成对的、完全相同的单词

```
awk '
FILENAME != prevfile {      # new file
    NR = 1                  # reset line number
    prevfile = FILENAME
}
NF > 0 {
    if ($1 == lastword)
        printf "double %s, file %s, line %d\n", $1, FILENAME, NR
    for (i = 2; i <= NF; i++)
        if ($i == $(i-1))
            printf "double %s, file %s, line %d\n", $i , FILENAME, NR
    if (NR > 0)
        lastword = $NF
} ' $*
```

## 数组

**example:**将每个输入行收集到单个数组元素中，以行数为索引，然后以逆序将其打印输出

```
awk '
    { line[NR] = $0 }
END {
    for (i = NR; i > 0; i--)
        print line[i]
} ' $*
```

`n = split(s, arr, seq)`
将字符串`s`分成若干字段，并把这些字段分别保存在数组`arr`从`1`至`n`的元素中。若提供了分隔符字符`seq`，则使用它；否则，使用`FS`的当前值。

```
sed 1q /etc/passwd | awk '{
    split($0, a, ":");
    print a[1]
}'
```

```
echo 9/29/83 | awk '{
    split($0, date, "/");
    print date[3]
}'
```

`awk`的内置函数

|                内置函数 | 说明                                               |
|------------------------:|:---------------------------------------------------|
|            `cons(expr)` | `expr`的余弦                                       |
|             `exp(expr)` | `expr`的指数                                       |
|             `Getline()` | 读入下一个输入行，若是文件尾，则返回0;否则返回1    |
| `index(string, substr)` | `string `中字符串`substr`的位置，若不存在，则返回0 |
|             `int(expr)` | `expr`的整数部分                                   |
|             `lenght(s)` | 字符串`s`的长度                                    |
|             `log(expr)` | `expr`的自然对数                                   |
|             `sin(expr)` | `expr`的正弦                                       |
|        `Split(s, a, c)` | 以`c`为分隔符将`s`分隔至`a[1],...,a[n]`返回`n`     |
|      `sprintf(fmt,...)` | 根据格式`fmt`格式化                                |
|       `substr(s, m, n)` | 起始于位置`m`的字串`s`的`n`个字符的子串            |

## 关联数组

```
    { sum[$1] += $2 }
END {
    for (name in sum)
        print name, sum[name]
}
```


```
awk ' {
        for (i = 1; i <= NF; i++)
            num[$i]++
    }
    END {
        for (word in num)
            print word, num[word]
    } ' $*
```

## 字符串


**example:** 将较长的行调整为80列

```
awk '
BEGIN {
    N = 80              # fold at column 80
    for(i <= N; i++)    # make a string of blanks
        blanks = blanks " "
}
{
    if ((n = lenght($0) <= N))
        print
    else {
        for (i = 1; n > N; n -=) {
            printf "%s\\\n", substr($0, i, N)
            i += N
        }
        printf "%s%s\n", substr(blanks, 1, N-n), substr($0, i)
    }
} '
```

## 与shell的交互作用

```
awk '{ print $'$1' }'
```


```
awk "{ print \$$1 }"
```


**example:**addup n

```
awk '
    { s += $'$1' }
END { print s } '
```

**example:**对n个列中的每个列分别单独求和，然后再将各列之和相加

```
awk '
BEGIN {n = '$1'}
{
    for (i = 1; i <= n; i++)
        sum[i] += $i
}
END {
    for (i = 1; i <= n; i++) {
        printf "%6g", sum[i]
        totoal += sum[i]
    }
    printf "; totoal = %6g\n", totoal
} '
```

## 基于 awk 的日历服务

```
# calendar: version 3 -- today and tomorrow
awk < $HOME/calendar '
BEGIN {
    x = "Jan 31 Feb 28 Mar 31 Apr 30 May 31 Jun 30 " \
    "Jul 31 Aug 31 Seq 30 Oct 31 Nov 30 Dec 31 Jan 31"
    split(x, data)
    for (i = 1; i < 24; i += 2) {
        days[data[i]] = data[i+1]
        nextmon[data[i]] = data[i+2]
    }
    split("''"`date`"''", date)
    mon1 = date[2]; day1 = date[3]
    mon2 = mon1; day2 = day1 + 1
    if (day1 >= days[mon1]) {
        day2 = 1
        mon2 = nextmon[mon1]
    }
}
$1 == mon1 && $2 == day1 || $1 == mon2 && $2 == day2 ' | mail $NAME
```
