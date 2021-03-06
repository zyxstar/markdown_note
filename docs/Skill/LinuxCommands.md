# sort
```
1) 对文件内容进行排序，缺省分割符为空格，如果自定义需要使用-t选择，如-t:
2) 使用分隔符分割后，第一个field为0，awk中为1
3) 具体用法如下：
sort -t: sort_test.txt(缺省基于第一个field进行排序，field之间的分隔符为":")
sort -t: -r sort_test.txt(缺省基于第一个field进行倒序排序，field之间的分隔符为":")
sort -t: +1 sort_test.txt(基于第二个field进行排序，field之间的分隔符为":")
sort +3n sort_test.txt(基于第三个field进行排序，其中n选项提示是进行"数值型"排序) 已丢弃，请使用 -k
sort -u  sort_test.txt(去除文件中重复的行，同时基于整行进行排序)
sort -o output_file -t: +1.2[n] sort_text.txt(基于第二个field,同时从该field的第二个字符开始，这里n的作用也是"数值型"排序,并将结果输出到output_file中)
sort -t: -m +0 filename1 filename2(合并两个文件之后在基于第一个field排序)

sort命令行选项：
  -t  字段之间的分隔符
  -f  基于字符排序时忽略大小写
  -k  定义排序的域字段，或者是基于域字段的部分数据进行排序
  -m  将已排序的输入文件，合并为一个排序后的输出数据流
  -n  以整数类型比较字段
  -o outfile  将输出写到指定的文件
  -r  倒置排序的顺序为由大到小，正常排序为由小到大
  -u  只有唯一的记录，丢弃所有具有相同键值的记录
  -b  忽略前面的空格


-t定义了冒号为域字段之间的分隔符，-k 2指定基于第二个字段正向排序(字段顺序从1开始)。
sed -n '1,5p' /etc/passwd | sort -t: -k 1

还是以冒号为分隔符，这次是基于第三个域字段进行倒置排序。
sed -n '1,5p' /etc/passwd | sort -t: -k 3r

先以第六个域的第2个字符到第4个字符进行正向排序，在基于第一个域进行反向排序。
sort -t':' -k 6.2,6.4 -k 1r

先以第六个域的第2个字符到第4个字符进行正向排序，在基于第一个域进行正向排序。和上一个例子比，第4和第5行交换了位置。
sort -t':' -k 6.2,6.4 -k 1

基于第一个域的第2个字符排序
sort -t':' -k 1.2,1.2

基于第六个域的第2个字符到第4个字符进行正向排序，-u命令要求在排序时删除 键值 重复的行。
sort -t':' -k 6.2,6.4 -u users

```

# find
```
find pathname -options [-print -exec -ok]
 -print find命令将匹配的文件输出到标准输出。
 -exec find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' {} \;，注意{}和\；之间的空格，同时两个{}之间没有空格,注意一定有分号结尾。
 -ok 和-exec的作用相同，只不过以一种更为安全的模式来执行该参数所给出的shell命令，在执行每一个命令之前，都会给出提示，让用户来是否执行

find . -name "datafile*" -ctime -1 -exec ls -l {} \; 找到文件名为datafile*, 同时创建实际为1天之内的文件, 然后显示他们的明细.
find . -name   基于文件名查找,但是文件名的大小写敏感.
find . -iname  基于文件名查找,但是文件名的大小写不敏感.
find . -maxdepth 2 -name fred 找出文件名为fred,其中find搜索的目录深度为2(距当前目录), 其中当前目录被视为第一层.
find . -perm 644 -maxdepth 3 -name "datafile*"  (表示权限为644的, 搜索的目录深度为3, 名字为datafile*的文件)
find . -path "./rw" -prune -o -name "datafile*" 列出所有不在./rw及其子目录下文件名为datafile*的文件。
find . -path "./dir*" 列出所有符合dir*的目录及其目录的文件.
find . \( -path "./d1" -o -path "./d2" \) -prune -o -name "datafile*" 列出所有不在./d1和d2及其子目录下文件名为datafile*的文件。

find . -user ydev 找出所有属主用户为ydev的文件。
find . ! -user ydev 找出所有属主用户不为ydev的文件， 注意!和-user之间的空格。
find . -nouser    找出所有没有属主用户的文件，换句话就是，主用户可能已经被删除。
find . -group ydev 找出所有属主用户组为ydev的文件。
find . -nogroup    找出所有没有属主用户组的文件，换句话就是，主用户组可能已经被删除。

find . -mtime -3[+3] 找出修改数据时间在3日之内[之外]的文件。
find . -mmin  -3[+3] 找出修改数据时间在3分钟之内[之外]的文件。
find . -atime -3[+3] 找出访问时间在3日之内[之外]的文件。
find . -amin  -3[+3] 找出访问时间在3分钟之内[之外]的文件。
find . -ctime -3[+3] 找出修改状态时间在3日之内[之外]的文件。
find . -cmin  -3[+3] 找出修改状态时间在3分钟之内[之外]的文件。

find . -newer file     找出所有比file的更改时间更新的文件
find . ! -newer file 找出所有比file的更改时间更老的文件
find . -newer eldest_file ! -newer newest_file 找出文件的更改时间 between eldest_file and newest_file。

find . -type d    找出文件类型为目录的文件。
find . ! -type d  找出文件类型为非目录的文件。
       b - 块设备文件。
       d - 目录。
       c - 字符设备文件。
       p - 管道文件。
       l - 符号链接文件。
       f - 普通文件。

find . -size [+/-]100[c/k/M/G] 表示文件的长度为等于[大于/小于]100块[字节/k/M/G]的文件。
find . -empty 查找所有的空文件或者空目录.
find . -type f | xargs grep "ABC"  使用xargs和-exec的区别是, -exec可能会为每个搜索出的file,启动一个新的进程执行-exec的操作, 而xargs都是在一个进程内完成, 效率更高.
```



# cut
```
cut [options] file1 file2  #从一个或多个文件中删除所选列或字段
  -b byte 指定剪切字节数
  -c list 指定剪切字符数。
  -f field 指定剪切域数。
  -d 指定与空格和tab键不同的域分隔符。
  -c 用来指定剪切范围，如下所示：
  -c1,5-7 剪切第1个字符，然后是第5到第7个字符。
  -c2- 剪切第2个到最后一个字符
  -c-5 剪切最开始的到第5个字符
  -c1-50 剪切前50个字符。
  -f 格式与-c相同。
  -f1,5 剪切第1域，第5域。
  -f1,10-12 剪切第1域，第10域到第12域。

cut -d: -f3 cut_test.txt (基于":"作为分隔符，同时返回field 3中的数据) *field从0开始计算。
cut -d: -f1,3 cut_test.txt (基于":"作为分隔符，同时返回field 1和3中的数据)
cut -d: -c1,5-10 cut_test.txt(返回第1个和第5-10个字符)

cut只擅长处理“以一个字符间隔”的文本内容
```

# grep
```
grep --color 高亮显示
grep时应该把正则放到引号中(单引号优于双引号)，否则shell将它们作为文件名来解释
grep的可选项(以下只能3选1)
  -G默认动作，搜索模式作为基本正则来解释
  -E搜索模式作为扩展正则来解释
  -F搜索模式作为简单字符串来解释

fgrep == grep -F
egrep == grep -E
使用egrep不需要来转义花括号，加号等
gerp '[1-9]\{1,2\}' file
==
egrep '[1-9]{1,2}' file

grep其它可选项规定如何返回输出
  -c输出字符串行数，而不是行本身
  -h显示所有匹配的行（默认）
  -l只返回包含模式的文件的名称
  -L列出不包含匹配行文件的名称
  -n显示行及其行号
  -s避免返回目录和设备文件的错误消息
  -v指示输出不匹配模式的任何行
  -x显示那些模式匹配整行的行
  -d read(默认)|skip|resurse指示如何处理目录
  -f file从文件中取模式列表
  -i大小写不敏感
  -q找到第一个实例后停止扫描

grep中的file可以使用文件名扩展中任何通配符，因为它由shell解释
ps aux|gerp service
```

# xargs
```
xargs是给命令传递参数的一个过滤器，也是组合多个命令的一个工具。它把一个数据流分割为一些足够小的块，以方便过滤器和命令进行处理。通常情况下，xargs从管道或者stdin中读取数据，但是它也能够从文件的输出中读取数据。xargs的默认命令是echo，这意味着通过管道传递给xargs的输入将会包含换行和空白，不过通过xargs的处理，换行和空白将被空格取代。

xargs 是一个强有力的命令，它能够捕获一个命令的输出，然后传递给另外一个命令，下面是一些如何有效使用xargs 的实用例子。

1. 当你尝试用rm 删除太多的文件，你可能得到一个错误信息：/bin/rm Argument list too long. 用xargs 去避免这个问题
find ~ -name '*.log' -print0 | xargs -0 rm -f

2. 获得/etc/ 下所有*.conf 结尾的文件列表，有几种不同的方法能得到相同的结果，下面的例子仅仅是示范怎么实用xargs ，在这个例子中实用 xargs将find 命令的输出传递给ls -l
# find /etc -name "*.conf" | xargs ls –l

3. 假如你有一个文件包含了很多你希望下载的URL, 你能够使用xargs 下载所有链接
# cat url-list.txt | xargs wget –c

4. 查找所有的jpg 文件，并且压缩它
# find / -name *.jpg -type f -print | xargs tar -cvzf images.tar.gz

5. 拷贝所有的图片文件到一个外部的硬盘驱动
# ls *.jpg | xargs -n1 -i cp {} /external-hard-drive/directory
```


# sed
```
sed复制原文件并修改新文件，默认发送给标准输出，也可以保存到其它文件中
可用于
  自动编辑文件
  简化对多个文件的重复编辑
  编写转换程序

sed是一次读取一行，每行在移到下行前都获得了对其应用的所有命令
两种语法
 sed options editing_instruction file(s)
  >filename 附加到命令结尾将让输出发送到这个文件
  options
    -e 告诉下个参数是一个编辑指令，除非有多个指令，否则不需要此选项
    -n 防止在应用指令后把输入行送到标准输出
  editing_instruction 格式为address,[adderss]!actions arguments
    address规定要操作的行，可以是行号、表示文件最后一行的$、括在正斜线中的正则，如何未指定则应用每行输入
    ,[adderss]地址结束，如果指定了它，要应用的行的范围是上面的开始到这里指定的结束
    actions arguments
      a\string把字串追加到行后
      c\string修改行并用字符串替换
      d删除行
      i\string插到行前
      p显示行
      s进行替换 s/oldstr/newstr/[g] g代表替换所有，默认只替换第一个

 sed options -f scriptfile file(s)




sed的命令和选项：
a\   在当前行的后面加入一行或者文本。
c\   用新的文本改变或者替代本行的文本。
d    从pattern space位置删除行。
i\   在当前行的上面插入文本。
h    拷贝pattern space的内容到holding buffer(特殊缓冲区)。
H    追加pattern space的内容到holding buffer。
g    获得holding buffer中的内容，并替代当前pattern space中的文本。
G    获得holding buffer中的内容，并追加到当前pattern space的后面。
n    读取下一个输入行，用下一个命令处理新的行而不是用第一个命令。
p    打印pattern space中的行。
P    打印pattern space中的第一行。
q    退出sed。
w file   写并追加pattern space到file的末尾。
!    表示后面的命令对所有没有被选定的行发生作用。
s/re/string  用string替换正则表达式re。
=    打印当前行号码。
替换标记
g    行内全面替换，如果没有g，只替换第一个匹配。
p    打印行。
x    互换pattern space和holding buffer中的文本。
y    把一个字符翻译为另一个字符(但是不能用于正则表达式)。
选项
-e   允许多点编辑。
-n   取消默认输出。


/> cat testfile
northwest       NW      Charles Main                3.0     .98     3       34
western         WE      Sharon Gray                 5.3     .97     5       23
southwest       SW      Lewis Dalsass               2.7     .8      2       18
southern        SO      Suan Chin                   5.1     .95     4       15
southeast       SE      Patricia Hemenway           4.0     .7      4       17
eastern         EA      TB Savage                   4.4     .84     5       20
northeast       NE      AM Main Jr.                 5.1     .94     3       13
north           NO      Margot Weber                4.5     .89     5       9
central         CT      Ann Stephens                5.7     .94     5       13

/> sed '/north/p' testfile #如果模板north被找到，sed除了打印所有行 之外，还有 打印匹配行。

#-n选项取消了sed的默认行为。在没有-n的时候，包含模板的行被打印两次，但是在使用-n的时候将 只 打印包含模板的行。
/> sed -n '/north/p' testfile

删除
/> sed '3d' testfile  #第三行被删除，其他行默认输出到屏幕。
/> sed '3,$d' testfile  #从第三行删除到最后一行，其他行被打印。$表示最后一行。
/> sed '$d' testfile    #删除最后一行，其他行打印。
/> sed '/north/d' testfile #删除所有包含north的行，其他行打印。

#s表示替换，g表示命令作用于整个当前行。如果该行存在多个west，都将被替换为north，如果没有g，则只是替换第一个匹配。没有p不打印
/> sed 's/west/north/g' testfile

/> sed -n 's/^west/north/p' testfile #-n表示只打印匹配行，如果某一行的开头是west，则替换为north。 注意p


#&符号表示替换字符串中被找到的部分。所有以两个数字结束的行，最后的数字都将被它们自己替换，同时追加.5。
/> sed 's/[0-9][0-9]$/&.5/' testfile            &是占位符

/> sed -n 's/Hemenway/Jones/gp' testfile  #所有的Hemenway被替换为Jones。-n选项加p命令则表示只打印匹配行。

#模板Mar被包含在一对括号中，并在特殊的寄存器中保存为tag 1，它将在后面作为\1替换字符串，Margot被替换为Marlianne。
/> sed -n 's/\(Mar\)got/\1lianne/p' testfile     1也是占位符


#s后面的字符一定是分隔搜索字符串和替换字符串的分隔符，默认为斜杠，但是在s命令使用的情况下可以改变。不论什么字符紧跟着s命令都认为是新的分隔符。这个技术在搜索含斜杠的模板时非常有用，例如搜索时间和路径的时候。
/> sed 's#3#88#g' testfile

#所有在模板west和east所确定的范围内的行都被打印，如果west出现在esst后面的行中，从west开始到下一个east，无论这个east出现在哪里，二者之间的行都被打印，即使从west开始到文件的末尾还没有出现east，那么从west到末尾的所有行都将打印。
/> sed -n '/west/,/east/p' testfile

#-e选项表示多点编辑。第一个编辑命令是删除第一到第三行。第二个编辑命令是用Jones替换Hemenway。
/> sed -e '1,3d' -e 's/Hemenway/Jones/' testfile

/> sed -n '/north/w newfile' testfile #将所有匹配含有north的行写入newfile中。

/> sed '/eastern/i\ NEW ENGLAND REGION' testfile #i是插入命令，在匹配模式行前插入文本。

#找到匹配模式eastern的行后，执行后面花括号中的一组命令，每个命令之间用逗号分隔，n表示定位到匹配行的下一行，s/AM/Archie/完成Archie到AM的替换，p和-n选项的合用，则只是打印作用到的行。
/> sed -n '/eastern/{n;s/AM/Archie/;p}' testfile

-e表示多点编辑，第一个编辑命令y将前三行中的所有小写字母替换为大写字母，-n表示不显示替换后的输出，第二个编辑命令将只是打印输出转换后的前三行。注意y不能用于正则。
/> sed -n -e '1,3y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/' -e '1,3p' testfile

/> sed '2q' testfile  #打印完第二行后退出。

#当模板Lewis在某一行被匹配，替换命令首先将Lewis替换为Joseph，然后再用q退出sed。
/> sed '/Lewis/{s/Lewis/Joseph/;q;}' testfile

G命令告诉sed从holding buffer中取得该行，然后把它放回到pattern space中，且追加到现在已经存在于模式空间的行的末尾。
/> sed -e '/northeast/h' -e '$G' testfile

WE所在的行被移动并追加到包含CT行的后面。
/> sed -e '/WE/{h;d;}' -e '/CT/{G;}' testfile

第二行到第三行间的所有WE被替换为wee
/> sed -n '2,3s/WE/wee/p' testfile

判断空格与tab键
/> sed -n l tab_space.txt



删除某行
     /> sed '1d' ab             #删除第一行
     /> sed '$d' ab             #删除最后一行
     /> sed '1,2d' ab           #删除第一行到第二行
     /> sed '2,$d' ab           #删除第二行到最后一行

显示某行
     /> sed -n '1p' ab          #显示第一行
     /> sed -n '$p' ab          #显示最后一行
     /> sed -n '1,2p' ab        #显示第一行到第二行
     /> sed -n '2,$p' ab        #显示第二行到最后一行
     /> sed -n '/zyx/,/zhou/p' /etc/passwd

使用模式进行查询
     /> sed -n '/ruby/p' ab     #查询包括关键字ruby所在所有行
     /> sed -n '/\$/p' ab       #查询包括关键字$所在所有行，使用反斜线\屏蔽特殊含义

增加一行或多行字符串
     /> sed '1a drink tea' ab   #第一行后增加字符串"drink tea"
     /> sed '1,3a drink tea' ab #第一行到第三行后增加字符串"drink tea"
     /> sed '1a drink tea\nor coffee' ab   #第一行后增加多行，使用换行符\n

代替一行或多行
     /> sed '1c Hi' ab          #第一行代替为Hi
     /> sed '1,2c Hi' ab        #第一行到第二行代替为Hi

就地插入
     /> sed -i '$a bye' ab         #在文件ab中最后一行直接输入"bye"

替换两个或多个空格为一个空格
     /> sed 's/[ ][ ]*/ /g' file_name

替换两个或多个空格为分隔符:
     /> sed 's/[ ][ ]*/:/g' file_name

如果空格与tab共存时用下面的命令进行替换
替换成空格
     /> sed 's/[[:space:]][[:space:]]*/ /g' filename

替换成分隔符:
     /> sed 's/[[:space:]][[:space:]]*/:/g' filename

替换单引号为空
     /> sed 's/'"'"'//g'
     /> sed 's/'\''//g'
     /> sed s/\'//g

快速一行命令:
    's//.$//g'         删除以句点结尾行
    '-e /abcd/d'       删除包含abcd的行
    's/[][][]*/[]/g'   删除一个以上空格,用一个空格代替
    's/^[][]*//g'      删除行首空格
    's//.[][]*/[]/g'   删除句号后跟两个或更多的空格,用一个空格代替
    '/^$/d'            删除空行
    's/^.//g'          删除第一个字符,区别  's//.//g'删除所有的句点
    's/COL/(.../)//g'  删除紧跟COL的后三个字母
    's/^////g'         删除路径中第一个/
```

# awk
```
awk的基本格式：
    /> awk 'pattern' filename
    /> awk '{action}' filename
    /> awk 'pattern {action}' filename

    /> awk '/Mary/' employees   #打印所有包含模板Mary的行。

    打印文件中的第一个字段，这个域在每一行的开始，缺省由空格或其它分隔符。
    /> awk '{print $1}' employees
    /> awk '/Sally/{print $1, $2}' employees #打印包含模板Sally的行的第一、第二个域字段。

awk的格式输出：
    awk中同时提供了print和printf两种打印输出的函数，其中print函数的参数可以是变量、数值或者字符串。字符串必须用双引号引用，参数用逗号分隔。如果没有逗号，参数就串联在一起而无法区分。这里，逗号的作用与输出文件的分隔符的作用是一样的，只是后者是空格而已。下面给出基本的转码序列：

    /> date | awk '{print "Month: " $2 "\nYear: ", $6}'
    Month: Oct
    Year:  2011

    /> awk '/Sally/{print "\t\tHave a nice day, " $1,$2 "\!"}' employees
    Have a nice day, Sally Chang!

    在打印数字的时候你也许想控制数字的格式，我们通常用printf来完成这个功能。awk的特殊变量OFMT也可以在使用print函数的时候，控制数字的打印格式。它的默认值是"%.6g"----小数点后面6位将被打印。
    /> awk 'BEGIN { OFMT="%.2f"; print 1.2456789, 12E-2}'
    1.25  0.12

格式化说明符  功能  示例  结果
    %c  打印单个ASCII字符。    printf("The character is %c.\n",x)  The character is A.
    %d  打印十进制数。 printf("The boy is %d years old.\n",y)  The boy is 15 years old.
    %e  打印用科学记数法表示的数。   printf("z is %e.\n",z)  z is 2.3e+01.
    %f  打印浮点数。  printf("z is %f.\n",z)  z is 2.300000
    %o  打印八进制数。 printf("y is %o.\n",y)  y is 17.
    %s  打印字符串。  printf("The name of the culprit is %s.\n",$1);  The name of the culprit is Bob Smith.
    %x  打印十六进制数。    printf("y is %x.\n",y)  y is f.
    注：假设列表中的变脸值为x = A, y = 15, z = 2.3, $1 = "Bob Smith"

    /> echo "Linux" | awk '{printf "|%-15s|\n", $1}'  # %-15s表示保留15个字符的空间，同时左对齐。
    |Linux          |

    /> echo "Linux" | awk '{printf "|%15s|\n", $1}'   # %-15s表示保留15个字符的空间，同时右对齐。
    |          Linux|

    %8d表示数字右对齐，保留8个字符的空间。
    /> awk '{printf "The name is %-15s ID is %8d\n", $1,$3}' employees

awk内置变量
    ARGC               命令行参数个数
    ARGV               命令行参数排列
    ENVIRON            支持队列中系统环境变量的使用
    FILENAME           awk浏览的文件名
    FNR                浏览文件的记录数
    FS                 设置输入域分隔符，等价于命令行 -F选项
    NF                 浏览记录的域的个数
    NR                 已读的记录数
    OFS                输出域分隔符
    ORS                输出记录分隔符
    RS                 控制记录分隔符

   此外,$0变量是指整条记录

   awk中默认的记录分隔符是回车，保存在其内建变量ORS和RS中。$0变量是指整条记录。
      /> awk '{print $0}' employees #这等同于print的默认行为。

   变量NR(Number of Record)，记录每条记录的编号。
      /> awk '{print NR, $0}' employees

   变量NF(Number of Field)，记录当前记录有多少域。
      /> awk '{print $0,NF}' employees

这里-F选项后面的字符表示分隔符。如果分隔符不是空白字符时)了
/> awk -F: '/root/{print $1,$2}' /etc/passwd


对于awk而言，其模式部分将控制这动作部分的输入，只有符合模式条件的记录才可以交由动作部分基础处理，而模式部分不仅可以写成正则表达式(如上面的例子)，awk还支持条件表达式，如：
/> awk '$3 < 4000 {print}' employees

模式和动作一般是捆绑在一起的。需要注意的是，动作是花括号内的语句。模式控制的动作是从第一个左花括号开始到第一个右花括号结束，如下：
/> awk '$3 < 4000 && /Sally/ {print}' employees

" ~ " 用来在记录或者域内匹配正则表达式。
 /> awk '$1 ~ /[Bb]ill/' employees      #显示所有第一个域匹配Bill或bill的行。

 /> awk '$1 !~ /[Bb]ill/' employees     #显示所有第一个域不匹配Bill或bill的行，其中!~表示不匹配的意思。
 /> awk '/^north/' testfile            #打印所有以north开头的行。

 /> awk '/^(no|so)/' testfile          #打印所有以so和no开头的行。

 /> awk '$5 ~ /\.[7-9]+/' testfile     #第五个域字段匹配包含.(点)，后面是7-9的数字。
 /> awk '$8 ~ /[0-9][0-9]$/{print $8}' testfile  #第八个域以两个数字结束的打印。

原地修改
awk '{print $1>"urfile"}' urfile
而不是
awk '{print $1}' urfile >urfile #重定向输出部分先执行， 即先把urfile清空了。

BEGIN 和 END 模式
cat /etc/passwd |awk  -F ':'  'BEGIN {print "name,shell"}  {print $1","$7} END {print "blue,/bin/nosh"}'


awk编程
变量和赋值

除了awk的内置变量，awk还可以自定义变量。

下面统计/etc/passwd的账户人数

awk '{count++;print $0;} END{print "user count is ", count}' /etc/passwd
root:x:0:0:root:/root:/bin/bash
......
user count is  40
count是自定义变量。之前的action{}里都是只有一个print,其实print只是一个语句，而action{}可以有多个语句，以;号隔开。


这里没有初始化count，虽然默认是0，但是妥当的做法还是初始化为0:

awk 'BEGIN {count=0;print "[start]user count is ", count} {count=count+1;print $0;} END{print "[end]user count is ", count}' /etc/passwd
[start]user count is  0
root:x:0:0:root:/root:/bin/bash
...
[end]user count is  40


统计某个文件夹下的文件占用的字节数
ls -l |awk 'BEGIN {size=0;} {size=size+$5;} END{print "[end]size is ", size}'
[end]size is  8657198


如果以M为单位显示:
ls -l |awk 'BEGIN {size=0;} {size=size+$5;} END{print "[end]size is ", size/1024/1024,"M"}'
[end]size is  8.25889 M
注意，统计不包括文件夹的子目录。

条件语句
 awk中的条件语句是从C语言中借鉴来的，见如下声明方式：

if (expression) {
    statement;
    statement;
    ... ...
}

if (expression) {
    statement;
} else {
    statement2;
}

if (expression) {
    statement1;
} else if (expression1) {
    statement2;
} else {
    statement3;
}


统计某个文件夹下的文件占用的字节数,过滤4096大小的文件(一般都是文件夹):

ls -l |awk 'BEGIN {size=0;print "[start]size is ", size} {if($5!=4096){size=size+$5;}} END{print "[end]size is ", size/1024/1024,"M"}'
[end]size is  8.22339 M

循环语句
awk中的循环语句同样借鉴于C语言，支持while、do/while、for、break、continue，这些关键字的语义和C语言中的语义完全相同。

数组
  因为awk中数组的下标可以是数字和字母，数组的下标通常被称为关键字(key)。值和关键字都存储在内部的一张针对key/value应用hash的表格里。由于hash不是顺序存储，因此在显示数组内容时会发现，它们并不是按照你预料的顺序显示出来的。数组和变量一样，都是在使用时自动创建的，awk也同样会自动判断其存储的是数字还是字符串。一般而言，awk中的数组用来从记录中收集信息，可以用于计算总和、统计单词以及跟踪模板被匹配的次数等等。

显示/etc/passwd的账户

awk -F ':' 'BEGIN {count=0;} {name[count] = $1;count++;}; END{for (i = 0; i < NR; i++) print i, name[i]}' /etc/passwd
0 root
1 daemon
2 bin
3 sys
4 sync
5 games
......
这里使用for循环遍历数组


环境变量获取
> echo $LS_COLORS|awk '{split($0, arr, ":")} END {for(i=0;i<length(arr);i++) print arr[i]}'
> echo $LS_COLORS|tr -s ":" "\n"




```

# crontab
```
usage:  crontab [-u user] file
        crontab [-u user] [ -e | -l | -r ]
                (default operation is replace, per 1003.2)
        -e      (edit user's crontab)
        -l      (list user's crontab)
        -r      (delete user's crontab)
        -i      (prompt before deleting user's crontab)

文件格式如下(每个列之间是使用空格分开的):
       第1列分钟1～59
       第2列小时1～23（0表示子夜）
       第3列日1～31
       第4列月1～12
       第5列星期0～6（0表示星期天）
       第6列要运行的命令

       分 时 日 月 星期 要运行的命令

       30 21* * * /apps/bin/cleanup.sh
       上面的例子表示每晚的21:30运行/apps/bin目录下的cleanup.sh。
       45 4 1,10,22 * * /apps/bin/backup.sh
       上面的例子表示每月1、10、22日的4:45运行/apps/bin目录下的backup.sh。
       10 1 * * 6,0 /bin/find -name "core" -exec rm {} \;
       上面的例子表示每周六、周日的1:10运行一个find命令。
       0,30 18-23 * * * /apps/bin/dbcheck.sh
       上面的例子表示在每天18:00至23:00之间每隔30分钟运行/apps/bin目录下的dbcheck.sh。
       0 23 * * 6 /apps/bin/qtrend.sh
       上面的例子表示每星期六的11:00pm运行/apps/bin目录下的qtrend.sh。

       -u 用户名。
       -e 编辑crontab文件。
       -l 列出crontab文件中的内容。
       -r 删除crontab文件。
       系统将在/var/spool/cron/目录下自动保存名为<username>的cron执行脚本.
       cron是定时完成的任务, 在任务启动时,一般来讲都是重新启动一个新的SHELL, 因此当需要使用登录配置文件的信息,特别是环境变量时,是非常麻烦的.
       一般这种问题的使用方法如下:
       0 2 * * * ( su - USERNAME -c "export LANG=en_US; /home/oracle/yb2.5.1/apps/admin/1.sh"; ) > /tmp/1.log 2>&1
       如果打算执行多条语句, 他们之间应使用分号进行分割. 注: 以上语句必须在root的帐户下执行.
```


# join
```
join---实现两个文件中记录的连接操作，连接操作将两个文件中具有相同域的记录选择出来，再将这些记录所有的域放到一行（包含来自两个文件的所有域）
join [选项] 文件1 文件2
选项 意义
-a1或-a2 除了显示以共同域进行连接的结果外，-a1表示还显示第1个文件中没有共同域的记录，-a2则表示显示第2个文件中没有共同域的记录
-i 比较域内容时，忽略大小写差异
-o 设置结果显示的格式
-t 改变域分隔符
-v1或-v2  跟-a选项类似，但是，不显示以共同域进行连接的结果

-1和-2 -1用于设置文件1用于连接的域，-2用于设置文件2用于连接的域
当两个文件进行连接时，文件1中的记录可能在文件2中找不到共同域，反过来，文件2中也可能存这在样的记录，join命令的结果默认是不显示这些未进行连接的记录的
-a和-v选项就是用于显示这些未进行连接的记录，-a1和-v1指显示文件1中的未连接记录，而-a2和-v2指显示文件2中的未连接记录
-a和-v选项的区别在于：-a选项显示以共同域进行连接的结果，而-v选项则不显示这些记录

当两个文件进行连接时，文件1中的记录可能在文件2中找不到共同域，反过来，文件2中也可能存在这样的记录，join命令的结果默认是不显示这些未进行连接的记录的
-a和-v选项就是用于显示这些未进行连接的记录，-a1和-v1指显示文件1中的未连接记录，而-a2和-v2指显示文件2中的未连接记录
-a和-v选项的区别在于：-a选项显示以共同域进行连接的结果，而-v选项则不显示这些记录

join命令默认显示连接记录在两个文件中的所有域，而且是按顺序来显示的。-o选项用于改变结果显示的格式
join命令默认比较文件1和文件2的第1域，如果我们需要通过其他域进行连接，就需要使用-1和-2选项，-1用于设置文件1用于连接的域，-2用于设置文件2用于连接的域
join -t: -i -1 3 -2 1 TEACHER1.db TEACHER_HOBBY.db
```

# paste
```
paste命令用于将文本文件或标准输出中的内容粘贴到新的文件，它可以将来自于不同文件的数据粘贴到一起，形成新的文件
paste  [选项] 文件1 文件2
选项 意义
-d 默认域分隔符是空格或Tab键，设置新的域分隔符
-s 将每个文件粘贴成一行
- 从标准输入中读取数据

paste命令的“-”选项比较特殊，当paste命令从标准输入中读取数据时，“-”选项才起作用
eg：[root@jselab shell-book]# ls | paste -d" " - - - - -                  #从标准输入读取数据
anotherres.sh array_eval2.sh colon.sh example execerr.sh          #每行显示5个文件名
execin.sh exec.sh FILE1 FILE2 forever.sh
hfile loggg loggg1 loopalias.sh matrix.sh
newfile nokillme.sh part1 part2 part3
parttotal refor.sh reif.sh selfkill.sh sleep10.sh
sleep55.sh stack.sh subsenv.sh subsep.sh subsig.sh
subsparallel.sh subspipe.sh subsvar.sh TEACHER.db test.sh
testvar.sh traploop.sh
```

# uniq
```
– c 显示输出中，在每行行首加上本行在文件中出现的次数。它可取代- u和- d选项。
– d 只显示重复行。
– u 只显示文件中不重复的各行。
- D 显示所有重复的行，每个重复的行都显示

uniq -f 1 test       #忽略每行的第一个字段
uniq -f 2 -s 2 test  #忽略每行的前2个字段，忽略第二 个空白字符和第三个字段的首字符
```


# dd
```
dd可以处理原始的,未格式化的的数据
使用dd这个linux命令可以创建一定大小文件。

linux创建文件命令：dd命令
把指定的输入文件拷贝到指定的输出文件中，并且在拷贝的过程中可以进行格式转换。语法：
CODE:[Copy to clipboard]dd 〔选项〕
QUOTE:
if =输入文件(或设备名称)。
of =输出文件(或设备名称)。
ibs = bytes 一次读取bytes字节，即读入缓冲区的字节数。
skip = blocks 跳过读入缓冲区开头的ibs*blocks块。
obs = bytes 一次写入bytes字节，即写 入缓冲区的字节数。
bs = bytes 同时设置读/写缓冲区的字节数(等于设置obs和obs)。
cbs = bytes 一次转换bytes字节。
count = blocks 只拷贝输入的blocks块。
conv = ASCII 把EBCDIC码转换为ASCII码。
conv = ebcdic 把ASCII码转换为EBCDIC码。
conv = ibm 把ASCII码转换为alternate EBCDIC码。
conv = blick 把变动位转换成固定字符。
conv = ublock 把固定们转换成变动位
conv = ucase 把字母由小写变为大写。
conv = lcase 把字母由大写变为小写。
conv = notrunc 不截短输出文件。
conv = swab 交换每一对输入字节。
conv = noerror 出错时不停止处理。
conv = sync 把每个输入记录的大小都调到ibs的大小(用ibs填充)。
fdformat命令
低级格式化软盘。
实例:
创建一个100M的空文件
dd if=/dev/zero of=hello.txt bs=100M count=1
以上是linux创建文件命令：dd的用法。



2.实例分析
2.1.数据备份与恢复
2.1.1整盘数据备份与恢复
备份
将本地的/dev/hdx整盘备份到/dev/hdy ：dd if=/dev/hdx of=/dev/hdy
将/dev/hdx全盘数据备份到指定路径的image文件：dd if=/dev/hdx of=/path/to/image
备份/dev/hdx全盘数据，并利用gzip工具进行压缩，保存到指定路径：dd if=/dev/hdx | gzip
>/path/to/image.gz
恢复
将备份文件恢复到指定盘：dd if=/path/to/image of=/dev/hdx
将压缩的备份文件恢复到指定盘 ：gzip -dc /path/to/image.gz | dd of=/dev/hdx
2.1.2.利用netcat远程备份
在源主机上执行此命令备份/dev/hda：dd if=/dev/hda bs=16065b | netcat < targethost-IP >
1234在目的主机上执行此命令来接收数据并写入/dev/hdc：netcat -l -p 1234 | dd of=/dev/hdc
bs=16065b
以下两条指令是目的主机指令的变化分别采用bzip2 gzip对数据进行压缩，并将备份文件保存在当
前目录 ：
netcat -l -p 1234 | bzip2 > partition.img
netcat -l -p 1234 | gzip > partition.img
2.1.3.备份MBR
备份：
备份磁盘开始的512Byte大小的MBR信息到指定文件：dd if=/dev/hdx of=/path/to/image
count=1 bs=512
恢复：
将备份的MBR信息写到磁盘开始部分：dd if=/path/to/image of=/dev/hdx
2.1.4.备份软盘
将软驱数据备份到当前目录的disk.img文件：dd if=/dev/fd0 of=disk.img count=1 bs=1440k
2.1.5.拷贝内存资料到硬盘
将内存里的数据拷贝到root目录下的mem.bin文件：dd if=/dev/mem of=/root/mem.bin
bs=1024
2.1.6.从光盘拷贝iso镜像
拷贝光盘数据到root文件夹下，并保存为cd.iso文件：dd if=/dev/cdrom of=/root/cd.iso
2.2.增加Swap分区文件大小
创建一个足够大的文件（此处为256M）：dd if=/dev/zero of=/swapfile bs=1024 count=262144
把这个文件变成swap文件：mkswap /swapfile
启用这个swap文件：swapon /swapfile
在每次开机的时候自动加载swap文件, 需要在 /etc/fstab 文件中增加一行：/swapfile swap
swap defaults 0 0
2.3.销毁磁盘数据
利用随机的数据填充硬盘：dd if=/dev/urandom of=/dev/hda1
在某些必要的场合可以用来销毁数据。执行此操作以后，/dev/hda1将无法挂载，创建和拷贝操作
无法执行。
2.4磁盘管理
2.4.1.得到最恰当的block size
通过比较dd指令输出中所显示的命令执行时间，即可确定系统最佳的block size大小：
dd if=/dev/zero bs=1024 count=1000000 of=/root/1Gb.filedd if=/dev/zero bs=2048 count=500000 of=/root/1Gb.file
dd if=/dev/zero bs=4096 count=250000 of=/root/1Gb.file
dd if=/dev/zero bs=8192 count=125000 of=/root/1Gb.file
2.4.2测试硬盘读写速度
通过两个命令输出的执行时间，可以计算出测试硬盘的读／写速度：
dd if=/root/1Gb.file bs=64k | dd of=/dev/null
hdd if=/dev/zero of=/root/1Gb.file bs=1024 count=1000000
2.4.3.修复硬盘
当硬盘较长时间（比如一两年年）放置不使用后，磁盘上会产生magnetic flux point。当磁头读到
这些区域时会遇到困难，并可能导致I/O错误。当这种情况影响到硬盘的第一个扇区时，可能导致
硬盘报废。下面的命令有可能使这些数据起死回生。且这个过程是安全，高效的。
dd if=/dev/sda of=/dev/sda
```

