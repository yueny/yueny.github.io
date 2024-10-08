# 自定义语法的工程实践

# 模块介绍
* [parser-jj-generator](https://gitee.com/whosly/calcite-jj/tree/master/parser-jj-generator)
    * 根据 parser-jj 模板文件生成 parser-jj 代码文件
* [parser-generator](https://gitee.com/whosly/calcite-jj/tree/master/parser-generator)
    * 根据 parser-jj 代码文件文件生成  Parser Java代码
* [auto-generator](https://gitee.com/whosly/calcite-jj/tree/master/auto-generator)
    * 根据 parser-jj 模板文件生成 Parser Java代码(不需要对Parser.jj进行定制化修改)。
* [new-grammar](https://gitee.com/whosly/calcite-jj/tree/master/new-grammar)
    * 新增自定义语法的例子工程
        * CREATE MATERIALIZED VIEW [ IF NOT EXISTS ] view_name AS query
        * JACKY JOB 'query'
* [calcite-schema](https://gitee.com/whosly/calcite-jj/tree/master/calcite-schema)
  * 多种数据源加载的示例
  * 自定义语法 submit job as query 的示例


# 模块

## parser-jj-generator
根据 parser-jj 模板生成 parser-jj。

### 复制模板文件
从calcite源码包中，将code\src\main\codegen下所有文件复制到自己的代码路径下。

包含 config.fmpp、templates/Parser.jj（模板文件）

### 合并 config.fmpp
把 default_config.fmpp 中的parser属性与config.fmpp中的parser属性合并。

就可以通过mvn package命令生成可用的 Parser.jj（代码文件） 了。
当然，如果有定制化修改的需求，也可以在这个阶段修改config.fmpp

```
mvn clean package

# 此时会在 target\generated-sources\fmpp\javacc 下生成 Parser.jj（代码文件）
```


## parser-generator
将 parser-jj-generator 模块中生成的 Parser.jj 代码文件生成 Parser Java代码 (路径 target\generated-sources\fmpp\javacc)
copy至此项目中。

```
mvn clean initialize

mvn package
```

此时就可以直接进行编码开发工作了。
```java
import org.apache.calcite.avatica.util.Casing;
import org.apache.calcite.avatica.util.Quoting;
import org.apache.calcite.schema.SchemaPlus;
import org.apache.calcite.sql.SqlNode;
import org.apache.calcite.sql.parser.SqlParser;
import org.apache.calcite.sql.parser.impl.JackySqlParserImpl;
import org.apache.calcite.sql.validate.SqlConformanceEnum;
import org.apache.calcite.tools.FrameworkConfig;
import org.apache.calcite.tools.Frameworks;

public class Application {
    public static void main(String[] args) {
        SchemaPlus rootSchema = Frameworks.createRootSchema(true);

        final FrameworkConfig config = Frameworks.newConfigBuilder()
                .parserConfig(SqlParser.configBuilder()
                        .setParserFactory(JackySqlParserImpl.FACTORY)
                        .setCaseSensitive(false)
                        .setQuoting(Quoting.BACK_TICK)
                        .setQuotedCasing(Casing.TO_UPPER)
                        .setUnquotedCasing(Casing.TO_UPPER)
                        .setConformance(SqlConformanceEnum.ORACLE_12)
                        .build())
                .build();

        String sql = "select ids, name from test where id < 5";
        SqlParser parser = SqlParser.create(sql, config.getParserConfig());
        try {
            SqlNode sqlNode = parser.parseStmt();

            System.out.println(sqlNode.toString());

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### FAQ JackySqlParserImpl.java:16000:: 可能尚未初始化变量startNum

修改代码，设置初始化值null， 移除 final。


## auto-generator
如果不需要对Parser.jj进行定制化修改，那么可以通过连续运行两个插件， 根据 parser-jj 模板生成 Parser Java代码。

功能包含 parser-jj-generator + parser-generator。

```
mvn clean initialize

mvn package
```

## new-grammar

### JavaCC 编译插件配置
使用 FreeMarker 模版插件根据 config.fmpp 生成 parser.jj 文件，最后使用 JavaCC 编译插件生成最终的解析器代码。

#### 配置 FreeMarker 插件

配置方式见上一篇文章#FreeMarker-插件。

配置好后可以使用 mvn generate-resources 命令测试是否生成了新的 parser.jj 文件。


#### 配置 JavaCC 插件

配置方式见上一篇文章#JavaCC-插件。


### 复制模板文件
从calcite源码包中，将code\src\main\codegen 下所有文件复制到自己的代码路径下。包含 config.fmpp、templates/Parser.jj（模板文件）

### 合并 config.fmpp
把 default_config.fmpp 中的parser属性与config.fmpp中的parser属性合并。


### 编译
```
mvn clean initialize

mvn package

mvn generate-resources
```

### CREATE MATERIALIZED VIEW
在 Calcite 中新增语法的示例。 新增语法为 `CREATE MATERIALIZED VIEW`。

#### 语法格式
语法格式
```markdown
CREATE MATERIALIZED VIEW
[ IF NOT EXISTS ] view_name
AS query
```


#### 新增一种解析类

* 新建包 org.apache.calcite.sql
* 包 org.apache.calcite.sql 下新建类 [CreateMaterializedView](https://gitee.com/whosly/calcite-jj/tree/master/new-grammar/src/main/java/org/apache/calcite/sql/CreateMaterializedView.java)


#### 编写语法文件 parserImpls.ftl
在 codegen/includes 目录下新建 parserImpls.ftl 文件。

新增如下代码：

```java

SqlNode SqlCreateMaterializedView() :
{
    SqlParserPos pos;
    SqlIdentifier viewName;
    boolean existenceCheck = false;
    SqlSelect query;
}
{
    <CREATE> { pos = getPos(); }
    <MATERIALIZED> <VIEW>
    <#-- [] 代表里面的元素可能出现 -->
    [ <IF> <NOT> <EXISTS> { existenceCheck = true; } ]
    <#-- CompoundIdentifier() 为 Calcite 内置函数，
    可以解析类似 catalog.schema.tableName 这样的全路径表示形式 -->
    viewName = CompoundIdentifier()
    <AS>
    <#-- SqlSelect() 为 Calcite 内置函数，解析一个 select sql -->
    query = SqlSelect()
    {
        return new CreateMaterializedView(pos, viewName, existenceCheck, query);
    }
}
```

#### 配置 config.fmpp 文件
在 codegen 目录下修改 [config.fmpp](https://gitee.com/whosly/calcite-jj/tree/master/new-grammar/src/main/codegen/config.fmpp) 文件。定义解析器的包名和类型，声明新增的关键字和解析方法等。


data.parser 部分新增的全部代码如下：
```
data: {
  parser: {
    package: "org.apache.calcite.sql.parser.impl",
    class: "JackySqlParserImpl",

    implementationFiles: [
        "parserImpls.ftl"
    ]

    imports: [
        "org.apache.calcite.sql.CreateMaterializedView"
    ]

    keywords: [
        "IF",
        "MATERIALIZED"
    ]

    statementParserMethods: [
        "SqlCreateMaterializedView()"
    ]
  }
}

freemarkerLinks: {
  includes: includes/
}
```

每个节点的解释见 [上一篇文章#合并-config.fmpp](./calcite_parser_JJ.md#合并-configfmpp)。


#### 运行编译

```
mvn clean generate-resources
```

#### 测试 Case

测试代码
```
@Test
public void test() throws SqlParseException {
    String sql = "CREATE MATERIALIZED VIEW IF NOT EXISTS "test"."demo"."materializationName" AS SELECT * FROM "system"";

    SqlParser.Config myConfig = SqlParser.config()
            .withQuoting(Quoting.DOUBLE_QUOTE)
            .withQuotedCasing(Casing.UNCHANGED)
            .withParserFactory(JackySqlParserImpl.FACTORY);
    
    SqlParser parser = SqlParser.create(sql, myConfig);
    SqlNode sqlNode = parser.parseQuery();

    assertTrue(sqlNode instanceof CreateMaterializedView);
    System.out.println(sqlNode);
}
```

#### 执行结果
```
CREATE MATERIALIZED VIEW IF NOT EXISTS `TEST`.`DEMO`.`MATERIALIZATIONNAME` AS SELECT *
FROM `SYSTEM`
```


### JACKY JOB 'query'
在 Calcite 中新增语法的示例。 新增语法为 `JACKY JOB 'query'`。

#### 语法格式
语法格式
```markdown
JACKY JOB 'query'
```

例如:  `JACKY JOB  'select ids, name from test where id < 5'`


#### 新增一种解析类
在 org.apache.calcite.sql 包下新增 SqlNode 实现。

```
package org.apache.calcite.sql;

import org.apache.calcite.sql.parser.SqlParserPos;
import org.apache.calcite.sql.util.SqlVisitor;
import org.apache.calcite.sql.validate.SqlValidator;
import org.apache.calcite.sql.validate.SqlValidatorScope;
import org.apache.calcite.util.Litmus;

public class SqlJacky extends SqlNode {
    private String jackyString;
    private SqlParserPos pos;

    public  SqlJacky(SqlParserPos pos, String jackyString){
        super(pos);
        this.pos = pos;
        this.jackyString = jackyString;
    }

    public String getJackyString(){
        System.out.println("getJackyString");
        return this.jackyString;
    }

    @Override
    public SqlNode clone(SqlParserPos sqlParserPos) {
        System.out.println("clone");
        return null;
    }

    @Override
    public void unparse(SqlWriter sqlWriter, int i, int i1) {
        sqlWriter.keyword("jacky");
        sqlWriter.keyword("job");
        sqlWriter.print("\n");
        sqlWriter.keyword("" + jackyString + "");
    }

    @Override
    public void validate(SqlValidator sqlValidator, SqlValidatorScope sqlValidatorScope) {
        System.out.println("validate");
    }

    @Override
    public <R> R accept(SqlVisitor<R> sqlVisitor) {
        System.out.println("accept");
        return null;
    }

    @Override
    public boolean equalsDeep(SqlNode sqlNode, Litmus litmus) {
        System.out.println("equalsDeep");
        return false;
    }
}
```


#### 编写语法文件 parserImpls.ftl
在 codegen/includes 目录下修改 parserImpls.ftl 文件。

新增如下代码：

```java
SqlNode SqlJacky() :
{
    SqlNode stringNode;
}
{
    <JACKY> <JOB>
    stringNode = StringLiteral()
    {
        return new SqlJacky(getPos(), token.image);
    }
}
```

#### 配置 config.fmpp 文件

引入上面的解析类
```
# List of additional classes and packages to import.
# Example: "org.apache.calcite.sql.*", "java.util.List".
imports: [
    "org.apache.calcite.sql.CreateMaterializedView",
    "org.apache.calcite.sql.SqlJacky"
]
```

定义新的关键字
```
# List of new keywords. Example: "DATABASES", "TABLES". If the keyword is
# not a reserved keyword, add it to the 'nonReservedKeywords' section.
keywords: [
    "IF",
    "MATERIALIZED",
    "JACKY",
    "JOB"
]
```

引入新的 Stat
```
# List of methods for parsing custom SQL statements.
# Return type of method implementation should be 'SqlNode'.
# Example: "SqlShowDatabases()", "SqlShowTables()".
statementParserMethods: [
    "SqlCreateMaterializedView()",
    "SqlJacky()"
]
```


#### 运行编译

```
mvn clean generate-resources
```

#### 测试 Case
```
package com.news.grammar;

import org.apache.calcite.avatica.util.Casing;
import org.apache.calcite.avatica.util.Quoting;
import org.apache.calcite.sql.SqlJacky;
import org.apache.calcite.sql.SqlNode;
import org.apache.calcite.sql.parser.SqlParseException;
import org.apache.calcite.sql.parser.SqlParser;
import org.apache.calcite.sql.parser.impl.JackySqlParserImpl;
import org.apache.calcite.sql.validate.SqlConformanceEnum;
import org.apache.calcite.tools.FrameworkConfig;
import org.apache.calcite.tools.Frameworks;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertTrue;

public class JackyJobTest {

    private FrameworkConfig frameworkConfig;

    @Before
    public void setUp() throws Exception {
        final FrameworkConfig config = Frameworks.newConfigBuilder()
                .parserConfig(SqlParser.config()
                        .withParserFactory(JackySqlParserImpl.FACTORY)
                        .withCaseSensitive(false)
                        .withQuoting(Quoting.BACK_TICK)
                        .withQuotedCasing(Casing.TO_UPPER)
                        .withUnquotedCasing(Casing.TO_UPPER)
                        .withConformance(SqlConformanceEnum.ORACLE_12))
                .build();

        this.frameworkConfig = config;
    }

    @Test
    public void testCustomGramJackyJob() throws SqlParseException {
        String sql = "JACKY JOB  'select ids, name from test where id < 5'";

        SqlParser parser = SqlParser.create(sql, this.frameworkConfig.getParserConfig());

        SqlNode sqlNode = parser.parseQuery();

        System.out.println(sqlNode);
        assertTrue(sqlNode instanceof SqlJacky);
    }

}


```

#### 执行结果
```
JACKY JOB 
'SELECT IDS, NAME FROM TEST WHERE ID < 5'
```

## calcite-schema
多种数据源加载的示例

关于自定义语法的实现，参考《[new-grammar](https://gitee.com/whosly/calcite-jj#new-grammar)》章节。

关于多种数据源加载的实现，见源代码 [schema](https://gitee.com/whosly/calcite-jj/tree/master/calcite-schema/src/main/java/com/whosly/calcite/schema/ISchemaLoader.java)

