#1. 引用了cocoapods，achieve出other items
  答：选中pods project，选择build setting中的Deployment，把skip install改为Yes，如果还是不行，选中不行的target，build phases，把Headers中的h文件从public中移到Project中。
  
#2. 自定义返回按钮内容
````swift
UIBarButtonItem *returnButtonItem = [[UIBarButtonItem alloc] init];
returnButtonItem.title = @"返回";
self.navigationItem.backBarButtonItem = returnButtonItem;
````
#3.如何点击uitableview 的HeaderInSection隐藏或展开自section
首先定义一个标识符数组，用来表示每个section的展开收起状态

设置每个section的row个数的方法中，加入一个对标识符的判断
判断为 展开，即按照设定的个数来显示row（每个section的row个数，可以存在一个数组里）
判断为收起，则返回0

在header上添加button，绑定方法
方法中，根据button的tag值来判断点击的是哪个section，然后对响应section的标识符进行更改

基本流程就这样了

我写的练习代码供参考（没粘贴变量的声明部分，新手，见笑了...）：
````swift
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
````
#4.textview显示边框
````swift
textview.layer.borderColor = UIColor.grayColor.CGColor;
textview.layer.borderWidth = 5; 
````
#5.模拟器弹出键盘
虚拟机设置里面 Hardware - keyboard 内设置了 链接电脑键盘。你可以点击第三个：toggle software keyboard让虚拟机弹出，或者把 connect hardware keyboard 不勾选，这样就能自动弹出虚拟键盘了（不过电脑键盘就输入不了了） 

#6.textview和textfield隐藏键盘
````swift
- (BOOL)textFieldShouldReturn:(UITextField *)textField
{
    [textField resignFirstResponder];    
    return YES;
}

func textView(textView: UITextView, shouldChangeTextInRange range: NSRange, replacementText text: String) -> Bool {
        if text == "\n"{
            textView.resignFirstResponder()
            return false
        }
        
        return true
}
````
还有就是添加tapgesture，点击view隐藏键盘

#7获取string长度和判断string是否为空格
````swift
extension String {
    
    // readonly computed property
    var length: Int {
        return self.characters.count
    }
}
if self.textView?.text.length == 0 || textView?.text.stringByTrimmingCharactersInSet(NSCharacterSet.whitespaceAndNewlineCharacterSet()).length == 0{
}
````
#8 实现textview的placeholder文字
````swift
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
   
    label = [[UILabel alloc]initWithFrame:CGRectMake(3, 3, 200, 20)];
    label.enabled = NO;
    label.text = @"在此输入反馈意见";
    label.font =  [UIFont systemFontOfSize:15];
    label.textColor = [UIColor lightGrayColor];
    [self.textView addSubview:label];
}
//TextView Delegate
- (void) textViewDidChange:(UITextView *)textView{
    if ([textView.text length] == 0) {
        [label setHidden:NO];
    }else{
        [label setHidden:YES];
    }
}
````
#9 cellforrowatindexpath在滚动时不加载图片，停止时加载图片，省内存
````swift
if self.tableView.dragging == false && self.tableView.decelerating == false{
  cell.imageView.sd_setImage()
}
````
#10 iOS7以后UITextView的那些坑   
坑1：
iOS7及以上的版本上，UITextView出现这样的问题：弹出键盘时，没输入任何文字，但是光标位置不是在最上方。

解决方案: iOS7以后新增了一个属性automaticallyAdjustsScrollViewInsets，将其置为NO即可。别忘了加版本判断。
````swift
if (IOS7_And_Later) {  
        self.automaticallyAdjustsScrollViewInsets = NO;  
    }  
````
坑2:
iOS7及以上的版本上，UITextView输入中文时，在输入多行后，光标有时会上下跳动，输入文字的时候内容有时会往上跳，光标都显示不出来。这个bug坑死好多人啊，特别是做聊天输入框的时候，不得已找一些非正常的方法解决。从网上找的各种解决方案都不行。

解决方案：
````swift
- (void)textViewDidChangeSelection:(UITextView *)textView  
{  
 
    //fix ios7 bug (modified by 老岳).  
    if (floor(NSFoundationVersionNumber) > NSFoundationVersionNumber_iOS_6_1) {  
        CGRect r = [textView caretRectForPosition:textView.selectedTextRange.end];  
        CGFloat caretY =  MAX(r.origin.y - textView.frame.size.height + r.size.height + 8, 0);  
        if (textView.contentOffset.y < caretY && r.origin.y != INFINITY) {  
            textView.contentOffset = CGPointMake(0, caretY);  
        }  
    }  
}  
````

#11 UIButton设置圆角
````swift
UIButton *roundBtn = [UIButton buttonWithType:UIButtonTypeCustom];

roundBtn.frame = CGRectMake(100, 100, 50, 50);

roundBtn.backgroundColor = [UIColor redColor

//设置cornerRadius

_roundBtn.layer.cornerRadius = 10.0；
在IB中，当你使用Custom类型的Button时，你可以指定按钮的背景色。但当你运行时按钮就失去了圆角特性，你看到的仅仅是一个方块。因为custombutton没有定义任何属性默认值。你必须自己去定义它们，这就需要使用Core Animation Layer。
提示：编写代码之前，需要导入QuartzCore框架到工程中，然后#import<QuartzCore/QuartzCore.h>。我会通常会把它放在.pch文件中。
IB没有干的事情，你只能通过代码来做。例如，如果你想做一个圆角且红色背景的按钮，你需要将按钮链接到你的viewcontroller的出口中，然后在Xcode中通过它的layer属性修改按钮的下列属性。
 [[button layer] setCornerRadius:8.0f];
[[button layer] setMasksToBounds:YES];
[[button layer] setBorderWidth:1.0f];
上述代码将layer的圆角半径设为8.0，-setMasksToBounds:方法告诉layer将位于它之下的layer都遮盖住。这是必须的，这样会使圆角不被遮，但是这样会导致阴影效果没有，很多网上都给出资料，再添加一个SubLayer，添加阴影。（意思也就是讲mask作为bound的边界，对原来的frame进行裁剪？？）
 
正确的解释：
Because shadow is an effect done outside the View, and that masksToBounds set to YES will tell the UIView not to draw everything that is outside itself.
 
最后，将border设为1.0，将显示出按钮的边框。默认边框色为黑色，你可以用-setBorderColor:方法修改为其他颜色，参数使用CGColorRef类型（例如[[UIColorgreenColor]CGColor]会显示绿色边框）。
 
两个view设置阴影的效果：注意是view1添加到view2，不要反了
UIView *view1 = [[UIView alloc] init]; 
UIView *view2 = [[UIView alloc] init];   
view1.layer.cornerRadius = 5.0; 
view1.layer.masksToBounds = YES; 
view2.layer.cornerRadius = 5.0; 
view2.layer.shadowColor = [[UIColor blackColor] CGColor]; 
view2.layer.shadowOpacity = 1.0; 
view2.layer.shadowRadius = 10.0; 
view2.layer.shadowOffset = CGSizeMake(0.0f, 0.0f); 
[view2 addSubview:view1]; 
[view1 release]; 


http://www.2cto.com/kf/201112/112730.html
http://nachbaur.com/blog/fun-shadow-effects-using-custom-calayer-shadowpaths
````
