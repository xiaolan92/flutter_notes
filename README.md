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

