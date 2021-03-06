##Apple Developer Program telephone support
https://developer.apple.com/contact/phone.php

##出现莫名其妙的问题时试试清理重编，傻X的Xcode识别改动文件的能力太差

##AppIcon Size

29x29、40x40、58x58、76x76、80x80、87x87、120x120、152x152、167x167、180x180。

##LaunchImage Size

320 x 480、640 x 960、640 x 1136、750 x 1334、768 x 1024、1242 x 2208、1536 x 2048。

Default.png: 320 x 480

Default@2x.png: 640 x 960

Default-568h@2x.png: 640 x 1136

Default-667h@2x.png: 750 x 1334 (iPhone 6, Portrait)

Default-736h@3x.png: 1242 x 2208 (iPhone 6 Plus, Portrait)

Default-Landscape-568h@2x.png: 1136 x 640

Default-Landscape-667h@2x.png: 1334 x 750 (iPhone 6, Landscape)

Default-Landscape-736h@3x.png: 2208 x 1242 (iPhone 6 Plus, Landscape)

Default-Portrait.png: 768 x 1024

Default-Portrait@2x.png: 1536 x 2048

Default-Landscape.png: 1024 x 768

Default-Landscape@2x.png: 2048 x 1536

##Pods

pod 'Reveal-iOS-SDK', :configurations => ['Debug']

pod 'FLEX', '~> 2.0', :configurations => ['Debug']

<br/>
pod 'CocoaLumberjack'
pod 'AFNetworking', '~> 3.0'
pod 'Harpy'

<br/>
pod 'FMDB/SQLCipher', :head

pod 'LKDBHelper', :head

pod 'MJExtension'

pod 'PAPreferences'

<br/>
pod 'Masonry'

pod 'CYLTabBarController'

pod 'RadioButton'

pod 'DropdownMenu'

platform :ios, '8.0'

pod 'CTAssetsPickerController',  '~> 3.3.0'

pod "MWPhotoBrowser"

pod 'ALCameraViewController'

pod 'RSKImageCropper'

pod "DZNPhotoPickerController"

pod 'DZNPhotoPickerController/Editor'

pod 'InAppSettingsKit'

pod 'MBProgressHUD'


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

3.Category新添加的方法如果和已经存在的方法具有相同的prototype，那么新添加的方法将会覆盖已经存在的方法。<font color="red">在使用多个第三库时尤其要注意AppDelegate的回调被覆盖。</font>

不建议在 category 中覆盖类中的方法，因为在 category 中的方法不能调用 superClass 的方法（因为没有元数据支持）。

category 方法不能覆盖于同一class 的其它 category 中的方法。因为不法预知他们的加载优先顺序，就可能在编译时出错。

4.代码块的使用

```objectivec
typedef void (^CCLoginBoardBlock)();
@property (nonatomic, copy) CCLoginBoardBlock loginSucceed;
if (self.loginSucceed != nil) {
                    self.loginSucceed();
}
```

##ImageSize
1.launch image:320x480、640x960、640x1136、750x1334、1242x2208

##Container
1.NSSet、NSDictionary默认调用isEqual进行比较。

##NSURL
1.本地文件路径使用fileURLWithPath。

##Storyboard
1.xib与storyboadr相比，存在诸多限制，尽量使用storyboard。

##NSLayoutConstraint
1.代码添加constraint时要设置setTranslatesAutoresizingMaskIntoConstraints: NO。

```objectivec
\#define ADD_CONSTRAINT(attr, num) \
[self addConstraint: [NSLayoutConstraint constraintWithItem: seperator \
                                                  attribute: attr \                                                  
                                                  relatedBy: NSLayoutRelationEqual \
                                                     toItem: self \
                                                  attribute: attr \
                                                 multiplier: 1 \
                                                   constant: num]];

\#define ADD_SINGLE_CONSTRAINT(attr, num) \
[seperator addConstraint: [NSLayoutConstraint constraintWithItem: seperator \
                                                  attribute: attr \
                                                  relatedBy: NSLayoutRelationEqual \
                                                     toItem: nil \
                                                  attribute: NSLayoutAttributeNotAnAttribute \
                                                 multiplier: 1 \
                                                   constant: num]];
                                                   
UIView *seperator = [UIView new];
[seperator setTranslatesAutoresizingMaskIntoConstraints: NO];
seperator.backgroundColor = [ViewHelper tableViewSeperatorColor];
[self addSubview: seperator];

ADD_CONSTRAINT(NSLayoutAttributeLeading, 15.0)
ADD_CONSTRAINT(NSLayoutAttributeTrailing, 8.0)
ADD_CONSTRAINT(NSLayoutAttributeBottom, 0.0)
ADD_SINGLE_CONSTRAINT(NSLayoutAttributeHeight, 0.8)
```

2.6plus下约束表现不一样时注意关掉约束里面的Relative to margin选项。

##UIGestureRecognizer
1.多种手势并存时通过requireGestureRecognizerToFail指定优先级，参数里面的手势优先。

```objectivec
[tap requireGestureRecognizerToFail:longPress];
```

##UIView
1.貌似通过segue、navigation导航出现的视图，在回退时会被回收，下次重新执行viewdidload。在UITabBarViewController里的视图不会被回收，下次显示时不执行viewdidload。

2.创建高度为0.5(头发丝)的view。

```objectivec
+ (void)changeToHairLine:(UIView*)v
{
    v.layer.borderColor = [v.backgroundColor CGColor];
    v.layer.borderWidth = (1.0 / [UIScreen mainScreen].scale) / 2;
    v.backgroundColor = [UIColor clearColor];
}
```
##UILable
1.lable lines 不为0时无法换行，在storyboard中设置时尤其需要注意。

##UIImageView
1.添加点击手势时，将userInteractionEnabled属性设置为YES。

```objectivec
self.portraitImageView.userInteractionEnabled = YES;
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapPortrait)];
[self.portraitImageView addGestureRecognizer:tap];
```
2.设置圆角

```objectivec
self.pictureImageView.layer.masksToBounds = YES;
self.pictureImageView.layer.cornerRadius = 2.0;
```

##UIButton
1.设置UIButton图片时需要将Type改为Custom，在storyboard中设置时尤其需要注意。设置时需要同时设置UIControlStateNormal和UIControlStateHighlighted两种状态，否则点击时图片有可能旋转一下。

2.通过在button重叠位置放置imageview和label控件实现button同时设置图片和文字。

##UITextField
1.UITextField border style改为非RoundedRect后（在storyboard或xib修改即可）可以修改高度。
2.修改边框颜色时需同时设置边框宽度，否则不起作用。tf.layer.borderColor、tf.layer.borderWidth。

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

2.滑动删除view回退崩溃时可以只实现commitEditingStyle委托方法来解决。删section必须用deleteSections。 删row必须用deleteRowsAtIndexPaths。

3.tableview content为static cells方式时才可以直接连接cell内的控件到TabViewController的outlet。

4.静态cell只可以出现在storyboard中，不可以在xib中，只可以在UITableViewController，不可以在UIViewController。

5.contentInset在backgroundView或tableHeaderView之后设置会导致刚显示时无效（奇葩！）。

6.解决UITableView的无法正常显示最后一行的问题：

```objectivec
tableView.autoresizingMask= UIViewAutoresizingFlexibleHeight|UIViewAutoresizingFlexibleWidth;
```
##UICollectionView
1.只有StoryBoard里面的UICollectionView有UICollectionViewCell。如果使用XIB，需要创建一个包含CollectionViewCell的xib文件，然后使用registerNib。

Only UICollectionView inside StoryBoard have UICollectionViewCell inside. If use XIB, create a new XIB with CellName.xib, add CollectionViewCell to it, specify name of UICollectionView custom class. After that use registerNib.

##UINavigationController
1.navigationbar默认有透明效果，需手动关闭透明。

```objectivec
self.navigationController.navigationBar.translucent = NO;
```

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

#import libxml/tree.h file not found
set HeaderSearchPath and LibrarySearchPaths to /usr/include/libxml2
