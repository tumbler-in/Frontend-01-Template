重学JavaScript表达式、类型转换

    一、正0负0的判断：
        function check(zero){
            if(1/zero == Infinity){     //正无穷
                return 1;
            }
            if(1/zero == -Infinity){    //负无穷
                return -1;
            }
        }
        console.log(check(0));
        console.log(check(-0));

    二、关于浮点数的运算，不同运算的精度丢失程度是不一样的，算数运算是要考虑精度损失的影响；

    三、JavaScript Expression：
        1、代码里面主体的事，都是用表达式来做的；
        2、Gramar（语法）部分：
            （1）Tree vs Priority（运算符的优先级，用表达式生成树的方式计算）
                +-                  +             //1+2*3
                */                  |
                                  1   *          //加法的右边是一个新的节点
                                      |
                                    2   3
                ()

            （2）Left Handside（等号左边）：
                Member (成员访问)                     New               Call
                a.b                                   new foo           foo()
                a[b]    //b可以是变量                                   super()
                foo`string`    //foo可以是函数                          foo()['b']
                function foo(){                                         foo().b
                    console.log(arguments);                             foo()`abc`
                }
                foo`hello${name}`;
                super.b    //构造函数
                super[b]
                new.target
                new Foo()

            （3）运算符两个new的区别：优先级不一样，new Foo()的优先级更高
                function clas1() {}
                function clas2() {
                    return clas1;
                }
                new clas2;     //f clas1{}    函数
                clas2();       //f clas1{}     函数
                new (clas2());     //clas1{}    对象

            （4）Member Expression返回的是一个Reference类型；
                由两部分组成：Object和Key;
                例如：
                    var x={o:1};
                    console.log(x.o+2);    //3
                    console.log(1+2);     //3
                    delete  x.o;
                    delete 1;
                    console.log(x.o+2);    //NaN      表达式返回的是Reference类型
                    console.log(1+2);     //3

            （5）Right Handside（等号右边）：

            （6）Update Expression:
                a++
                ++a
                a--
                --a

            （7）Unary Expression（单目运算符）:
                delete a.b
                void foo()      //将后面的任何东西变成undefined
                    for(var i=0;i<10;i++){     //void 的使用
                        var button=document.createElement("button");
                        button.innerHTML=i;
                        document.body.append(button);
                        (function(i){
                            button.onclick=function(){
                            console.log(i);
                        }})(i);
                        void function(i){
                            button.onclick=function(){
                                console.log(i);
                            }
                        }(i)
                        }
                typeof a
                +a
                -a
                ~a    //按位取反
                !a    //非    类型转换   !!1  ==>true
                await a

            （8）Exponental Expression:
                 **（唯一一个右结合的运算符）    3**2**3
                                                3**（2**3）
            （9）Multiplicative：* / %
            （10）Additive：+ -
            （11）Shift（左右移位）：<<  >>  >>>
            （12）Relationship（比较）：<> <= >= instanceof in
            （13）Equality（相等比较）：==  != ===  !==
            （14）Bitwise（位运算）：& ^ |
            （15）Logical：&&  ||   短路逻辑
            （16）Conditional（三目运算）：?:
            （17）逗号：,


        3、Runtime(运行时)部分：

        4、Type Convertion（类型转换）：
                    Number   String   Boolean   Undefined   Null   Object   Symbol
            Number    -               0 false      x         x     Boxing      x
            String             -      "" false     x         x     Boxing      x
            Boolean  true 1   'true'      -        x         x     Boxing      x
                     false 0  'false'
            Undefined   0    'Undefined'  false    -         x        x        x
            Null        0    'Null'       false    x         -        x        x
            Object   valueOf   valueOf    true     x         x        -        x
                               toString
            Symbol     x          x        x       x         x     Boxing      -

        5、Boxing & Unboxing：
            （1）Number、String、Boolean、Symbol这四种基本类型都有一个对应的class（类），这个称作装箱运算，
                其他类型没有装箱运算；
                   var a=new Number(1);
                   console.log(a);
                   //Number {1}       1被包成了对象

                   var b=new String("Hello");
                   console.log(b);
                   //String {"Hello"}   Hello被包成了对象

                   console.log(Number(1),String("Hello"),Boolean(true));
                   //1 hello true 前面不带new时，就不是对象 有强制类型转换的作用

                   console.log(Object(1),Object("Hello"));
                   //Number {1} String {"Hello"}  Object强制装箱

                   (function(){return this}).apply(Symbol("1111"));
                   //Symbol {Symbol(1111)}   apply装箱

            （2）拆箱：toString vs valueOf：
                var a=1+{};
                console.log(a,typeof a);   //1[object Object] string
                var b=1+{valueOf(){return 1}};
                console.log(b);      //2
                var c=1+{toString(){return 1}};
                console.log(c);      //2
                var d=1+{toString(){return "4"}};
                console.log(d);      //14
                var e=1+{toString(){return "4"},valueOf(){return 2}};
                console.log(e);      //3   valueOf的值优先
                var f="1"+{toString(){return "4"},valueOf(){return 2}};
                console.log(f);      //12   valueOf的值优先
                var g="1"+{[Symbol.toPrimitive](){return 5},toString(){return "4"},valueOf(){return 2}};
                console.log(g);      //15   toPrimitive的值优先
                var h="1"+{[Symbol.toPrimitive](){return {}},toString(){return "4"},valueOf(){return 2}};
                console.log(h);      //toPrimitive 返回对象  报错
                var i="1"+{toString(){return "4"},valueOf(){return {}}};
                console.log(i);      //14  valueOf返回对象  调用toString

    四、Exercise：
        1、StringToNumber：
        2、NumberToString：
		
		
		
重学JavaScript语句、对象

	一、JavaScript Statement：
		1、Atom-Grammar：
			（1）简单语句：就一句，语句最基本的组成单位；
				• ExpressionStatement（表达式语句）：
					一个表达式可以构成一个语句，语句里最主体的部分，表示计算的语句，用来告诉计算机做一个运算；
					例如：a=1+2;
				• EmptyStatement（空语句）：
					例如：;
				• DebuggerStatement（调试语句）：会产生一个调试的中断，不产生任何作用；
					例如：debugger;
				• ThrowStatement：
					例如：throw a;
				• ContinueStatement：有循环搭配使用；
					例如：continue;
						 continue label；  
				• BreakStatement：有循环搭配使用；
					例如：break;
						 break label;
				• ReturnStatement：
					例如：return 1+2;
			（2）组合语句：语句里嵌套别的语句；
				• BlockStatement：
					{
						……
						……
						……
					}
					执行结果：遇到非normal的结果的语句，会中断执行；
					• [[type]]: normal
					• [[value]]: --
					• [[target]]: --
					
				• IfStatement：
				• SwitchStatement：
				• IterationStatement：循环语句；
					• while( ▒▒ ) ░░░░                
					• do ░░░░ while( ▒▒ )
					• for( ▓▓ ; ▒▒ ; ▒▒) ░░░░
						for(let i=0;i<10;i++){
							let i=1;
							console.log(i);
						}
						||
						||
						||
						{               //for(let i=0;i<10;i++)
							let i=0;
							{
								let i=1;   
								console.log(i);
							}
							console.log(i);
						}
					• for( ▓▓ in ▒▒ ) ░░░░       //遍历对象
					• for( ▓▓ of ▒▒ ) ░░░░       //循环任何具有迭代特点的对象
						function *a(){
							yield 1;
							yield 1;
							yield 1;
						}
						for(let p of a()){
							console.log(p)
						}
				• WithStatement：
				• LabelledStatement：
				• TryStatement：
					try {
						░░░░░░░░
						throw    //所有可能产生运行时错误的地方都可能会产生throw；
					} catch( ▓▓▓ 【变量】) {    
						░░░░░░░░
					} finally {
						░░░░░░░░
					}
					执行结果：
						• [[type]]: return
						• [[value]]: --
						• [[target]]: label
					try{
						throw 2;
					}catch(e){   //与for不同，catch这里不产生作用域，所以let e报错
						let e;    
						console.log(a);
					}
			（3）声明：会产生一些特殊的行为；
				• FunctionDeclaration：
					function foo(){}
					var foo=function(){}
				• GeneratorDeclaration:一种特殊的函数，可以返回多个值得函数；
					function* foo(){
						yield 1;
						yield 2;
						 
						var i=1;
						while(true)        //可以有死循环
							yield i++
					}
					var gen = foo();     //调用
					gen.next();        //调用一次执行一次
					gen.next();
					gen.next();
					
					适用场景；一个函数需要返回多个值；
					
				• AsyncFunctionDeclaration：异步函数
					function sleep(d){
						return new Promise(resolve => setTimeout(resolve,d));
					}
					void async function(){
						var i=0;
						while(true){
							console.log(i++);
							await sleep(1000);   
						}
					}();      //立即执行的函数表达式
										
				• AsyncGeneratorDeclaration：
					function sleep(d){
						return new Promise(resolve => setTimeout(resolve,d));
					}
					async function* foo(){
						var i=0;
						while(true){
							yield i++;
							await sleep(1000);  
						}
					};
					void async function(){
						var g=foo();
						console.log(await g.next());
						console.log(await g.next());
						console.log(await g.next());
						console.log(await g.next());
						console.log(await g.next());
						
						for await(let e of g){
							console.log(e);
						}
					}();    
					
				• VariableStatement：变量声明；
					如果有var，尽量不要写在语句的子结构里面，尽量写在函数的范围内；
					不要在任何的block里面写var；
					var声明不管写在函数的那个变量之前，最终起到的作用都会针对整个function；
					
				• ClassDeclaration：
					class a{}
					var o=class{}
					
				• LexicalDeclaration：
			
		2、Atom-Runtime：
			（1）Completion Record（完成的记录）：语句执行完成的结果；
				• [[type]]：normal，break，continue，return，throw；
				• [[value]]：Types；
				• [[target]]：label；
			（2）Lexical Environment：			

	五、补充-作用域：一个声明，有效的文本范围；
	
	六、对象：世间万物的总称；
		1、对象并不是数据存储的工具，从结构体的概念上说，才是数据存储的工具；
		2、对象的三要素，唯一性、状态、行为；
			（1）三只一模一样的鱼，其实是三个对象；
			（2）其中一只鱼发生了状态改变，失去了尾巴；
			（3）其它两只鱼并不受到影响；
			（4）因此，当我们在计算机中描述这三只鱼时，必须把相同的数据存储三份；
			（5）所以任何一个对象都是唯一的，这与它本身的状态无关；
			（6）所以，即使状态完全一致的两个对象，也并不相等；
			（7）我们用状态来描述对象；
			（8）我们状态的改变即是行为；
		3、对象有一个生命周期，在这个声明周期里，只要这个对象存在，就是可被唯一标记的，所以JavaScript中，
		   两个对象不相等；
		4、对象是有状态的，不是固定不变的；
		5、Object—Class（基于类的面相对象）：
			（1）类是一种常见的描述对象的方式；
			（2）而”归类”和”分类”则是两个主要的流派；
			（3）对于”归类”方法而言，多继承是非常自然的事情，如C++；
		6、Object—Prototype（基于原型的面相对象）：
			（1）原型是一种更接近人类原始认知的描述对象的方法；
			（2）我们并不试图做严谨的分类，而是采用“相似”这样的方式去描述对象；
			（3）任何对象仅仅需要描述它自己与原型的区别即可；
						Nihilo
						  |
					Object Prototype
						  |					
					Animal Prototype
					|			 |
			Fish Prototype	 Sheep Prototype
		7、狗 咬 人，“咬”这个行为该如何使用对象抽象？
			(1)我们不应该受到语言描述的干扰；
			(2)在设计对象的状态和行为时，我们总是遵循“行为改变状态”的原则。
			class Dog {    //错
				bite(human) {
					//……
				}
			}
			class Human {    //对
				hurt(damage) {
					//……
				}
			}
			
		8、Object in JavaScript：
			（1）在JavaScript运行时，原生对象的描述方式非常简单，我们只需要关心原型和属性两个部分；
			（2）JavaScript用属性来统一抽象对象状态和行为；
			（3）一般来说，数据属性用于描述状态，访问器属性则用于描述行为；
			（4）数据属性中如果存储函数，也可以用于描述行为。



		
