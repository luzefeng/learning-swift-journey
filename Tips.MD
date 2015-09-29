1. 引用了cocoapods，achieve出other items
  答：选中pods project，选择build setting中的Deployment，把skip install改为Yes，如果还是不行，选中不行的target，build phases，把Headers中的h文件从public中移到Project中。
  
