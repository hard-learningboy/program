## 项目

### 1、myenums

#### 1）CustomizeErrorCode

```java
	private String message;
    private String code;

	OK("1000","请求成功"),
    NOT_OK("1500","请求失败"),
    USER_NO_LOGIN("2000","兄弟,你还没有登入呢~"),
    QUESTION_NOT_FOUND("2001","该帖子不存在或已被移除"),
    SYSTEM_Error("2002","服务器太累了~"),
    QUESTION_NOT_IS_YOURS("2003","走错房间了吧,兄弟~~"),
    PAGE_NOT_FOUNT("2004","兄弟你找的页面不存在,鸭~" ),
    PEOPLE_DOT_HAVE("2005","用户信息不存在" ),
    COMMENT_CANT_EMPTY("2006","评论内容不能为空"),
    COMMENT_NOT_FOUNT("2006","找不到你要回复的评论"),
    NOT_LIKE_YOURSELF("2007", "不能给自己点赞"),
    CANT_LIKE_YOURSELF_QUESTION("2008","不能给自己的帖子点赞" ),
    COMMENT_LIKE_TWICE("2009","你今天已经赞过了哟~" ),
    CANT_LIKE_QUESTION_TWICE("2010","不能重复点赞帖子" ),
    USER_NOT_FOUND("2011","该用户不存在" ),
    NOT_FOND_CATEGORY("20012","找不到此类型"),
    NOT_ADD_OTHER_BQ("20013","暂时不支持其他平台的表情" ),
    COMMENT_CONTENT_TO_MANY("20014","评论的内容不能超过50个字" ),
    READ_ALL_FAIL("20015","全部标为已读失败哦"),
    YOU_COLLECTED_QUESTION("20016","你已经收藏该帖子" ),
    NOT_FOUND_TOPIC("20017","你查找的话题不存在"),
    FOLLOW_TOPIC_FAIL("20018","关注话题失败"),
    FOLLOW_NEED_LOGIN("20019","登入后关注"),
    TOPIC_IS_FOLLOWED("20020","你已经关注该话题"),
    DELETE_NOTIFICATION_ERROR("20021","删除通知失败"),
    COMMENT_INSERT_FAIL("20022", "点赞评论错误"),
    TITLE_IS_TOO_SIMPLE("20023","你的标题太过简单了~" ),
    SEND_CODE_ERROR("20024","发送失败" ),
    USER_NAME_NOT_EMPTY("20025", "用户名不能为空"),
    PASSWORD_TWICE_EQ("20026","两次的密码不一致"),
    CODE_NOT_SUCCESS("20027","验证码错误"),
    PHONE_NOT_EMPTY("20028","电话号码不能为空" ),
    THIS_PHONE_USED("20028","该电话号码已经被注册"),
    NAME_AND_PASSWORD_CANT_EMPTY("20029","用户名和密码不能为空" ),
    LOGIN_FAIL("20030","用户名或密码错误"),
    USERNAME_IS_USED("20031","用户名已经被占用" ),
    USER_ID_EMPTY("20032","用户ID不能为空"),
    ALREADY_SIGIN("20033","今天已经签到");
```

#### 2)QuestionErrorEnum

```java
	private String msg;
    private String code;	

	QUESTION_CANT_EMPTY("4000","帖子不能为空"),
    QUESTION_HEAD_CANT_EMPTY("4001","帖子标题不能为空"),
    QUESTION_DESC_CANT_EMPTY("4002","帖子描述不能为空"),
    QUESTION_TAGS_CANT_EMPTY("4003","帖子标签不能为空"),
    QUESTION_NEED_LOGIN("4004","发表帖子需要登入哟~~"),
    QUESTION_PUBLISH_SUCCESS("4005","帖子发表成功" ),
    QUESTION_UPDATE_SUCCESS("4006","帖子更新成功" ),
    QUESTION_DELETE_SUCCESS("4007","帖子删除成功"),
    Question_Category_CANT_EMPTY("4008","帖子分类不能为空");
```

##### 3)RepositoryErrorEnum

```java
	private String msg;
    private String code;

	REPOSITORY_CANT_EMPTY("4000","库不能为空"),
    REPOSITORY_HEAD_CANT_EMPTY("4001","库标题不能为空"),
    REPOSITORY_DESC_CANT_EMPTY("4002","库描述不能为空"),
    REPOSITORY_TAGS_CANT_EMPTY("4003","库标签不能为空"),
    REPOSITORY_NEED_LOGIN("4004","发表库需要登入哟~~"),
    REPOSITORY_PUBLISH_SUCCESS("4005","库发表成功" ),
    REPOSITORY_UPDATE_SUCCESS("4006","库更新成功" ),
    REPOSITORY_DELETE_SUCCESS("4007","库删除成功"),
    Repository_Category_CANT_EMPTY("4008","库分类不能为空");
```

#### 4）IntegralType

```java
	private long val;
    private String message;

	SIGN_IN(10,"签到"),
    COMMENT(5,"被评论"),
    LIKE(2,"被点赞" );
```

#### 5）AdType：广告，未实现

```java
NAV,SIDE,HEAD,FOOTER,ITEM
```





### 2、dto

#### 1）ResultTypeDTO

```java
	private String code;
    private String message;
    private Map<String,Object> extend=new HashMap<>();

	public ResultTypeDTO errorOf(CustomizeErrorCode | QuestionErrorEnum | RepositoryErrorEnum)
    
    public ResultTypeDTO okOf(){CustomizeErrorCode.OK.getMessage(),CustomizeErrorCode.OK.getCode()}
    
    public ResultTypeDTO addMsg(String key, Object value){extend.put(key,value);}
```

#### 2）NewUserDTO :

- 最新登录用户右侧列表内容
- 继承了User类，拓展了下面四个属性（在使用时通过BeanUtils.copyProperties(user, newUserDTO);  将User中的属性赋值给DTO，再完成其他值注入）

```java
private Integer questionCount;
//点赞人数
private Integer likeCount;
//粉丝数
private Integer fansCount;
private Integer time;
```

#### 3）QuestionDTO

```java
 private Integer id;
private String title;
private String description;
private long gmtCreate;
private long gmtModified;
private String tag;
private int viewCount;
private int likeCount;
private int commentCount;
private Integer creator;
private Integer category;

//跟question数据表相比，多了User，少了topic
private User user;
```

#### 4)QuestionQueryDTO

```java
private String tag;
private String search;
private long beginTime;
private long endTime;
private Integer category;
private Integer pageNo;
private Integer pageSize;

private String sort;
```

- Question.java 中 //跟question数据表相比，多了以下三个属性

```java
private User user;
private String showTime;
private String typeName;
```





### 3、Controller

#### 1）SigInController

```java
- "/sigIn":用户签到
    session获取user,判断user是否为空
    //1）空返回ResultTypeDTO（USER_NO_LOGIN）
    //2）非空执行userService.signIn(user.getId())，返回ResultTypeDTO
    	- "进入service" //**signIn**
    	private RedisTemplate redisTemplate;
		//redis中存在表示今天已签到
		redisTemplate.opsForSet().isMember(key, id.toString());
		//判断返回的布尔值
		//1）true 已存在 返回ResultTypeDTO（ALREADY_SIGIN）
		//2）执行下述操作，返回ResultTypeDTO().okOf()
			//加入redis中
			redisTemplate.opsForSet().add(key,id.toString());
			//expire://重新设置过期时间为getRefreshTime秒，刷新时间
			//**getRefreshTime**：获取当前时间距离明天还有多少小时
            this.redisTemplate.expire(key,getRefreshTime(), TimeUnit.SECONDS);
			//**addPointsRecord** 更新integral表中的积分，如果还未有，则新增一行插入
            addPointsRecord(id, IntegralType.SIGN_IN);//给用户增加积分

- "/sigIned":判断用户是否已签到
    从session中获取user，
    //如果没有，则为未登录，返回ResultTypeDTO().okOf().addMsg("sigined","1");
    //有user，为登录状态
    	- "进入service" //**isSigined**
   		redisTemplate.opsForSet().isMember(key, id.toString());
		//返回布尔值，为真则表示缓存已经有该条数据，表示用户已经签到过了，返回 ResultTypeDTO().okOf().addMsg("sigined","1");
		//假，即用户未签到过，返回 new ResultTypeDTO().okOf().addMsg("sigined","0");
```





### 4、cache

#### 1）HotTagCache！

- 降序排序方法具体看不明白！1

- IndexController 和 TagTask 两个类中有使用到

  - TagTask是个定时任务类，他会每隔30分钟就更新一次热门标签，但是**项目启动的时候就会执行，而bean是单例的，所以 hotTagCache中的字段属性properties就会被设置，已经有数据了！！！**（这样就不怕进入index.html页面时 Controller调用的hotTagCache.updateTags(); 无效了！）（定时任务中做到了==查询更换==）

  - 在进入index.html页面时引入了index.js，js默认调用build_right_list（）加载右边数据的方法，该方法中使用了$.ajax，其中传过去跟后端交互的就是’/loadRightList’方法，该方法是IndexController中的，hotTagCache.updateTags(); ，**这里不用给hotTagCache.properties赋值，因为上面的定时任务在项目启动时就执行了，properties已经有值了**。（Controller中只做==查询展示==）

    

```java
//properties 是一个Map ，存放（“标签”，“占比数值”）,通过其他类调用传入参数值给properties，然后再更新热门标签
private  Map<String,Integer> properties=new HashMap<>();

public void setProperties(Map<String, Integer> properties) {
    this.properties = properties;
}

public Map<String, Integer> getProperties() {
    return properties;
}


//updateTags()方法返回一个指定大小的 List<String> ， top10的标签
public List<String> updateTags(){
    List<String> objects = new ArrayList<>();
    Map<String, Integer> map = HotTagCache.sortByValueDescending(this.properties);
    for(Map.Entry<String,Integer> entry:map.entrySet()){
        String key = entry.getKey();
        if(key!=null&&key.length()!=1&&key.length()<15){
            objects.add(key);
        }
    }
    int size = objects.size();
    //mcx:原33
    
//只要前10个标签数
    if(size>=10){
        return objects.subList(0,10);
    }
    return objects;
}

// sortByValueDescending()方法返回一个降序的map，key值为tag（如“旅游攻略”），value值为占比数值（只需要key）

//降序排序
public static <K, V extends Comparable<? super V>> Map<K, V> sortByValueDescending(Map<K, V> map)
{
    List<Map.Entry<K, V>> list = new LinkedList<>(map.entrySet());
    Collections.sort(list, (o1, o2) -> {
        int compare = (o1.getValue()).compareTo(o2.getValue());
        return -compare;
    });

    Map<K, V> result = new LinkedHashMap<>();
    for (Map.Entry<K, V> entry : list) {
        result.put(entry.getKey(), entry.getValue());
    }
    return result;
}
```





#### 2）TagTask！

- 还有疑惑！1

> 定时更新最热标签，通过设置同类别的叠加（初始为5+评论数），如果已有值则改为（已有+5+评论数）

```java
@Component
public class TagTask {

    @Autowired
    private QuestionMapper questionMapper;

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Autowired
    private HotTagCache hotTagCache;

//20个小时更新一次热门???20分钟定时
    @Scheduled(fixedRate = 10000*6*20)
    public void printTest() {
        System.out.println(new Date()+"进入定时");
        int pageNo = 1;
        int pageSize = 10;
        List<Question> questions = new ArrayList<>();
        Map<String, Integer> properties = new HashMap<>();

//10条10条处理，刚开始pageNo为1，自动进入，然后每次读取，如果满10，则赋给questions,可以进入下次循环，如果不满十条则下次可以退出
        while (pageNo == 1 || questions.size() == pageSize) {
            //questions = questionExtMapper.findQuestionPage((pageNo - 1) * pageSize, pageSize);

            PageHelper.startPage(pageNo,pageSize);
            QuestionExample example = new QuestionExample();
            example.setOrderByClause("gmt_create desc");
            questions=questionMapper.selectByExample(example);
            for (Question question : questions) {
                String tagstr = question.getTag();
				
//下面这个for循环可能是防止有多个标签？？？，同时出现名字相同但有大小写之分？？？（默认是 '，' 分割吗？？？）
                String[] tags = tagstr.split(",");
                for (String tag : tags) {
                    tag=tag.trim().toLowerCase();
                    Integer integer = properties.get(tag);

                    if (null!=integer) {
                        properties.put(tag, integer + 5 + question.getCommentCount());
                    } else {
                        properties.put(tag, 5 + question.getCommentCount());
                    }
                }
            }
//页数加一       
            pageNo++;
        }
        
//while处理完后得到一个名为 properties 的Map ，里面存放的是（tag，每类占的数值），如（”旅游攻略”，15）;
        
//通过HotTagCache类 设置属性properties
        hotTagCache.setProperties(properties);
        
//调用HotTagCache类的updateTags()方法，更新热门标签
        //按权重排序
        List<String> tags = hotTagCache.updateTags();
        
        //清除之前的缓存
        //redisTemplate.delete("hot");
        //放到redis中
        //for (String tag : tags) {
            //redisTemplate.opsForList().rightPush("hot",tag);
        //}
    }


}
```



### 5、utils

#### 1)RedisUtils！

- 发现配置类没有被使用！，IndexController只是引用了这个类，没有使用。 -->使用了==StringRedisTemplate==！！！

```java
private RedisTemplate<String, String> redisTemplate;

//1、读取缓存
	redisTemplate.opsForValue().get(key);
//2、写入缓存
redisTemplate.opsForValue().set(key, value);
//3、更新缓存
redisTemplate.opsForValue().getAndSet(key, value);
//4、删除缓存
redisTemplate.delete(key);
```









[Calendar时间设置](https://blog.csdn.net/yx0628/article/details/79317440)

scheduletask包：定时任务 关联 modal包下 Data，Forecast，yesterday

modal包下weatherData ：无关联

```html
/** shift:6 为弹出框出现的形式，6为左右抖动**/
layer.msg（("test", {time: 2000, icon: 5, shift: 6});
```

```css
- "显示文本"
- {内容}：
	- shift:6 为弹出框出现的形式，6为左右抖动
    - time: 保存时间
    - icon 对应图标
layer.msg（("test", {time: 2000, icon: 5, shift: 6});
```



![效果](C:\Users\FX\AppData\Roaming\Typora\typora-user-images\image-20210331142807754.png)



引入响应式布局的rem根标签?(待测试)

```javascript

<script>
    (function(win,doc){
        var docEl = doc.documentElement || document.body;//获取HTML标签
        var container = doc.getElementById('container');//container元素
        //判断是移动端设备还是PC,移动 就采用'orientationchange',横竖屏事件，PC端就采用onresize，窗口改变时间
        var resize = 'onorientationchange' in win ? 'orientationchange':'resize';
        function rem(){
            docEl.style.fontSize= 100*(container.clientWidth/750)+'px';
        }

        //监听'DOMContent事件:DOM加载完成执行,如果DOMContent事件，那么执行rem函数
        doc.addEventListener('DOMContentLoaded',rem,false);

        //win下监听resize事件,如果resize事件，那么执行rem函数
        win.addEventListener(resize,rem,false);
    })(window,document);
</script>
```

$.parseJSON

## Api接口调用过程

### 1、旧版

```js
index.js

function eachnews(obj) {
    var str = '';
    if (obj.constructor == Array) {
        for (var i = 0, len = obj.length; i < len; i++) {
            var thetime = obj[i].time.split(" ");

            if (obj[i].time != "") {
                str += `<li>
                            <div class="cbp_tmtime">`;
                str += `<span>` + thetime[0] + `</span>`;
                str += `<span>` + thetime[1] + `</span>`;
                str += `</div>
                            <div class="cbp_tmlabel">`;
                str += `<h2>` + obj[i].title + `</h2>`;
                str += `<p style="font-size: 0.25rem;padding:10px 0 0 0;">` + obj[i].desc + `</p>`;
                str += `</div>`;
                str += `</li>`;
            }
        }
        return str;
    }
}

$.ajax({
        //mcx:https://view.inews.qq.com/g2/getOnsInfo?name=wuwei_ww_time_line
        //改了后：https://lab.isaaclin.cn/nCoV/api/news?page=1&num=20
        url: "https://view.inews.qq.com/g2/getOnsInfo?name=wuwei_ww_time_line", //请求的服务端地址
        type: "post",
        dataType: "JSONP",
        success: function (data) {
            //mcx:data.data
            var ssdata = JSON.parse(data.data);
            //mcx:原先注释了
            console.log(ssdata);
            //mcx:下面一行被注释掉
            var newestdata = ssdata.reverse().slice(0, 20); //20条数据
            var newswrap = $(".dongtai .main .cbp_tmtimeline");
            //mcx:newestdata
            //修改后：ssdata
            newswrap.html(eachnews(newestdata));

        },
        error: function () {
            console.log('请求数据出错');
        }
    });
```

### 2、解决

JSONP是什么格式

```

white-space: pre; //保存原来样式\n\t等
white-space:pre-line //合并空白，保留样式\n等
```

js：字符串变整形 直接*1 

js:时间戳转正常

```js
var date = new Date(时间戳[数字！不能是字符串]);

var Y = date.getYear();
var M = date.getMonth() + 1 ;
var D = date.getDate();

var transData1 = Y + "-" M + "-" + D;

var h = date.getHours();
var m = date.getMinutes();
var s = date.getSeconds();

var transDate2 = h + ":" + m + ":" + s;
```

js获取数字的长度:原格式不会被转换，除非重新赋值

```js
调用toString方法转为字符串后取长度
var num = 123;
alert(num.toString().length);

隐式转字符串后取长度
var num = 123;
alert((num + '').length)
```

判断类型 **typeof**

js 自动在数字类型前补齐0，缺点：格式被转为string

```js
//1
function PrefixInteger(num, length) {
  return (num/Math.pow(10,length)).toFixed(length).substr(2);
}

//2
function PrefixInteger(num, length) {
 return ( "0000000000000000" + num ).substr( -length );
}

//3
function PrefixInteger(num, length) {
 return (Array(length).join('0') + num).slice(-length);
}
```

js中new Date（参数）：参数需要为整形 ->将 字符串*1

字符串：返回Invalid Date

数字类型：Wed Mar 31 2021 09:43:46 GMT+0800 (中国标准时间)



dataList [{name，value}，{}]

![image-20210401141649278](C:\Users\FX\AppData\Roaming\Typora\typora-user-images\image-20210401141649278.png)

```
map_option
	- tooltip
	- visualMap
	- geo
	- series
map_echarts.setOption(map_option);
```



```js
// 全国疫情新增确诊/新增疑似趋势图
            //mcx:tojson.chinaDayAddList
            var addData10 = tojson.chinaDayAddList;
            //mcx
            console.log("test");
            console.log(addData10);
            var xData10 = [];
            var yData11 = [];

            var yData12 = [];
            for (var j = 0; j < addData10.length; j++) {
                xData10.push(addData10[j].date);
                yData11.push(addData10[j].confirm);
                yData12.push(addData10[j].suspect);
            }
            var echarts1 = echarts.init(document.getElementById('echarts1'));
            var echarts1_option = {
                title: {
                    top: '0',
                    left: 'left',
                    text: '全国疫情新增确诊/新增疑似趋势图',
                    textStyle: {
                        align: 'left',
                        color: '#000',
                        fontWeight: '500',
                        fontSize: 13,
                    }
                },
                backgroundColor: '#fff',
                legend: {
                    right: '0',
                    top: '20px',
                    itemWidth: 10,
                    itemHeight: 5,
                    itemGap: 15,
                    textStyle: {
                        color: '#333',
                        fontSize: 10,
                    },
                    icon: 'roundRect',
                    data: ['新增确诊', '新增疑似']
                },
                grid: {
                    right: '0',
                    bottom: '0',
                    left: '0',
                    top: '50px',
                    containLabel: true
                },
                xAxis: [{
                    type: 'category',
                    data: xData10,
                    axisLine: { //坐标轴轴线相关设置
                        lineStyle: {
                            color: "#aaa"
                        }
                    },
                    axisTick: { //坐标轴刻度相关设置
                        show: true,
                        lineStyle: {
                            color: '#aaa',
                            width: 1,
                        },
                        length: 3,
                    },
                    axisLabel: { //坐标轴刻度标签的相关设置
                        show: true,
                        // interval:1,
                        textStyle: {
                            color: "#aaa",
                            fontSize: 9,
                        },
                        rotate: 30
                    }
                }],
                yAxis: [{
                    type: 'value',
                    position: 'left',
                    interval: 1000,
                    max: 6000,
                    axisLine: { //坐标轴轴线相关设置
                        show: false
                    },
                    axisTick: { //坐标轴刻度相关设置
                        show: false
                    },
                    axisLabel: { //坐标轴刻度标签的相关设置
                        show: true,
                        color: "#aaa",
                        fontSize: 9

                    },
                    splitLine: { //坐标轴在 grid 区域中的分隔线
                        lineStyle: {
                            color: "#eee",
                        }
                    }
                }, ],
                series: [{
                        name: '新增确诊',
                        type: 'line',
                        yAxisIndex: 0,
                        symbolSize: 4,
                        itemStyle: {
                            normal: {
                                color: 'rgb(255,215,104)',
                            }
                        },
                        data: yData11
                    },
                    {
                        name: '新增疑似',
                        type: 'line',
                        yAxisIndex: 0,
                        symbolSize: 4,
                        itemStyle: {
                            normal: {
                                color: 'rgb(127,17,0)',
                            }
                        },
                        data: yData12
                    }

                ]
            };
            echarts1.setOption(echarts1_option);


```

```js
<div class="map_con" id="map_echarts" style="width:100%;height:6.5rem;">
var map_echarts = echarts.init(document.getElementById('map_echarts'));

<div class="map_con" id="echarts1" style="width:100%;height:6.5rem;">
var echarts1 = echarts.init(document.getElementById('echarts1'));
```



```html
nov.html
<div class="map_con" id="echarts1" style="width:100%;height:4.5rem; "></div>

<div class="map_con" id="echarts3" style="width:100%;height:4.5rem; "></div>
```

```js
index.js
// 全国疫情新增确诊/新增疑似趋势图
// var addData10 = tojson.chinaAdd;
// console.log(addData10);
// 无数据！！！
var xData10 = ["3-23","3-25","3-27","3-29","4-1"];
var yData11 = ["10","15","18","21","30"];
var yData12 = ["1","5","3","10","18"];
// for (var j = 0; j < addData10.length; j++) {
//     xData10.push(tojson.lastUpdateTime);
//     yData11.push(addData10.confirm);
//     yData12.push(addData10.suspect);
// }
var echarts1 = echarts.init(document.getElementById('echarts1'));
var echarts1_option = {
    title: {
        top: '0',
        left: 'left',
        text: '全国疫情新增确诊/新增疑似趋势图',
        textStyle: {
            align: 'left',
            color: '#000',
            fontWeight: '500',
            fontSize: 22,
        }
    },
    // backgroundColor: '#fff',
    legend: {
        right: '0',
        top: '20px',
        itemWidth: 10,
        itemHeight: 5,
        itemGap: 15,
        textStyle: {
            color: '#333',
            fontSize: 18,
        },
        icon: 'roundRect',
        data: ['新增确诊', '新增疑似']
    },
    grid: {
        right: '8%',
        bottom: '3%',
        left: '3%',
        top: '16%',
        containLabel: true
    },
    xAxis: [{
        type: 'category',
        //mcx:xData10
        data: xData10,
        axisLine: { //坐标轴轴线相关设置
            lineStyle: {
                color: "#6A6A6A"
            }
        },
        axisTick: { //坐标轴刻度相关设置
            show: true,
            inside: true,
            lineStyle: {
                color: '6A6A6A',
                width: 1,
            },
            length: 5,
        },
        axisLabel: { //坐标轴刻度标签的相关设置
            show: true,
            interval:0,
            // interval:1,
            textStyle: {
                color: "#6A6A6A",
                fontSize: 15,
            },
            //设置旋转
            rotate: 35,
            margin: 10
        }
    }],
    yAxis: [{
        type: 'value',
        position: 'left',
        name:'人数',
        interval: 10,
        max: 50,
        axisLine: { //坐标轴轴线相关设置\
            lineStyle: {
                color: "#6A6A6A"
            }
        },
        axisTick: { //坐标轴刻度相关设置
            show: false
        },
        axisLabel: { //坐标轴刻度标签的相关设置
            show: true,
            color: "#6A6A6A",
            fontSize: 15,
            margin: 10

        },
        splitLine: { //坐标轴在 grid 区域中的分隔线
            lineStyle: {
                color: "#B5B5B5",
                type:'dashed'
            }
        }
    }, ],
    series: [{
            name: '新增确诊',
            type: 'line',
            yAxisIndex: 0,
            symbolSize: 4,
            itemStyle: {
                normal: {
                    color: 'rgb(255,215,104)',
                }
            },
            data: yData11
        },
        {
            name: '新增疑似',
            type: 'line',
            yAxisIndex: 0,
            symbolSize: 4,
            itemStyle: {
                normal: {
                    color: 'rgb(127,17,0)',
                }
            },
            data: yData12
        }

    ]
};
echarts1.setOption(echarts1_option);

// 全国疫情累计确诊/治愈趋势图
var addData30 = tojson.chinaDayList;
var xData30 = [];
var yData31 = [];
var yData32 = [];
// for (var m = 0; m < addData30.length; m++) {
//     xData30.push(addData30[m].date);
//     yData31.push(addData30[m].dead);
//     yData32.push(addData30[m].heal);
// }
var xData30 = ["3-5","3-15","3-25","4-5","4-15"];
var yData31 = ["445.16","854.46","1012.12","1286.56","1457.51"];
var yData32 = ["120.32","305.12","500.56","790.15","1100.42"];
var echarts3 = echarts.init(document.getElementById('echarts3'));
var echarts3_option = {
    title: {
        top: '0',
        left: 'left',
        text: '全球疫情累计确诊/治愈趋势图',
        textStyle: {
            align: 'left',
            color: '#000',
            fontWeight: '500',
            fontSize: 22,
        }
    },
    legend: {
        right: '0',
        top: '20px',
        itemWidth: 10,
        itemHeight: 5,
        itemGap: 15,
        textStyle: {
            color: '#333',
            fontSize: 18,
        },
        icon: 'roundRect',
        data: ['累计确诊', '累计治愈']
    },
    grid: {
        right: '8%',
        bottom: '3%',
        left: '3%',
        top: '16%',
        containLabel: true
    },
    xAxis: [{
        type: 'category',
        data: xData30,
        axisLine: { //坐标轴轴线相关设置
            lineStyle: {
                color: "#6A6A6A"
            }
        },
        axisTick: { //坐标轴刻度相关设置
            show: true,
            inside: true,
            lineStyle: {
                color: '6A6A6A',
                width: 1,
            },
            length: 5,
        },
        axisLabel: { //坐标轴刻度标签的相关设置
            show: true,
            interval:0,
            textStyle: {
                color: "#6A6A6A",
                fontSize: 15,
            },
            //设置旋转
            rotate: 35,
            margin: 10
        }
    }],
    yAxis: [{
        type: 'value',
        position: 'left',
        name:'人数/万',
        interval: 200,
        min:100,
        max: 1500,
        axisLine: { //坐标轴轴线相关设置\
            lineStyle: {
                color: "#6A6A6A"
            }
        },
        axisTick: { //坐标轴刻度相关设置
            show: false
        },
        axisLabel: { //坐标轴刻度标签的相关设置
            show: true,
            color: "#6A6A6A",
            fontSize: 15,
            margin: 10

        },
        splitLine: { //坐标轴在 grid 区域中的分隔线
            lineStyle: {
                color: "#B5B5B5",
                type:'dashed'
            }
        }
    }, ],
    series: [{
        name: '累计确诊',
        type: 'line',
        yAxisIndex: 0,
        symbolSize: 4,
        itemStyle: {
            normal: {
                color: 'rgb(255,215,104)',
            }
        },
        data: yData31
    },
        {
            name: '累计治愈',
            type: 'line',
            yAxisIndex: 0,
            symbolSize: 4,
            itemStyle: {
                normal: {
                    color: 'rgb(127,17,0)',
                }
            },
            data: yData32
        }

    ]
};
echarts3.setOption(echarts3_option);
```





```html
最热标签 <tag_wrapper>
最近登入<list-group>
热门推荐<recommend_wrapper>
最近更新<newquestions_wrapper>
```

![image-20210402092435829](C:\Users\FX\AppData\Roaming\Typora\typora-user-images\image-20210402092435829.png)

![image-20210402093756996](C:\Users\FX\AppData\Roaming\Typora\typora-user-images\image-20210402093756996.png)

```
<ul style="margin-top: 30px!important;overflow: hidden;" id="page"></ul>
```

![image-20210402094139788](C:\Users\FX\AppData\Roaming\Typora\typora-user-images\image-20210402094139788.png)



```java
data.extend.sigined : 这个extend是ResultTypeDTO中的字段属性！！！
    
ResultTypeDTO：
private String code;

private String message;

private Map<String,Object> extend=new HashMap<>();

public ResultTypeDTO addMsg(String key, Object value){
    this.extend.put(key,value);
    return this;
}

//addMsg方法就是往extend这个Map丢值
```

## 数据库

### follow

- 通过status这一列实现逻辑删除，当status为1时表示已关注，为0时表示取消关注





#### 定时任务

1、主启动类添加注解：**@EnableScheduling**

2、方法放在一个实体类中：实体类添加注解：**@Compoment**

3、方法上添加注解：**@Scheduled**

> 属性：- **cron**
>
> ​			-**fixedRate**：上一个调用开始后再次调用的延时（不用等待上一次调用完成），这样就会存在重复执行的问题
>
> ​			-**fixedDelay**：等到方法执行完成后延迟配置的时间再次执行该方法
>
> ​			-**initialDelay**：第一次执行延迟时间，只是做延迟的设定，并不会控制其他逻辑，所以要配合fixedDelay或者fixedRate来使用
>
> 配置时间单位都是毫秒





热门推荐数据的选择是根据评论数，点赞数，观看数（从左到右一个个执行，相同才比较下一个）



分页插件

```yaml
pagehelper.helper-dialect=mysql #标识是哪一种数据库，设计上必须
pagehelper.params=count=countSql
pagehelper.reasonable=false
pagehelper.support-methods-arguments=true
```

```xml
配置

<plugins>
    <!-- com.github.pagehelper为PageHelper类所在包名 -->
    <plugin interceptor="com.github.pagehelper.PageHelper">
        <property name="dialect" value="mysql"/>
        <!-- 该参数默认为false -->
        <!-- 设置为true时，会将RowBounds第一个参数offset当成pageNum页码使用 -->
        <!-- 和startPage中的pageNum效果一样-->
        <property name="offsetAsPageNum" value="false"/>
        <!-- 该参数默认为false -->
        <!-- 设置为true时，使用RowBounds分页会进行count查询 -->
        <property name="rowBoundsWithCount" value="false"/>
        <!-- 设置为true时，如果pageSize=0或者RowBounds.limit = 0就会查询出全部的结果 -->
        <!-- （相当于没有执行分页查询，但是返回结果仍然是Page类型）-->
        <property name="pageSizeZero" value="true"/>
        <!-- 3.3.0版本可用 - 分页参数合理化，默认false禁用 -->
        <!-- 启用合理化时，如果pageNum<1会查询第一页，如果pageNum>pages会查询最后一页 -->
        <!-- 禁用合理化时，如果pageNum<1或pageNum>pages会返回空数据 -->
        <property name="reasonable" value="true"/>
        <!-- 3.5.0版本可用 - 为了支持startPage(Object params)方法 -->
        <!-- 增加了一个`params`参数来配置参数映射，用于从Map或ServletRequest中取值 -->
        <!-- 可以配置pageNum,pageSize,count,pageSizeZero,reasonable,不配置映射的用默认值 -->
        <!--<property name="params" value="pageNum=start;pageSize=limit;pageSizeZero=zero;reasonable=heli;count=contsql"/>-->
    </plugin>
</plugins>
```

## 缓存



项目中用到缓存的地方，热门标签和积分！（积分的缓存名字乱码？无法打开）

- ==SpringRedisTemplate==

- RedisTemplate

这两个在同个程序中使用，RedisTemplate添加的缓存名称出现乱码等现象，坑！

> ​               key            ：        value
>
> signIn:2021-04-02 ：用户id.toString()

```java
//查找用户是否存在于今日的缓存当中
redisTemplate.opsForSet().isMember(key, id.toString()); 
//无，则添加用户到缓存，并且设置失效时间
redisTemplate.opsForSet().add(key,id.toString());
this.redisTemplate.expire(key,getRefreshTime(), TimeUnit.SECONDS);
```

![image-20210402175816753](C:\Users\FX\AppData\Roaming\Typora\typora-user-images\image-20210402175816753.png)

