
##UITableView

1.滑动删除时按钮不出现时要修改tableview的constraint，删除tableviewcell前先删除数据源。
2.滑动删除view回退崩溃时可以只实现commitEditingStyle委托方法来解决。


##Container
1.NSSet、NSDictionary默认调用isEqual进行比较。


##CoreData
1.NSFetchRequest分组去重查询时返回类型为dictionary，处理方式如下：

	request.resultType = NSDictionaryResultType;
	request.propertiesToFetch = [NSArray arrayWithObject:[[entityDescription propertiesByName] 	objectForKey: @"bareJidStr"]];
	request.returnsDistinctResults = YES;
	
	NSArray *jidArr = [context executeFetchRequest:request error:&error];
	NSMutableArray *message = [NSMutableArray new];
	NSSortDescriptor *ageDescriptor = [[NSSortDescriptor alloc] initWithKey:@"timestamp" 	ascending:NO];
	NSArray *sortDesArr = @[ageDescriptor];
	for (NSDictionary *jidDic in jidArr) {
	    NSString *jid = [jidDic allValues][0];
	}

##XMPPFramework xcode编译出错处理
OTHER_LDFLAGS 添加 -lxml2 -lresolv
HEADER_SEARCH_PATHS 添加 /usr/include/libxml2/