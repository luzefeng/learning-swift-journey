##1 App crashes with reason: Collection <__NSArrayM: 0x7071700> was mutated while being enumerated
原因：是在一个数组序列化的时候对其进行修改导致的错误，我是在writeToFile的时候修改了数组

修改：在序列化的时候先深拷贝给一个临时数组，让临时数组进行序列化
```swift
    NSArray *tempArray = [self.mpDownloadArray mutableCopy];
    [tempArray writeToFile:filename atomically:NO];
```
