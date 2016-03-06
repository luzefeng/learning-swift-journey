##1 App crashes with reason: Collection <__NSArrayM: 0x7071700> was mutated while being enumerated
原因：是在一个数组序列化的时候对其进行修改导致的错误，我是在writeToFile的时候修改了数组

修改：在序列化的时候先深拷贝给一个临时数组，让临时数组进行序列化
```swift
    NSArray *tempArray = [self.mpDownloadArray mutableCopy];
    [tempArray writeToFile:filename atomically:NO];
```

##2 TabBarController带了3个NavigationController页面的storyboard，首次启动时显示的第一个NavigationController里所有push出来的页面都不调用ViewDidAppear，如果切换到第二个页面再切回来，所有的都会调用了 

修改：改为在viewwillappear里调用，或者用下面方法
```swift
在tabbarviewcontroller中添加2个方法

- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController {
    [viewController viewWillAppear:NO];
}

-(void)viewWillAppear:(BOOL)animated
{
    
    [super viewWillAppear:animated];
    [self.tabBarController.selectedViewController viewWillAppear:animated];

}
```
