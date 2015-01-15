##Objective-C

1.在Objective-C中向nil发送消息是完全有效的——只是在运行时不会有任何作用。Cocoa中的几种模式就利用到了这一点。发向nil的消息的返回值也可以是有效的:

 * 如果一个方法返回值是一个对象，那么发送给nil的消息将返回0(nil)。例如：Person * motherInlaw = [ aPerson spouse] mother]; 如果spouse对象为nil，那么发送给nil的消息mother也将返回nil。

 * 如果方法返回值为指针类型，float，double，long double 或者long long的整型标量，发送给nil的消息将返回0。

 * 如果方法返回值为结构体，正如在《Mac OS X ABI 函数调用指南》，发送给nil的消息将返回0。结构体中各个字段的值将都是0。

 * 如果方法的返回值不是上述提到的几种情况，那么发送给nil的消息的返回值将是未定义的。


2.扩展(Extension)是一种匿名分类(Category)；但是和分类不一样的是，扩展可以添加新的实例变量。
```objectivec
@interface MyClass () { //注意此处：扩展  
    float value;  
}  
- (void)setValue:(float)newValue;  
@end
```

##Container
1.NSSet、NSDictionary默认调用isEqual进行比较。

##NSURL
1.本地文件路径使用fileURLWithPath。

##Storyboard
1.xib与storyboadr相比，存在诸多限制，尽量使用storyboard。

##UIView
1.貌似通过segue、navigation导航出现的视图，在回退时会被回收，下次重新执行viewdidload。在UITabBarViewController里的视图不会被回收，下次显示时不执行viewdidload。

##UILable
1.lable lines 不为0时无法换行，在storyboard中设置时尤其需要注意。

##UIButton
1.设置UIButton图片时需要将Type改为Custom，在storyboard中设置时尤其需要注意。设置时需要同时设置UIControlStateNormal和UIControlStateHighlighted两种状态，否则点击时图片有可能旋转一下。

2.通过在button重叠位置放置imageview和label控件实现button同时设置图片和文字。

##UIScrollView
1.使用Auto Layout时需嵌套一个UIView，在UIView里面进行布局。

```objectivec
UIView *contentView = [[UIView alloc]
    initWithFrame:CGRectMake(0,0,contentWidth,contentHeight)];
[scrollView addSubview:contentView];
// Set the content size of the scroll view to match the size of the content view:
 [scrollView setContentSize:CGMakeSize(contentWidth,contentHeight)];
```

##UITableView

1.滑动删除时按钮不出现时要修改tableview的constraint，删除tableviewcell前先删除数据源。

2.滑动删除view回退崩溃时可以只实现commitEditingStyle委托方法来解决。

3.tableview content为static cells方式时才可以直接连接cell内的控件到TabViewController的outlet。

4.静态cell只可以出现在storyboard中，不可以在xib中。

##UISearchDisplayController
1.tableview的上下constraint会造成UISearchDisplayController全屏后不正确的遮盖tableview，使用UISearchDisplayController时不要对tableview设置上下constraint。可以设置height constraint。

2.tableview的左右constraint会造成UISearchDisplayController全屏后取消按钮的位置不正确，使用UISearchDisplayController时不要对tableview设置左右constraint。可以设置width constraint。

##UINavigationItem
1.在storyboard中往navigationitem里面添加的按钮会使标题位置偏移（不居中），在代码中添加不会导致此效果。

##UIWebView
1.设置webview与屏幕相同大小以便网页内容自动适应。

##UITabBarController
1.UITabBarItem的图片设置应该在视图加载前就调用，在appDelegate didFinishLaunchingWithOptions方法中设置。否则不起作用。

##CoreData
1.NSFetchRequest分组去重查询时返回类型为dictionary，处理方式如下：

```objectivec
request.resultType = NSDictionaryResultType;
request.propertiesToFetch = [NSArray arrayWithObject:[[entityDescription propertiesByName] objectForKey: @"bareJidStr"]];
request.returnsDistinctResults = YES;
	
NSArray *jidArr = [context executeFetchRequest:request error:&error];
NSMutableArray *message = [NSMutableArray new];
NSSortDescriptor *ageDescriptor = [[NSSortDescriptor alloc] initWithKey:@"timestamp" 	ascending:NO];
NSArray *sortDesArr = @[ageDescriptor];
for (NSDictionary *jidDic in jidArr) {
    NSString *jid = [jidDic allValues][0];
}
```

2.修改model后需删除旧存储重新生成，否则读取时会崩溃。

##XMPPFramework xcode编译出错处理
1.OTHER_LDFLAGS 添加 -lxml2 -lresolv

2.HEADER_SEARCH_PATHS 添加 /usr/include/libxml2/
