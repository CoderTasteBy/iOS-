## 新建一个继承自`UITableViewCell`的子类，比如XMGTgCell
```objc
@interface XMGTgCell : UITableViewCell
@end
```
## 在XMGTgCell.m文件中
- 重写`-initWithStyle:reuseIdentifier:`方法
    - 在这个方法中添加所有需要显示的子控件
    - 给子控件做一些初始化设置（设置字体、文字颜色等）
    - `添加子控件的完整约束`

```objc
/**
 *  在这个方法中添加所有的子控件
 */
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    if (self = [super initWithStyle:style reuseIdentifier:reuseIdentifier]) {
        // ......
    }
    return self;
}
```

## 在XMGTgCell.h文件中提供一个模型属性，比如XMGTg模型
```objc
@class XMGTg;

@interface XMGTgCell : UITableViewCell
/** 团购模型数据 */
@property (nonatomic, strong) XMGTg *tg;
@end
```

## 在XMGTgCell.m中重写模型属性的set方法
- 在set方法中给子控件设置模型数据

```objc
- (void)setTg:(XMGTg *)tg
{
    _tg = tg;

    // .......
}
```

## 在控制器中
- 注册cell的类型

```objc
[self.tableView registerClass:[XMGTgCell class] forCellReuseIdentifier:ID];
```

- 给cell传递模型数据

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 访问缓存池
    XMGTgCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    // 设置数据(传递模型数据)
    cell.tg = self.tgs[indexPath.row];

    return cell;
}
```
