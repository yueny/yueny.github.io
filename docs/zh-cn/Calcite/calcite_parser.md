# 解析器原理

# Calcite SQL 解析器
Calcite 默认使用 JavaCC 生成 SQL 解析器，可以很方便的将其替换为 Antlr 作为代码生成器 。
JavaCC 全称 Java Compiler Compiler，是一个开源的 Java 程序解析器生成器，生成的语法分析器采用递归下降语法解析，简
称 LL(K)。主要通过一些模版文件生成语法解析程序（例如根据 .jj 文件或者 .jjt 等文件生产代码）。


Calcite 的解析体系是将 SQL 解析成抽象语法树， 
Calcite 中使用 SqlNode 这种数据结构表示语法树上的每个节点。

