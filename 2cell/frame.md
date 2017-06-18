## 给模型增加frame数据
- 所有子控件的frame
- cell的高度(cellHeight)

```objc
@interface XMGStatus : NSObject
/**** 文字\图片数据 ****/
// .....

/**** frame数据 ****/
/** 头像的frame */
@property (nonatomic, assign) CGRect iconFrame;
// .....
/** cell的高度 */
@property (nonatomic, assign) CGFloat cellHeight;
@end
```

- 重写模型cellHeight属性的get方法

```objc
- (CGFloat)cellHeight
{
    if (_cellHeight == 0) {
        // ... 计算所有子控件的frame、cell的高度
    }
    return _cellHeight;
}
```

## 在控制器中
- 实现一个返回cell高度的代理方法
    - 在这个方法中返回indexPath位置对应cell的高度

```objc
/**
 *  返回每一行cell的具体高度
 */
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatus *status = self.statuses[indexPath.row];
    return status.cellHeight;
}
```

- 给cell传递模型数据

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 访问缓存池
    XMGStatusCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    // 设置数据(传递模型数据)
    cell.status = self.statuses[indexPath.row];

    return cell;
}
```

## 新建一个继承自`UITableViewCell`的子类，比如XMGStatusCell
```objc
@interface XMGStatusCell : UITableViewCell
@end
```
## 在XMGStatusCell.m文件中
- 重写`-initWithStyle:reuseIdentifier:`方法
    - 在这个方法中添加所有可能需要显示的子控件
    - 给子控件做一些初始化设置（设置字体、文字颜色等）

```objc
/**
 *  在这个方法中添加所有可能需要显示的子控件
 */
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    if (self = [super initWithStyle:style reuseIdentifier:reuseIdentifier]) {
        // ......
    }
    return self;
}
```

## 在XMGStatusCell.h文件中提供一个模型属性，比如XMGStatus模型
```objc
@class XMGStatus;

@interface XMGStatusCell : UITableViewCell
/** 微博模型数据 */
@property (nonatomic, strong) XMGStatus *status;
@end
```

## 在XMGStatusCell.m中重写模型属性的set方法
- 在set方法中给子控件设置模型数据

```objc
- (void)setStatus:(XMGStatus *)status
{
    _status = status;

    // .......
}
```

## 重写`-layoutSubviews`方法
- 一定要调用`[super layoutSubviews]`
- 在这个方法中设置所有子控件的frame

```objc
/**
 *  在这个方法中设置所有子控件的frame
 */
- (void)layoutSubviews
{
    [super layoutSubviews];

    // ......
}
```
