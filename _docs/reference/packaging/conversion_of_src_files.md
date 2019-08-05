# .src文件的转换

NumPy distutils支持自动转换名为< somefile> .src的源文件。 此工具可用于维护非常相似的代码块，只需要在块之间进行简单的更改。 在安装的构建阶段，如果遇到名为< somefile> .src的模板文件，则会从模板构造一个名为< somefile>的新文件，并将其放在构建目录中以代替使用。 支持两种形式的模板转换。 第一种形式出现在名为< file> .ext.src的文件中，其中ext是可识别的Fortran扩展名(f, f90, f95, f77, for, ftn, pyf)。第二种形式用于所有其他情况。

## Fortran文件

此模板转换器将根据‘<…>’中的规则复制文件中名称包含‘<…>’的所有函数和子例程块。 ‘<…>’中以逗号分隔的单词数决定了块重复的次数。 这些单词的含义表示在每个块中应该替换重复规则‘<…>’。 块中的所有重复规则必须包含相同数量的逗号分隔的单词，表示该块应重复的次数。 如果重复规则中的单词需要逗号，左箭头或右箭头，则在前面加上反斜杠''。 如果重复规则中的单词与'\ < index>'匹配，则它将替换为相同重复规范中的< index> -th字。 重复规则有两种形式：命名和短。

### 命名重复规则

当必须在块中多次使用同一组重复时，命名重复规则很有用。 它使用< rule1 = item1, item2, item3, …, itemN>指定，其中N是块应重复的次数。在块的每次重复时，整个表达式  ‘<…>’ 将首先替换为item1，然后替换为item2，依此类推，直到完成N次重复。 一旦引入了命名重复规范，通过仅引用名称（即< rule1>），可以在当前块中使用相同的重复规则。

### 短重复规则

短重复规则看起来像< item1, item2, item3, …, itemN>。 该规则指定整个表达式 ‘<…>’应首先替换为item1，然后替换为item2，依此类推，直到完成N次重复。

### 预定义的名称

可以使用以下预定义的命名重复规则：

- < prefix=s,d,c,z>
- <_c=s,d,c,z>
- <_t=real, double precision, complex, double complex>
- < ftype=real, double precision, complex, double complex>
- < ctype=float, double, complex_float, complex_double>
- < ftypereal=float, double precision, \0, \1>
- < ctypereal=float, double, \0, \1>

## 其他的文件

非Fortran文件使用单独的语法来定义应使用类似于Fortran特定重复的命名重复规则的变量扩展重复的模板块。 这些文件的模板规则是：

1. ‘/** begin repeat ’在一行上标记了应该重复的段的开头。
2. 命名变量扩展使用 #name = item1, item2, item3, ..., itemN#定义并放置在连续的行上。这些变量在每个重复块中用相应的单词替换。 同一重复块中的所有命名变量必须定义相同数量的单词。
3. 在指定命名变量的重复规则时，项目* N是项目，项目，...的简写，项目重复N次。 另外，括号与* N组合可用于对应重复的若干项进行分组。 因此，＃name =（item1，item2）* 4＃相当于＃name = item1, item2, item1, item2, item1, item2,item1, item2＃
4. 一行上的 '* /' 标志着变量扩展命名的结束。 下一行是使用命名规则重复的第一行。
5. 在要重复的块内，应扩展的变量指定为@ name @。
6. 一行上的 '/** end repeat **/' 将前一行标记为要重复的块的最后一行。