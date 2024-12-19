# flutter 开发日常总结
****
* 当Wrap组件包裹的子组件需要滚动时，这样写：
```
SizedBox(
  height: 100.0,   // 自定义高度
  child: SingleChildScrollView(
    child: Wrap(
      children: [
      ],
    ),
  ),
)


```

****

* 当ListView没有和AppBar一起使用时，头部会有一个padding，为了去掉padding，可以使用MediaQuery.removePadding

```
Widget _listView(BuildContext context){
    return MediaQuery.removePadding(
      removeTop: true,
      context: context,
      child: ListView.builder(
        itemCount: 10,
        itemBuilder: (context,index){
          return _item(context,index);
        },

      ),
    );
  }
```
****
* 自定义带水波纹的button按钮

```
Ink(
        decoration: BoxDecoration(
            gradient: LinearGradient(
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
                colors: [Color(0xFFDE2F21), Color(0xFFEC592F)]),
            borderRadius: BorderRadius.all(Radius.circular(20))),
        child: InkWell(
          borderRadius: BorderRadius.all(Radius.circular(20)),
          child: Container(
            padding: EdgeInsets.symmetric(vertical: 8, horizontal: 20),
            child: Text(
              '这是InkWell的点击效果',
              style: TextStyle(color: Colors.white),
            ),
          ),
          onTap: () {},
        ),
      )

```
***

* Flutter弹起键盘页面布局超限问题

```
Scaffold(
   resizeToAvoidBottomInset: false,
)
```
***
* TextField 去掉下划线

 ```
  TextField(
    decoration: InputDecoration(
    border: InputBorder.none,
    ),
  )
 
 ```
 ***
 
 * 利用TabBar+TabBarView 切换页面并缓存
 ```
 
class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key}) : super(key: key);

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage>
    with SingleTickerProviderStateMixin {


  final List<String> _tabs = [
    '语文',
    '英语',
    '化学',
    '物理',
    '数学',
    '生物',
    '体育',
    '经济',
  ];

  late TabController _tabController;

  @override
  void initState() {
    super.initState();

    _tabController = TabController(length: _tabs.length, vsync: this);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: const Text("TabView效果演示"),
          bottom: TabBar(
            isScrollable: true,
            unselectedLabelColor: Colors.red,
            tabs: _tabs.map((e) => Text(e)).toList(),
            controller: _tabController,
          ),
        ),
        body: TabBarView(
            controller: _tabController,
            children: _tabs
                .map((e) =>ListDemo())
                .toList()
                
                )
                );
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }
}

class ListDemo extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return ListDemoState();
  }
}

class ListDemoState extends State<ListDemo> with AutomaticKeepAliveClientMixin{
  @override
  Widget build(BuildContext context) {
    super.build(context);
    return ListView.builder(itemBuilder: (context, index) => Text("内容$index"),itemCount: 100,);
  }

  @override
  bool get wantKeepAlive => true;
}

 
 ```
<strong>AutomaticKeepAliveClientMixin 一定要放在缓存的页面 </strong>

***
* 背景随机颜色
```
Colors.primaries

 // 与索引相关
Colors.primaries[index]
 // 与索引相关
Colors.primaries[index % Colors.primaries.length]

```
***
 * TabBarView 页面中间切换
 ```
 class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key}) : super(key: key);

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> with SingleTickerProviderStateMixin {

  final List<String> _tabs = [
    '语文',
    '英语',
    '化学',
    '物理',
    '数学',
    '生物',
    '体育',
    '经济',
  ];

    late TabController  tabController;

  @override
  void initState() {
    super.initState();
    tabController = TabController(length: _tabs.length, vsync: this);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: NestedScrollView(
        headerSliverBuilder: (context, innerBoxIsScrolled) {
          return <Widget>[
            SliverAppBar(
              elevation: 0,
              expandedHeight: 300,
              pinned: true,
              flexibleSpace:FlexibleSpaceBar(
                 title: Text("demo"),
              )
             
            ),
            SliverPersistentHeader(
              pinned: true,
              delegate: StickyTabBarDelegate(
                child: TabBar(                
                  isScrollable: true,
                  controller: tabController,
                  tabs: _tabs.map((e) => Tab(text: e,)).toList()
                )
              )
            )
          ];
        },
        body: TabBarView(
          controller: tabController,
          children: _tabs.map((e) => 
           MediaQuery.removePadding(
             removeTop: true,
             context: context,
              child: const MyList()
           )
          ).toList())
      ),
    );
  }
  @override
  void dispose() {
    super.dispose();
    tabController.dispose();
  }
}

class StickyTabBarDelegate extends SliverPersistentHeaderDelegate {
  final TabBar child;

  StickyTabBarDelegate({ required this.child});

  @override
  Widget build(
      BuildContext context, double shrinkOffset, bool overlapsContent) {
    return Container(
      color: Colors.blue,
      child: child,
    );
  }

  @override
  double get maxExtent => child.preferredSize.height;

  @override
  double get minExtent => child.preferredSize.height;

  @override
  bool shouldRebuild(SliverPersistentHeaderDelegate oldDelegate) {
    return true;
  }
}



class MyList extends StatefulWidget {
  const MyList({super.key});

  @override
  State<MyList> createState() => _MyListState();
}

class _MyListState extends State<MyList> with AutomaticKeepAliveClientMixin {
  @override
  Widget build(BuildContext context) {
    super.build(context);
    return  ListView(
              children: List.generate(50, (index) => Container(
              height: 50,
              child: Center(
                child: Text("${index +1}"),
              ),
              color: Colors.primaries[index % Colors.primaries.length],
            )
            ),
            );
  }

  @override
  bool get wantKeepAlive =>true;
}

 
 ```
 ***
 
 ```
 // 防抖
mixin Debouce {
  final Duration  duration = const Duration(seconds: 2);
  late Timer _timer;

  debouce(VoidCallback action){
     _timer.cancel();
    _timer = Timer(duration, action);
  }
}

// 节流
mixin Throttle {
  int _lastTime = 0;
  final int _interval = 2000;
  throttle(VoidCallback action){
    if(DateTime.now().microsecondsSinceEpoch -_lastTime > _interval){
      action();
      _lastTime = DateTime.now().microsecondsSinceEpoch;
    }

  }
}

 ```

***

* 阴影
```
boxShadow: const [
                  BoxShadow(
                    color: Color.fromRGBO(0, 0, 0, 0.1),
                    offset: Offset(0, 0),
                    blurRadius: 2,
                    spreadRadius: 0,
                  )
                ]
                
```
***
* flutter 复制到剪切板<br/>
1.引入包
```
import 'package:flutter/services.dart';
```
2.复制文本到剪贴板,该方法无返回值
```
Clipboard.setData(ClipboardData(text: "大家好，我是博主 Allen Su"));

```
3.从剪贴板粘贴文本
```
void _doRead() async {
  var text = await Clipboard.getData(Clipboard.kTextPlain); // 此时的 text 类型为 ClipboardData
  print("复制的内容是：${text.text}"); // text.text 就是从剪贴板粘贴的内容
}

```
***

颜色线性渐变

```
decoration: BoxDecoration(
      gradient: LinearGradient(      //渐变位置
         begin: Alignment.topRight, //右上
         end: Alignment.bottomLeft, //左下
         stops: [0.0, 1.0],         //[渐变起始点, 渐变结束点]
         //渐变颜色[始点颜色, 结束颜色]
         colors: [Color.fromRGBO(63, 68, 72, 1), Color.fromRGBO(36, 41, 46, 1)]
      )
    )
```

***

flutter中TextField设置文本内容后光标在文本末尾

```
     //改变焦点位置
     
  _controller.selection = TextSelection.fromPosition(TextPosition(offset: _controller.text.length));
  
  setState(() {});

```

***

```
// 自定义单选按钮

Row(
      crossAxisAlignment: CrossAxisAlignment.center,
      children: <Widget>[
        Radio(
          value: value,
          groupValue: groupValue,
          onChanged: onChanged,
          materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
        ),
        SizedBox(width: 8.0), // 调整这个值来减少按钮和文本之间的间距
        Text(
          title,
          style: TextStyle(fontSize: 16), // 可以根据需要调整文本的样式
        ),
      ],
    );

```

***

```
 // BottomNavigationBar 去除按钮水波纹和按钮放大

theme: ThemeData(

splashColor: Colors.transparent, // 点击时的高亮效果设置为透明

        highlightColor: Colors.transparent, // 长按时的扩散效果设置为透明

）

bottomNavigationBar: BottomNavigationBar(

        type: BottomNavigationBarType.fixed, // 固定类型

……

）

```


