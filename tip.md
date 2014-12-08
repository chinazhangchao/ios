
##UITableView

1.滑动删除时按钮不出现时要修改tableview的constraint，删除tableviewcell前先删除数据源。

2.滑动删除view回退崩溃时可以只实现commitEditingStyle委托方法来解决。


##UILable
1.lable lines 不为0时无法换行，在storyboard中设置时尤其需要注意。

##UIScrollView
1.使用Auto Layout时需嵌套一个UIView，在UIView里面进行布局。

```objectivec
UIView *contentView = [[UIView alloc]
    initWithFrame:CGRectMake(0,0,contentWidth,contentHeight)];
[scrollView addSubview:contentView];
// DON'T change contentView's translatesAutoresizingMaskIntoConstraints,
// which defaults to YES;
Set the content size of the scroll view to match the size of the content view:
 [scrollView setContentSize:CGMakeSize(contentWidth,contentHeight)];
```

##Container
1.NSSet、NSDictionary默认调用isEqual进行比较。


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

##XMPPFramework xcode编译出错处理
1.OTHER_LDFLAGS 添加 -lxml2 -lresolv

2.HEADER_SEARCH_PATHS 添加 /usr/include/libxml2/