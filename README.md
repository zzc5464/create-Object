# create Object（多种方式创建对象）

## 内置对象形式 new Object
	//利用内置对象形式创建
	var boy = new Object();
	//属性
	boy.name = 'JUGG';
	//属性
	boy.image = '最新至宝';
	//属性
	boy.HealthPoint_HP = 500;
	//属性
	boy.MagicPoint_MP = 200;
	//技能
	boy.technologys =['F':'剑刃风暴'];
	//方法
	boy.attack = function(){
	}
	
	var girl=new Object();
	girl.name='露娜'
  - 获取对象值的方法 
> boy.name;//常用方法
>> boy['name'];//动态编程中常用的方法（重要）。
	
## 工厂模式
> 就像生活中的工厂，我们不用关心他是怎么做出来的，负责用好他的产品就行。

    function createPerson(){
            //1.原料
            var obj = new Object();
            //2.加工
            obj.name = '露娜';
            obj.HP=100;
            obj.MP=100;
            obj.technologys=['普通攻击','c','光环','e'];
            obj.attack = function(){
                alert(obj.name+'发出攻击，导致对方10点伤害值')
            };
             obj.run = function(){
             };
             //3.出场
             return obj;
    
        }
    
        var boy = createPerson();
        boy.attack();

- 在函数内部已经实例过了，所以直接拿来使用。
- 在生活中工厂产出就是产品，这里的工厂产出就是代码。
- 使用场景，一些函数确实是默认值的情况下，就把这类做成原料，减少使用者考虑问题的时间。
- 与构造函数的区别，不用实例化和return。
- 设计模式：工厂模式（后端的顶级模式）
> 实际工作中：在函数中直接实例化，return带有默认属性的变量。
使用：拿到后端接口，并把接口传进函数中使用。

        function Product(){
            //实例 
            var obj = new Product();
            //默认值
            obj,name = '';
            //return
            return obj;
        }
        //使用
        var p = Product( data );
        //使用后端给的数据data
        p.name = data.name;
### 缺点:
> 太占用内存了，每次使用都要实例化一次。
>> 所以引出下面一个方法，原型创建对象。
## 原型创建对象
    var Role2 = function(){
      Role2.prototype.name = 'dotaer';
      Role2.prototype.technologys = ['',''];
  
    }
> 很好的解决了重复使用的问题，但是又会冒出新的问题。
      
      var Role2 = function(){
      Role2.prototype.name = 'dotaer';
      Role2.prototype.technologys = ['zzc','czz'];
  
      }
    var r1 = new Role2();
    r1.name = 'dota1';
    console.log(r1.name);//dota1
    var r2 = new Role2();
    r2.name = 'dota2';
    console.log(r1.name);//dota1
    console.log(r2.name);//dota2
    
- 虽然原型中的引用型都会被实例共享，但是里面的值类型却各干各的。

      var Role2 = function(){
      Role2.prototype.name = 'dotaer';
      Role2.prototype.technologys = ['普通攻击','隐身','飞镖','标记'];
      }
      var r1 = new Role2();
      r1.technologys.push('点灯');//添加一个数组
      var r2 = new Role2();
      r2.name = 'dota2';
      console.log("r1   "+r1.technologys.length);//5
      console.log("r2   "+r2.technologys.length);//5

> 这里有个问题没有解决，为什么用英文字符串会是如下效果。想不通...

      var Role2 = function(){
      Role2.prototype.name = 'dotaer';
      Role2.prototype.technologys = ['zzc','czz','zcz'];
      }
      var r1 = new Role2();
      r1.technologys.push('zzczz');
      var r2 = new Role2();
      r2.name = 'dota2';
      console.log("r1   "+r1.technologys[2]);//zcz
      console.log("r1   "+r1.technologys[3]);//undefine
      console.log("r2   "+r1.technologys[2]);//zcz
      console.log("r2   "+r2.technologys.length);//3
> 讲道理，并没有给原型的数组push。为什么中文字符串的r2就有3的长度，而英文却push不了呢。
- 问题还是很明显的
## 双对象法则-混合模式
    function Fn( option ) {
        this._init( option );
    }
    Fn.prototype = {
        _init:function( option ){
            
        },
        action:function(){
            
        }
    }
    
> 或者

    var Role =function() {
      this.name = {nickName:'张三',accountName:'774143745@qq.com'};
    }
    /*姓名*/
    //生命值
    Role.prototype.HP=100;
    //魔法值
    Role.prototype.MP=100;
    //技能
    Role.prototype.technologys=['普通攻击','灼烧烈焰','地狱火','骨隐步'];
    //跑起来
    Role.prototype.run=function() {
        alert('run');
    }

> 就是属性和那些要共享的东西放在里面，方法放外面。
## 字面量对象

    var boy = {
        name:'张三'
        ,image:'头像'
        ,age:20
        ,sex:'男'
        ,HP:100
        ,MP:100
        ,technologys:['普通攻击','灼烧烈焰','地狱火','骨隐步']
    };

    //将对象转换成字符串
    console.log(JSON.stringify(boy));
    //将字符换转换成json对象
    var sboy='{"name":"剑侠客","sex":"男","HP":100}';
    var objBoy = JSON.parse(sboy);
    console.log(objBoy.name);
- 后端传来的数据是字符串，不能直接用json对象来取，需要进行转换；
## 拷贝模式

        /*函数的用处：就是将一个json对象 所有属性拷贝给另外一个对象*/
        /*source：原始对象*/
        /*target：目标对象*/
        function extend(target,source) {
            //遍历对象
            for(var i in source){
                target[i] = source[i];
            }
            return target;
        }       
        //游戏随机生成名字
           var boy = {
            name:'郭靖'
            ,image:'头像'
            ,age:20
            ,sex:'男'
        };      
        var girl = {
            name:'黄蓉'
            ,age:18
            ,image:'女性头像'
            ,sex:'女'
        };      
        /*六大神器之一*/      
        var zuixiake = extend({}, boy);
        var huangrong = extend({},girl)
        alert(zuixiake.name);
        alert(zuixiake.sex);
        console.log(huangrong.name)

> 这个函数在框架中的应用非常广，号称六大神器之一。
## 拓展

    //extend2实现的功能：extend(target,obj1,obj2,obj3)
    /*功能：将多个多个json拷贝给目标*/
    /*原理：*/
    /*首先找到target   --arguments[0]*/
    function extend () {
        var key,i = 0,len = arguments.length,target = null,copy;
        if(len === 0){
            return;
        }else if(len === 1){
            target = this;
        }else{
            i++;
            target = arguments[0];
        }
        for(; i < len; i++){
            for(key in arguments[i]){
                copy = arguments[i][key];
                target[key] = copy;
            }
        }
        return target;
    }

    /*hasOwnProperty*/
    function extend2(){
        for (var p in source) {
            if (source.hasOwnProperty(p)) {
                target[p] = source[p];
            }
        }

        return target;
    }


    //游戏随机生成名字
    var boy = {
        name:'无忌'
        ,image:'男性头像'
        ,age:20
        ,sex:'男'
    };


    //技能名称，等级，伤害值，需要的魔法
    var technology = {tname:'亡灵复活',tlevel:10,tstrength:3000,tmagic:30};

    var shenqi = {sname:'霜之哀伤',slevel:30,sstrength:3000}
    //当这个人有了穿上盔甲，圣衣，六神合体，戴上魔法戒指之后，自动也拥有一个技能


    //第一种用法
    var zuixiake = extend({}, technology,shenqi);
    zuixiake.name='醉侠客';
    alert(zuixiake.name);
    alert(zuixiake.tname);
    alert(zuixiake.sname);


    //第二种用法
    extend(boy,technology,shenqi);
    alert(boy.name);
    alert(boy.tname);
    alert(boy.sname);
    
> 供研究
