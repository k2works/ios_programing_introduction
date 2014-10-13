iOSプログラミング入門
===
# 目的
# 前提
| ソフトウェア     | バージョン    | 備考         |
|:---------------|:-------------|:------------|
| OS X           |10.8.5        |             |
| Xcode     　　　|6.0.1        |             |
| Object-C       |              |             |

# 構成
+ [CocoaPods付きプロジェクトの導入](#1)
+ [TopControllerの作成](#2)
+ [DCIntrospectのセットアップ](#3)
+ [CocoaPodsの推奨ライブラリ](#4)

# 詳細
## <a name="1">CocoaPods付きプロジェクトの導入</a>
### ストーリーボードなしのEmpty Applicationを作成する
1. XCode6でSingle View Applicationを作る
1. Main.stroyboardとLanchScreen.xibを削除する
1. Info.plistファイル内エントリーの"Main storyboard file base name"と "Launch screen interface file base name"を削除する。
1. AppDelegate.mファイルを開いて以下の用に編集する

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    // Override point for customization after application launch.
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    return YES;
}
```

### DCIntrospectをインストールする
_HelloWorld/Podfile_

```
platform :ios ,'6.0'
pod 'DCIntrospect'
```

```bash
$ cd HelloWorld/
$ pod install
$ open HelloWorld.xcworkspace
```

## <a name="2">TopControllerの作成</a>
UiViewControllerのサブクラスとなるTopViewControllerを作る

_HelloWorld/HelloWorld/TopViewController.h_  
_HelloWorld/HelloWorld/TopViewController.m_
### TopViewControllerのインスタンス表示
_HelloWorld/HelloWorld/AppDelegate.h_

```
#import <UIKit/UIKit.h>
#import "TopViewController.h"

@interface AppDelegate : UIResponder <UIApplicationDelegate>

@property (strong, nonatomic) UIWindow *window;
@property (strong, nonatomic) TopViewController *topViewController;


@end
```
_HelloWorld/HelloWorld/AppDelegate.m_

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    _topViewController = [[TopViewController alloc]init];
    self.window.rootViewController = _topViewController;
    [self.window makeKeyAndVisible];
    return YES;
}
```

### TopViewControllerの背景色変更とボタン表示
_HelloWorld/HelloWorld/TopViewController.m_

```
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    [self.view setBackgroundColor:[UIColor whiteColor]];
    UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    [button setFrame: CGRectMake(10, 10, 100, 30)];
    [button setTitle:@"Hello World" forState:UIControlStateNormal];

    [self.view addSubview:button];
}
```

## <a name="3">DCIntrospectのセットアップ</a>
### QuartzCore frameworkの追加
![001](https://farm4.staticflickr.com/3950/15338971647_b127852ca6.jpg)
### HelloWorld-Prefix.pchの追加
_HelloWorld/HelloWorld/HelloWorld-Prefix.pch_
```
#import <UIKit/UIKit.h>
#import <Foundation/Foundation.h>
#import "DCIntrospect.h"
```
### AppDelegate.mの修正
_HelloWorld/HelloWorld/AppDelegate.m_

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    _topViewController = [[TopViewController alloc]init];
    self.window.rootViewController = _topViewController;
    [self.window makeKeyAndVisible];
#if TARGET_IPHONE_SIMULATOR
    [[DCIntrospect sharedIntrospector]start];
#endif
    return YES;
}
```
## <a name="4">CocoaPodsの推奨ライブラリ</a>
* [Reachability](https://github.com/tonymillion/Reachability)
* [AFNetworking](https://github.com/AFNetworking/AFNetworking)
* [SVProgressHUD](https://github.com/TransitApp/SVProgressHUD)
* [BlocksKit](https://github.com/zwaldowski/BlocksKit)
* [mogenerator](https://github.com/rentzsch/mogenerator)
* [MagicalRecord](https://github.com/magicalpanda/MagicalRecord)
* [SDWebImage](https://github.com/rs/SDWebImage)
* [Underscore.m](https://github.com/robb/Underscore.m)

# 参照

* [Xcode6でもストーリーボードなしのEmpty Applicationから始めたい](http://appstars.jp/archive/243)
* [Xcode6でViewControllerクラスを追加するメモ](http://qiita.com/jollyjoester/items/2129bb59ca8e18b6f11c)
* [Xcode6 PCHファイルがないので追加する](http://appstars.jp/archive/244)
