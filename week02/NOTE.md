编程语言通识与JavaScript语言设计

一、编程语言知识（文法分为语法和词法两部分）
	
	1、语言按语法分类：
		（1）非形式语言：没有严苛的定义，演化方式非常自由，例如中文（你吃饭了吗，吃了吗）、英文；
		
		（2）形式语言（乔姆斯基谱系，比较严谨的语言）；
			0-型文法：无限制文法（并不是完全无限制）或短语结构文法，包括所有的文法；
			1-型文法：上下文相关文法，生成上下文相关语言；
			2-型文法：上下文无关文法（计算机语言是主体上的上下文无关文法），生成上下文无关语言；
			3-型文法：正规文法，生成正则语言（对表达能力有很强的限制，会限制表达能力）；
			
		（3）乔姆斯基谱系：是计算机科学中刻画形式文法表达能力的一个分类谱系，是由诺姆·乔姆斯基于1956年提出的；
		
	2、语言按词法分类：
		（1）词法是用正则做一遍粗略的处理；
		（2）把语言变成单个是词；
		（3）再把这些词作为一种输入流，去做语法分析；
		
	3、产生式（BNF）：
		（1）BNF：
			即巴科斯范式，是一种用于表示上下文无关文法的语言；
			上下文无关文法描述了一类形式语言；
			它是由约翰·巴科斯和彼得·诺尔首先引入的用来描述计算机语言语法的符号集；
			
		（2）用尖括号括起来的名称来表示语法结构名；
		
		（3）语法结构分成基础结构和需要用其他语法结构定义的复核结构；
			基础结构称终结符（比如一个*）；
			复合结构称为非终结符（比如一个乘法表达式1*2=2）；
			
		（4）引号和中间的字符表示终结符；
		（5）可以有括号；
		（6）*表示重复多次；
		（7）|表示或；
		（8）+表示至少一次；
		
		（9）例1：<Program>="a"+ |"b"+       //program等于多个a或者多个b;
		(10)例2：
				<Number> = "0" | "2" | "3"……"9"     //定义数字0到9
				<DecimalNumber> = "0"|(("0"|"1"|……"9")<Number>+);   //排除01 
									  0
									  1                987110（<Number>+）
									
				<AdditionExpression> = <DecimalNumber> "+" <DecimalNumber>    //加法
				<AdditionExpression> = <AdditionExpression> "+" <DecimalNumber>    //连加
								||
								||
				<AdditionExpression> = <DecimalNumber> |  <AdditionExpression> "+" <DecimalNumber>   
				<AdditionExpression>=1  | <AdditionExpression>=1+2+3			//连加或不加
				
				//带括号的运算
				<PrimaryExpression> = <DecimalNumber> | "(" <LogicalExpression> ")"；
				
				//由加法推断乘法、除法
				<MultiplicationExpression> = <PrimaryExpression> |  
						<MultiplicationExpression> "*" <PrimaryExpression> | 
						<MultiplicationExpression> "/" <PrimaryExpression>
				
		(11)四则运算：1+2*3	
				//先乘除后加减 1+2*3  ==>  1+6 
				<AdditionExpression> = <MultiplicationExpression> |  
					<AdditionExpression> "+" <MultiplicationExpression> |
					<AdditionExpression> "-" <MultiplicationExpression>  
								||
								||   //逻辑判断
				<LogicalExpression> = <AdditiveExpression> |  
						<LogicalExpression> "||" <AdditiveExpression> |
						<LogicalExpression> "&&" <AdditiveExpression>
				
				
		（12）总结：
			终结符：最终在代码中出现的字符，比如Number、+、-、*、/;
			非终结符: <MultiplicationExpression>、<AdditionExpression>
		
		（13）产生式：在计算机中指Tiger编译器将源程序经过词法分析（Lexical Analysis）和语法分析（Syntax Analysis）后得到的一系列符合文法规则（Backus-Naur Form，BNF的语句
				
	4、通过产生式了解乔姆斯基谱系：
		（1）0型无限制文法：?::=?
			等号左右两边可以有多个非终结符，<a><b> :: ="c"；
		（2）1型上下文相关文法：?<A>?::=?<B>?	
			只用问号中间的内容可以改变，"a"<b>"c"::="a"x"c"，x在a和c的上下文中间被解释成了b；
		（3）2型上下文无关文法：<A>::=?
			等号左边只能有一个非终结符，JavaScript是以2型文法为主的；
		（4）3型正则文法：<A>::=<A>?
			只允许左递归，例如十进制数的正则表达式（<DecimalNumber>=/0|[1-9][0-9]*/）；				
					
	5、其他产生式：EBNF、ABNF；	
		
二、	现代语言的特例：
	1、JavaScript中：
		（1）/ 可能是除号，也可能是正则表达式开头，处理方式类似于VB；
		（2）字符串模板中也需要特殊处理；
		（3）还有自动插入分号规则；
	2、C++中：*，可能表示乘号或者指针，具体是那个，取决于*前面的标识符是否被声明为类型；
	3、VB中：<，可能是小于号，也可能是XML直接量的开始，取决于当前位置是否可以接受XML直接量；
	4、Python中，行首的tab符和空格会根据上一行的行首空白以一定规则被处理成虚拟终结符indent，或者dedent;
	
三、语言的分类：
	1、形式语言-用途：
		（1）数据描述语言：JSON、HTML、XAML、SQL、CSS;
		（2）编程语言：C、C++、JAVA、C#、Python、Ruby、Perl、JavaScript……；
	2、形式语言-表达方式：
		（1）声明式语言：JSON、HTML、XAML、SQL、CSS……;
		（2）命令式语言：C、C++、JAVA、C#、Python、Ruby、Perl、JavaScript；

四、图灵完备性：
	1、概念：
		（1）在可计算理论里，如果一系列操作数据的规则（如指令集、编程语言、细胞自动机）可以用来模拟单带图灵机，那么它是图灵完备的，即跟图灵机等效，都具有图灵完备性；
		（2）这个词源于引入图灵机概念的数学家艾伦·图灵；
		（3）虽然图灵机会受到储存能力的物理限制，图灵完全性通常指“具有无限存储能力的通用物理机器或编程语言”；
	2、图灵机：（Turing machine）：
		（1）又称确定型图灵机，是英国数学家艾伦·图灵于1936年提出的一种将人的计算行为抽象掉的数学逻辑机；
		（2）其更抽象的意义为一种计算模型，可以看作等价于任何有限逻辑数学过程的终极强大逻辑机器；
		（3）凡是可计算东西的都能计算出来，就叫图灵机；
	3、所有的编程语言都必须具有图灵完备性：
		（1）一台图灵机不能计算另一台图灵机是否停机，所以世界上的一切不都能够被计算机解决；
		（2）在可计算的范围内，就发展出了计算机学科和计算机相关的数学，所以所有的编程语言都必须具有图灵完备性；
		（3）今天我们用的计算机语言都是图灵机的等效形式；
	4、实现方式：
		（1）命令式-图灵机：goto、if（分支）和while（循环）；
		（2）声明式-lambda：递归；
		
五、动态与静态：https://www.cnblogs.com/raind/p/8551791.html（资料）
	1、动态：
		（1）在用户的设备上、在线服务器上；
		（2）产品实际运行时；
		（3）Runtime（运行时）；
	2、静态：
		（1）在程序员的设备上；
		（2）产品开发时；
		（3）CompiLetime（编译时）;
		
六、类型系统
	1、动态类型系统与静态类型系统；
	2、强类型（无隐式转换）与弱类型（有隐式转换）：String+Number、String==Boolean；
	3、复合类型：
		（1）结构体：对象就是结构体，每个属性都可以有一个类型；
		（2）函数签名：由函数的参数列表和返回值组成，t1，t2两个参数得到t3返回值，t1、t2、t3构成了一个函数签名；
	4、子类型：逆变（父类型数组传子类型数组的实例）、协变（与逆变相反）；
		https://jkchao.github.io/typescript-book-chinese/tips/covarianceAndContravariance.html（资料）
	
七、一般命令式编程语言（五层结构）；
	1、Atom（最小单位的东西）；
	2、Expression（表达式） ；
	3、Statement（if、else，语句）； 
	4、Structure(结构化) ；
	5、Program/modue（程序集）；
	
八、重学JavaScript：
	（1）语法、语义、运行时；
	（2）通过视角的变换会有更深的理解；
	


重学前端

一、any Unicode code point
	1、Unicode：
		（1）字符集，计算机领域的字符集都会对应一个正整数（这个正整数就是这个字符的码点）；
		（2）包含世界各国的所有字符；
		（3）资料参考：https://www.fileformat.info/info/unicode/
					https://home.unicode.org/
	2、Unicode的两个视角：
		（1）Blocks：
				Unicode标准将字符组一起排列在块中；
				
				Basic Latin（基本拉丁语）	从U+0000到U+007F	 128个 与ASCII码是对应的；
				<script>    //具体的基本拉丁语内容
					for(var i=0;i<128;i++){
						console.log(String.fromCharCode(i));
					}
				</script>
				具体说明参考资料：https://www.fileformat.info/info/unicode/block/basic_latin/list.htm
				例如：U+0007  响一声
				
				CJK Unified Ideographs（中文字符范围）	从U+4E00到U+9FFF	20989个   包含中日韩
				
				以Specials	从U+FFF0到U+FFFF	5个为分界点，列表以上的字符称为基本字符平面，兼容性好
				js中用String.fromCharCode获取
				列表一下的字符用String.fromCodePoint或者用String.codePointAt获取；				  
		（2）Categories：
				每个unicode字符都分配有一个类别；
				例如：小写字母、大写字母、十进制数、分隔符空格……
				
				分隔符、空格：
				资料参考：https://www.fileformat.info/info/unicode/category/Zs/list.htm



脑图补充：http://naotu.baidu.com/file/4a0a2fb8704bd4fbc29bea34f2a54b5a?token=e0fdd8a2eb003354


作业；
写一个正则表达式 匹配所有Number直接量
/^[+-]?(0|([1-9]\d*))(\.\d+)?$/

写一个正则表达式，匹配所有的字符串直接量，单引号和双引号
[\u0000-\007F]

写一个UTF-8 Encoding的函数

									
