# Programmer--RegularExpressions

##程序员的自我修养--正则表达式

###正则表达式

* 测试字符串的某个模式。例如，可以对一个输入字符串进行测试，看在该字符串是否存在一个电话号码模式或一个信用卡号码模式。这称为数据有效性验证 
* 替换文本。可以在文档中使用一个正则表达式来标识特定文字，然后可以全部将其删除，或者替换为别的文字 
* 根据模式匹配从字符串中提取一个子字符串。可以用来在文本或输入字段中查找特定文字 

###正则表达式语法
一个正则表达式就是由普通字符（例如字符a到z）以及特殊字符（称为元字符）组成的文字模式。该模式描述在查找文字主体时待匹配的一个或多个字符串。正则表达式作为一个模板，将某个字符模式与所搜索的字符进行匹配。

创建正则表达式

    var re = new RegExp();//RegExp是一个对象，和Array一样。但这样没有任何效果，需要将正则表达式的内容作为字符串传递进去
    re = new RegExp("a");//最简单的正则表达式，将匹配字母a
    re = new RegExp("a","i");//第二个参数，表示匹配时不分大小写
    
RegExp构造函数第一个参数为正则表达式的文本内容，而第一个参数则为可选项标志，标志可以组合使用

* g（全文查找）
* i（忽略大小写）
* m（多行查找）

例如：

    var re = new RegExp("a","gi");//匹配所有的a或A

正则表达式还有另一种正则表达式字面量的声明方式

    var re = /a/gi;

###和正则表达式相关的方法和属性
####正则表达式对象的方法

* test，返回一个Boolean值，它指出在被查找的字符串中是否存在模式。如果存在则返回true，否则就返回false。
* exec，用正则表达式模式在字符串中运行查找，并返回包含查找结果的一个数组。
* compile，把正则表达式编译为内部格式，从而执行得更快。

####正则表达式对象的属性

* source，返回正则表达式模式的文本的复本。只读。
* lastIndex，返回字符位置，它是被查找字符串中下一次成功匹配的开始位置。
* 1...9，返回九个在模式匹配期间找到的，最近保存的部分。只读。
* `input($_)`,返回执行规范表述查找的字符串。只读。
* `lastMatch($&)`，返回任何正则表达式搜索过程中的最后匹配的字符。只读。
* `lastParen($+)`，如果有的话，返回任何正则表达式查找过程中最后括的子匹配。只读。
* `leftContext($`)`，返回被查找的字符串中从字符串开始位置到最后匹配之前的位置之间的字符。只读。
* `rightContext($`)`，返回被搜索的字符串中从最后一个匹配位置开始到字符串结尾之间的字符。只读。

####String对象一些和正则表达式相关的方法

* match，找到一个或多个正则表达式的匹配。
* replace，替换与正则表达式匹配的子串。
* search，检索与正则表达式相匹配的值。
* split，把字符串分割为字符串数组。

###正则表达式是如何工作的

    //test方法，测试字符串，符合模式时返回true，否则返回false
    var re =/he/;//最简单的正则表达式，将匹配he这个单词
    var str = "he";
    alert(re.test(str));//true
    str = "we";
    alert(re.test(str));//false
    str = "HE";
    alert(re.test(str));//false,大写，如果要大小写都匹配，可以指定i标志（i是ignoreCase 或 case-insensitive的表示）
    re = /he/i;
    alert(re.test(str));//true
    str = "Certainly!He loves her!";
    alert(re.test(str));//true，只要包含he（HE）就符合，如果奥只是he或HE，不能有其他符合，则可使用^和$
    re = /^he/i;//脱字符(^)代表字符开始位置
    alert(re.test(str));//false，因为he不在str最开始
    str = "He is a good boy!";
    alert(re.test(str));//ture,He是字符开始位置，还需使用$
    re = /^he$/i;//$表示字符结束位置
    alert(re.test(str));//false
    str = "He";
    alert(re.test(str));//true
    //当然，这样不能发现正则表达式有多强大，因为我们完全可以在上面例子中使用==或indexOf
    re = /\s/;// \s匹配任何空白字符，包括空格、制表符、换页符等等
    str = "user Name";//用户名包含空格
    alert(re.test(str));//true
    str = "user    Name";//用户名包含制表符
    alert(re.test(str));//true
    re = /^[a-z]/i;//[]匹配指定范围内的任意字符，这里将匹配英文字母，不区分大小写
    str = "varibaleName";//变量名必须以字母开头
    alert(re.test(str));//true
    str = "124abc";
    alert(re.test(str));//false

当然，仅仅知道了字符串是否匹配模式还不够，我们还需知道哪些字符匹配了模式

    var osVersion = "Ubuntu 8";//其中的8表示系统主版本号
    var re = /^[a-z]+\s+\d+$/i;//+号表示字符至少要出现一次，\s表示空白字符,\d表示一个数字
    alert(re.test(osVersion));//true，但我们想知道主版本号
    //另一个方法exec，返回一个数组，数组的第一个元素为完整的匹配内容
    re = /^[a-z]+\s+\d+$/i;
    arr = re.exec(osVersion);
    alert(arr[0]);//将osVersion完整输出，因为整个字符串刚好匹配re
    //我们只需要输出数字
    re = /\d+/;
    var arr = re.exec(osVersion);
    alert(arr[0]);//8
        
更复杂的用法，使用子匹配

    //exec返回的数组第1到n元素中包含的是匹配中出现的任意一个子匹配
    re = /^[a-z]+\s+(\d+)$/i;//用()来创建子匹配
    arr = re.exec(osVersion);
    alert(arr[0]);//整个osVersion，也就是正则表达式的完整匹配
    alert(arr[1]);//8，第一个子匹配，事实也可以这样取出主版本号
    alert(arr.length);//2
    osVersion = "Ubuntu 8.10";//取出主版本号和此版本号
    re = /^[a-z]+\s+(\d+)\.(\d+)$/i;//.是正则表达式元字符之一，若要用它的字面意义须转义
    arr = re.exec(osVersion);
    alert(arr[0]);//完整的osVersion
    alert(arr[1]);//8
    alert(arr[2]);//10

>注意：当字符串不匹配re时，exec方法将返回null

类似于exec方法，String对象的match方法也用于将字符串与正则表达式进行匹配并返回结果数组

    var str = "My name is Dream.Hello everyone!";
    var re = /[A-Z]/;//匹配所有大写字母
    var arr = str.match(re);//返回数组
    alert(arr);//数组中只会包含一个M，因为我们没有使用全局匹配
    re = /[A-z]/g;
    arr = str.match(re);
    alert(arr);//M,D,H
    re = /\b[a-z]*\b/gi;// \b表示单词边界
    str = "one two three four";
    alert(str.match(re));//one,two,three,four
    
###RegExp对象实例的一些属性

    var re = /[a-z]/i;
    alert(re.source);//将[a-z]字符串输出
    //请注意，直接alert(re)会将正则表达式连同前面斜线与标志输出，这是re.toString方法定义的

每个RegExp对象的实例具有lastIndex属性，它是被查找字符串中下一次成功匹配的开始位置，默认值是-1.lastIndex属性被RegExp对象的exec和test方法修改，并且它是可写的。

    var re = /[A-Z]/;
    //exec方法执行后，修改了re的lastIndex属性
    var str = "Hello,World!!!";
    var arr = re.exec(str);
    alert(re.lastIndex);//0,因为没有设置全局标志
    re = /[A-Z]/g;
    arr = re.exec(str);
    alert(re.lastIndex);//1
    arr = re.exec(str);
    alert(re.lastIndex);//7

当匹配失败（后面没有匹配），或lastIndex值大于字符串长度时，再执行exec等方法会将lastIndex设为0（开始位置）

    var re = /[A-Z]/;
    var str = "Hello,World!!!";
    re.lastIndex = 120;
    var arr = re.exec(str);
    alert(re.lastIndex);//0
    
RegExp对象的静态属性

    //input 最后用于匹配的字符串（传递给test，exec方法的字符串）
    var re = /[A-Z]/;
    var str = "Hello,World!!!";
    var arr = re.exec(str);
    alert(RegExp.input);//Hello,World!!!
    re.exec("tempstr");
    alert(RegExp.input);//仍然是Hello,World!!!，因为tempstr不匹配
    //lastMatch最后匹配的字符
    re = /[a-z]/g;
    str = "hi";
    re.test(str);
    alert(RegExp.lastMatch);//h  
    re.test(str);
    alert(RegExp["$&"]);//i, $&是lastMatch的短名字，但由于它不是合法变量名，所以要。。
    //lastParent 最后匹配的分组
    re = /[a-z](\d+)/gi;
    str = "Class1 Class2 Class3";
    re.test(str);
    alert(RegExp.lastParent);//1
    re.test(str);
    alert(RegExp["$+"]);//2
    //leftContext 返回被查找的字符串中从字符串开始位置到最后匹配之间的位置之间的字符
    //rightContext 返回被搜索的字符串中从最后一个匹配位置开始到字符串结尾之间的字符
    re = /[A-Z]/g;
    str = "123ABC456";
    re.test(str);
    alert(RegExp.leftContext);//124
    alert(RegExp.rightContext);//BC456
    re.test(str);
    alert(RegExp["$`"]);//123A
    alert(RegExp["$'"]);//C456
