# WHC_Model
### WHC_Model 功能说明:
##### 1.支持json到模型对象
##### 2.支持模型对象到json的转换
##### 3.同时支持json和模型对象的无限嵌套
#### 4.高效的解析转换算法
#### 5.支持容错处理模型属性名称和json的key不区分大小写
#### 6.支持自定义模型类前缀

###咨询qq:712641411
###作者：吴海超


###这个配合WHC_DataModelFactory mac工具使用将让你体验超神的感觉：github地址：https://github.com/netyouli/WHC_DataModelFactory
###WHC_DataModelFactory mac工具说明:可以把json或者xml字符串自动生成模型类.m和.h文件内容
剩出创建模型类的麻烦而且零错误率大大提高了开发效率
###JSON --> Model、Core Data Model
###JSONString --> Model、Core Data Model
###Model、Core Data Model --> JSON
###JSON Array --> Model Array、Core Data Model Array
###JSONString --> Model Array、Core Data Model Array
###Model Array、Core Data Model Array --> JSON Array

```Objective-C
typedef enum {
    SexMale,
    SexFemale
} Sex;

@interface User : NSObject
@property (copy, nonatomic) NSString *name;
@property (copy, nonatomic) NSString *icon;
@property (strong, nonatomic) NSNumber * age;
@property (strong, nonatomic) NSNumber * height;
@property (strong, nonatomic) NSNumber *money;
@property (strong, nonatomic) Sex  *sex;
@end

/***********************************************/


NSDictionary *dict = @{
    @"name" : @"Jack",
    @"icon" : @"lufy.png",
    @"age" : @20,
    @"height" : @"1.55",
    @"money" : @100.9,
    @"sex" : @(SexFemale)
};
// JSON -> User
User * user = [User modelWithDictionary:dict];

NSLog(@"name=%@, icon=%@, age=%@, height=%@, money=%@, sex=%@",
      user.name, user.icon, user.age, user.height, user.money, user.sex);
// name=Jack, icon=lufy.png, age=20, height=1.550000, money=100.9, sex=1

// User -> json
NSLog(@"json = %@",[user json]);

```


###Model contains model

```Objective-C
@interface Status : NSObject
@property (copy, nonatomic) NSString *text;
@property (strong, nonatomic) User *user;
@property (strong, nonatomic) Status *status;
@end

/***********************************************/

NSDictionary *dict = @{
@"text" : @"Agree!Nice weather!",
@"user" : @{
@"name" : @"Jack",
@"icon" : @"lufy.png"
},
@"retweetedStatus" : @{
@"text" : @"Nice weather!",
@"user" : @{
@"name" : @"Rose",
@"icon" : @"nami.png"
}
}
};

// JSON -> Status
Status *status = [Status modelWithDictionary:dict];

// status -> json
NSLog(@"json = %@",[status json]);

NSString *text = status.text;
NSString *name = status.user.name;
NSString *icon = status.user.icon;
NSLog(@"text=%@, name=%@, icon=%@", text, name, icon);
// text=Agree!Nice weather!, name=Jack, icon=lufy.png

NSString *text2 = status.status.text;
NSString *name2 = status.status.user.name;
NSString *icon2 = status.status.user.icon;
NSLog(@"text2=%@, name2=%@, icon2=%@", text2, name2, icon2);
// text2=Nice weather!, name2=Rose, icon2=nami.png
// JSON -> User

```
###Model contains model-array

```Objective-C

@interface Ad : NSObject
@property (copy, nonatomic) NSString *image;
@property (copy, nonatomic) NSString *url;
@end

@interface StatusResult : NSObject
/** Contatins status model */
@property (strong, nonatomic) NSMutableArray *statuses;
/** Contatins ad model */
@property (strong, nonatomic) NSArray *ads;
@property (strong, nonatomic) NSNumber *totalNumber;
@end

/***********************************************/

// Tell MJExtension what type model will be contained in statuses and ads.
[StatusResult setupObjectClassInArray:^NSDictionary *{
return @{
@"statuses" : @"Status",
@"ads" : @"Ad"
};
}];
// Equals: StatusResult.m implements +objectClassInArray method.

NSDictionary *dict = @{
@"statuses" : @[
@{
@"text" : @"Nice weather!",
@"user" : @{
@"name" : @"Rose",
@"icon" : @"nami.png"
}
},
@{
@"text" : @"Go camping tomorrow!",
@"user" : @{
@"name" : @"Jack",
@"icon" : @"lufy.png"
}
}
],
@"ads" : @[
@{
@"image" : @"ad01.png",
@"url" : @"http://www.ad01.com"
},
@{
@"image" : @"ad02.png",
@"url" : @"http://www.ad02.com"
}
],
@"totalNumber" : @"2014"
};

// JSON -> StatusResult
StatusResult *result = [StatusResult modelWithDictionary:dict];

NSLog(@"totalNumber=%@", result.totalNumber);

// Printing
for (Status *status in result.statuses) {
    NSString *text = status.text;
    NSString *name = status.user.name;
    NSString *icon = status.user.icon;
    NSLog(@"text=%@, name=%@, icon=%@", text, name, icon);
}
// text=Nice weather!, name=Rose, icon=nami.png
// text=Go camping tomorrow!, name=Jack, icon=lufy.png
// Printing
for (Ad *ad in result.ads) {
NSLog(@"image=%@, url=%@", ad.image, ad.url);
}
// image=ad01.png, url=http://www.ad01.com
// image=ad02.png, url=http://www.ad02.com
```
