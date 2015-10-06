#1. 引用了cocoapods，achieve出other items
  答：选中pods project，选择build setting中的Deployment，把skip install改为Yes，如果还是不行，选中不行的target，build phases，把Headers中的h文件从public中移到Project中。
  
#2. 自定义返回按钮内容
UIBarButtonItem *returnButtonItem = [[UIBarButtonItem alloc] init];
returnButtonItem.title = @"返回";
self.navigationItem.backBarButtonItem = returnButtonItem;

#3.如何点击uitableview 的HeaderInSection隐藏或展开自section
首先定义一个标识符数组，用来表示每个section的展开收起状态

设置每个section的row个数的方法中，加入一个对标识符的判断
判断为 展开，即按照设定的个数来显示row（每个section的row个数，可以存在一个数组里）
判断为收起，则返回0

在header上添加button，绑定方法
方法中，根据button的tag值来判断点击的是哪个section，然后对响应section的标识符进行更改

基本流程就这样了

我写的练习代码供参考（没粘贴变量的声明部分，新手，见笑了...）：

//点击header的方法
-(void)tapHeader:(UIButton *)sender
{
    if ([[openedInSectionArr objectAtIndex:sender.tag - 100] intValue] == 0) {
        [openedInSectionArr replaceObjectAtIndex:sender.tag - 100 withObject:@"1"];
        NSLog(@"%d打开",sender.tag);
    }
    else
    {
        [openedInSectionArr replaceObjectAtIndex:sender.tag - 100 withObject:@"0"];
        NSLog(@"%d关闭",sender.tag);
    }
    
    [myTableView reloadData];
}

-(NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{

    return [mySectionArr count];
}

-(CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
{
    return 40;
}

-(UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    
    UIView * mySectionView = [[[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 40)] autorelease];
    mySectionView.backgroundColor = [UIColor orangeColor];
    UIButton * myButton = [UIButton buttonWithType:UIButtonTypeCustom];
    myButton.imageView.image = [UIImage imageNamed:@"淡蓝头.png"];
    myButton.frame = CGRectMake(0, 0, 320, 40);
    myButton.tag = 100 + section;
    [myButton addTarget:self action:@selector(tapHeader:) forControlEvents:UIControlEventTouchUpInside];
    [mySectionView addSubview:myButton];
    
    UILabel * myLabel = [[UILabel alloc] initWithFrame:CGRectMake(35, 0, 150, 30)];
    myLabel.backgroundColor = [UIColor clearColor];
    myLabel.text = [mySectionArr objectAtIndex:section];
    [myButton addSubview:myLabel];
    [myLabel release];
    
    return mySectionView;
}

-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
//    判断section的展开收起状态
    if ([[openedInSectionArr objectAtIndex:section] intValue] == 1) {
        return [[rowOfSectionArr objectAtIndex:section] intValue];
    }
    
    return 0;
}

-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    [self.navigationController pushViewController:memberInfoVC animated:YES];
}

-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    NSString * string = @"cell";
    UITableViewCell * cell = [tableView dequeueReusableCellWithIdentifier:string];
    if (!cell) {
        cell = [[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:string] autorelease];
    }
    cell.textLabel.text = [mySectionArr objectAtIndex:indexPath.section];

return cell;
}



// Implement loadView to create a view hierarchy programmatically, without using a nib.
- (void)loadView
{
    UIView * myView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 480)];
    myView.backgroundColor = [UIColor groupTableViewBackgroundColor];
    self.view = myView;
    [myView release];
    
    memberInfoVC = [[MemberInfo alloc] init];

//    sectionheader文字
    mySectionArr = [[NSMutableArray alloc] initWithObjects:@"猪头", @"笨蛋", @"傻瓜", @"神经病", nil];
//    每个section中的row个数
    rowOfSectionArr = [[NSMutableArray alloc] initWithObjects:@"2", @"3", @"4", @"2", nil];
//    每个section展开收起状态标识符
    openedInSectionArr = [[NSMutableArray alloc] initWithObjects:@"0", @"0", @"0", @"0",nil];

myTableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, 320, 418) style:UITableViewStylePlain];
    myTableView.dataSource = self;
    myTableView.delegate = self;
    [self.view addSubview:myTableView];
    [myTableView release];

}

#4.textview显示边框
textview.layer.borderColor = UIColor.grayColor.CGColor;
textview.layer.borderWidth = 5; 

#5.模拟器弹出键盘
虚拟机设置里面 Hardware - keyboard 内设置了 链接电脑键盘。你可以点击第三个：toggle software keyboard让虚拟机弹出，或者把 connect hardware keyboard 不勾选，这样就能自动弹出虚拟键盘了（不过电脑键盘就输入不了了） 
