# Queen SDK

[![Version](https://img.shields.io/cocoapods/v/Queen.svg?style=flat)](https://cocoapods.org/pods/Queen)
[![License](https://img.shields.io/cocoapods/l/Queen.svg?style=flat)](https://cocoapods.org/pods/Queen)
[![Platform](https://img.shields.io/cocoapods/p/Queen.svg?style=flat)](https://cocoapods.org/pods/Queen)

## 简介
- 本文为您介绍iOS端[阿里云Queen SDK](https://help.aliyun.com/document_detail/211047.html?spm=a2c4g.11186623.6.735.6a1b192eB31nYi)的接口文档、集成操作及简单使用示例，用于实现美颜特效功能。

### 设备和系统版本

- iOS设备：armv7或arm64的移动设备（iPad/iPhone，不包含支持arm64架构的Mac）
- iOS系统版本：iOS 9.0及以上

## FAQ
### 常见问题FAQ，参见：[FAQ](https://github.com/aliyunvideo/Queen_SDK_Android/blob/main/FAQ.md "Queen使用FAQ")

## 集成Queen SDK

支持pods与本地集成两种方式。

### full版本：
#### pods集成方式：
```ruby
pod 'Queen', '1.4.0-official-full'
```
#### 本地集成方式：

1. 下载并解压Sample示例工程，获取以下framework文件:
```
queen.framework
FaceDetection.framework
opencv2.framework
MNN.framework
bokeh_ios.framework
```
2. 打开Xcode，在工程target的General页签下，在Frameworks, Libraries, and Embedded Content区域中添加以上framework。其中，opencv2.framework和bokeh_ios.framework的Embed属性设置成Embed & Sign，其他framework的Embed属性设置成Do Not Embed。
3. 在Frameworks, Libraries, and Embedded Content区域中添加以下系统依赖。
```
libz.tbd
libc++.tbd
libcompression.tbd
Metal.framework
MetalPerformanceShaders.framework
Accelerate.framework
QuartzCore.framework
OpenGLES.framework
CoreMedia.framework
CoreMotion.framework
CoreImage.framework
Foundation.framework
CoreGraphics.framework
CoreVideo.framework
```
4. 将获取到的queen.framework文件中的queen-ios.Bundle添加到工程目录中。
5. 在工程target的Build Settings页签下，搜索找到ENABLE_BITCODE一项，将其设置成NO。

### pro版本：
#### pods集成方式：
```ruby
pod 'Queen', '1.4.0-official-pro'
```
#### 本地集成方式：

1. 下载并解压Sample示例工程，获取以下framework文件:
```
queen.framework
FaceDetection.framework
opencv2.framework
MNN.framework
```
2. 打开Xcode，在工程target的General页签下，在Frameworks, Libraries, and Embedded Content区域中添加以上framework。其中，opencv2.framework的Embed属性设置成Embed & Sign，其他framework的Embed属性设置成Do Not Embed。
3. 在Frameworks, Libraries, and Embedded Content区域中添加以下系统依赖。
```
libz.tbd
libc++.tbd
libcompression.tbd
Metal.framework
MetalPerformanceShaders.framework
Accelerate.framework
QuartzCore.framework
OpenGLES.framework
CoreMedia.framework
CoreMotion.framework
CoreImage.framework
Foundation.framework
CoreGraphics.framework
CoreVideo.framework
```
4. 将获取到的queen.framework文件中的queen-ios.Bundle添加到工程目录中。
5. 在工程target的Build Settings页签下，搜索找到ENABLE_BITCODE一项，将其设置成NO。

### lite版本：
#### pods集成方式：
```ruby
pod 'Queen', '1.4.1-official-lite'
```
#### 本地集成方式：

1. 下载并解压Sample示例工程，获取以下framework文件:
```
queen.framework
```
2. 打开Xcode，在工程target的General页签下，在Frameworks, Libraries, and Embedded Content区域中添加以上framework。将以上添加的framework的Embed属性设置成Do Not Embed。
3. 在Frameworks, Libraries, and Embedded Content区域中添加以下系统依赖。
```
libz.tbd
libc++.tbd
libcompression.tbd
Accelerate.framework
QuartzCore.framework
OpenGLES.framework
CoreMedia.framework
CoreMotion.framework
CoreImage.framework
Foundation.framework
CoreGraphics.framework
CoreVideo.framework
```
4. 将获取到的queen.framework文件中的queen-ios.Bundle添加到工程目录中。
5. 在工程target的Build Settings页签下，搜索找到ENABLE_BITCODE一项，将其设置成NO。

## 使用示例

```objc
- (void)initBeautyEngine
{
    // 初始化引擎配置信息对象
    QueenEngineConfigInfo *configInfo = [QueenEngineConfigInfo new];
    
    // 设置是否自动设置图片旋转角度，如设备锁屏，并且默认图像采集来自摄像头的话可以设置自动设置图片旋转角度
#if kEnableCustomSettingImgAngle
    configInfo.autoSettingImgAngle = NO;
#else
    configInfo.autoSettingImgAngle = YES;
#endif
    
    // 设置资源根目录
    NSString *bundlPath = [[NSBundle mainBundle] bundlePath];
    configInfo.resRootPath = [bundlPath stringByAppendingString:@"/res"];
    
    // 引擎初始化
    self.beautyEngine = [[QueenEngine alloc] initWithConfigInfo:configInfo];
    
    [self testBaseFaceBeauty];
    [self testAdvancedFaceBeauty];
    [self testFaceMakeup];
    [self testFaceShape];
    [self testFilter];
    [self testSticker];
    [self testGreenScreenOrBlueScreenCutout];
    [self testBackgroundCutout];
//    [self testDebug];
}

- (void)testBaseFaceBeauty
{
    // 打开磨皮锐化功能开关
    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeSkinBuffing enable:YES];
    
    // 设置磨皮系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsSkinBuffing value:0.5f];
    // 设置锐化系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsSharpen value:0.5f];
    
    // 打开美白功能开关
    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeSkinWhiting enable:YES];
    
    // 设置美白系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsWhitening value:0.5f];
}

- (void)testAdvancedFaceBeauty
{
    // 打开高级美颜功能开关
    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeFaceBuffing enable:YES];
    
    // 设置去眼袋系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsPouch value:0.5f];
    // 设置去法令纹系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsNasolabialFolds value:0.5f];
    // 设置白牙系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsWhiteTeeth value:0.5f];
    // 设置口红系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsLipstick value:0.5f];
    // 设置腮红系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsBlush value:0.5f];
    // 设置口红色相系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsLipstickColorParam value:0.1f];
    // 设置口红饱和度系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsLipstickGlossParam value:0.5f];
    // 设置口红明度系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsLipstickBrightnessParam value:0.5f];
    // 设置亮眼系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsBrightenEye value:0.5f];
    // 设置红润系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsSkinRed value:0.5f];
    // 设置去皱纹系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsWrinkles value:0.2f];
    // 设置去暗沉系数
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsBrightenFace value:0.2f];
}

- (void)testFaceMakeup
{
    // 打开美妆功能开关
    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeMakeup enable:YES];

    BOOL makeupWhole = true;

    if (makeupWhole)
    {
        // 设置美妆整妆效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeWhole paths:@[@"makeup/jichu.png"] blendType:kQueenBeautyBlendLabMix];
        // 设置美妆整妆效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeWhole female:YES alpha:1.0];
    }
    else
    {
        // 设置美妆局部妆效果：（注：设置局部妆后，如果之前设置了整妆，整妆会失效。即整妆和局部妆不能共存，但每个局部妆之间可以叠加使用，而整妆设置单个素材即可实现全脸上妆，但是无法调节各部位细节）
        // 设置美妆高光效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeHighlight paths:@[@"makeup/highlight/highlight.2.12.png"] blendType:kQueenBeautyBlendOverlay];
        // 设置美妆高光效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeHighlight female:YES alpha:0.4];
        // 设置美妆美瞳效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeEyeball paths:@[@"makeup/eyeball/milanda.2.1.png"] blendType:kQueenBeautyBlendLighten];
        // 设置美妆美瞳效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeEyeball female:YES alpha:1.0];
        // 设置美妆口红效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeMouth paths:@[@"makeup/mouth_yaochun/doushafen.2.31.png"] blendType:kQueenBeautyBlendLabMix];
        // 设置美妆口红效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeMouth female:YES alpha:0.5];
        // 设置美妆卧蚕效果，目前采用内置素材，不支持定制
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeWocan paths:@[@"makeup/wocan.png"] blendType:kQueenBeautyBlendCurve];
        // 设置美妆卧蚕效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeWocan female:YES alpha:0.2];
        // 设置美妆眉毛效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeEyeBrow paths:@[@"makeup/eyebrow/biaozhunmei.2.31.png"] blendType:kQueenBeautyBlendLabMix];
        // 设置美妆眉毛效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeEyeBrow female:YES alpha:1.0];
        // 设置美妆腮红效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeBlush paths:@[@"makeup/blush/shaonv.2.31.png"] blendType:kQueenBeautyBlendLabMix];
        // 设置美妆腮红效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeBlush female:YES alpha:1.0];
        // 设置美妆眼影效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeEyeShadow paths:@[@"makeup/eyeshadow/shaonvfen.2.31.png"] blendType:kQueenBeautyBlendLabMix];
        // 设置美妆眼影效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeEyeShadow female:YES alpha:0.8];
        // 设置美妆眼线效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeEyeliner paths:@[@"makeup/eyeliner_5B443E/guima.2.31.png"] blendType:kQueenBeautyBlendLabMix];
        // 设置美妆眼线效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeEyeliner female:YES alpha:0.6];
        // 设置美妆睫毛效果，资源路径也可以是资源的绝对路径
        [self.beautyEngine setMakeupWithType:kQueenBeautyMakeupTypeEyelash paths:@[@"makeup/eyelash/yesheng.2.31.png"] blendType:kQueenBeautyBlendLabMix];
        // 设置美妆睫毛效果的透明度，目前female参数的值统一传YES/true，男性妆容还在优化中
        [self.beautyEngine setMakeupAlphaWithType:kQueenBeautyMakeupTypeEyelash female:YES alpha:0.6];
    }

//    // 清除美妆效果
//    [self.beautyEngine resetAllMakeupType];

//    // 关闭美妆功能开关
//    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeMakeup enable:NO];

    /*
    建议采用组合妆来替换整妆的效果，可以调节各部分细节，下面提供几种组合妆的模式：
    1、糖果妆：高光（makeup/highlight/highlight.2.12.png, 透明度：0.4）、口红（makeup/mouth_yaochun/doushafen.2.31.png 透明度：0.5）、腮红（makeup/blush/shaonv.2.31.png 透明度：1.0）、眼影（makeup/eyeshadow/shaonvfen.2.31.png 透明度：0.8）、眼线（makeup/eyeliner_5B443E/guima.2.31.png 透明度：0.6）、睫毛（makeup/eyelash/yesheng.2.31.png 透明度：0.6）
    2、夜店妆：高光（makeup/highlight/highlight.2.12.png, 透明度：0.4）、口红（makeup/mouth_wumian/naiyoucheng.2.31.png 透明度：0.5）、腮红（makeup/blush/chulian.2.31.png 透明度：1.0）、眼影（makeup/eyeshadow/jiaotangzong.2.31.png 透明度：0.8）、眼线（makeup/eyeliner_5B443E/xiaoyemao.2.31.png 透明度：0.6）、睫毛（makeup/eyelash/huopo.2.31.png 透明度：0.6）
    3、复古妆：高光（makeup/highlight/highlight.2.12.png, 透明度：0.4）、口红（makeup/mouth_zirun/ningxiangzi.2.31.png 透明度：0.5）、腮红（makeup/blush/suyan.2.31.png 透明度：1.0）、眼影（makeup/eyeshadow/zhuanhongse.2.31.png 透明度：0.8）、眼线（makeup/eyeliner_5B443E/lingdong.2.31.png 透明度：0.6）、睫毛（makeup/eyelash/ziran.2.31.png 透明度：0.6）
    4、桃花妆：高光（makeup/highlight/highlight.2.12.png, 透明度：0.4）、口红（makeup/mouth_wumian/naiyoucheng.2.31.png 透明度：0.5）、腮红（makeup/blush/shaonv.2.31.png  透明度：1.0）、眼影（makeup/eyeshadow/ziranse.2.31.png 透明度：0.8）、眼线（makeup/eyeliner_5B443E/wenrou.2.31.png 透明度：0.6）、睫毛（makeup/eyelash/wugu.2.31.png 透明度：0.6）
    */
}

- (void)testFaceShape
{
    // 打开美型功能开关
    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeFaceShape enable:YES];
    
    // 设置大眼系数
    [self.beautyEngine setFaceShape:kQueenBeautyFaceShapeTypeBigEye value:1.0f];
    // 设置发际线系数
    [self.beautyEngine setFaceShape:kQueenBeautyFaceShapeTypeHairLine value:1.0f];
    // 设置嘴角上扬(微笑)系数
    [self.beautyEngine setFaceShape:kQueenBeautyFaceShapeTypeSmile value:1.0f];
}

- (void)testFilter
{
    // 打开滤镜功能开关
    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeLUT enable:YES];
    
    // 设置滤镜资源，也可以是资源的绝对路径
    [self.beautyEngine setLutImagePath:@"lookups/ly1.png"];
    // 设置滤镜强度
    [self.beautyEngine setQueenBeautyParams:kQueenBeautyParamsLUT value:0.8f];
}

- (void)testSticker
{
    // 添加贴纸，也可以是资源的绝对路径
    [self.beautyEngine addMaterialWithPath:@"sticker/1"];
//    // 添加贴纸，贴纸图层从下往上叠加
//    [self.beautyEngine addMaterialWithPath:@"sticker/2"];
//    // 删除贴纸
//    [self.beautyEngine removeMaterialWithPath:@"sticker/1"];
//    [self.beautyEngine removeMaterialWithPath:@"sticker/2"];
}

- (void)testGreenScreenOrBlueScreenCutout
{
    // 开启绿幕抠图功能
    NSString *backgroundImgPath = @"background/red.png";//也可以是资源的绝对路径
    BOOL enableBlue = NO;
    float threshold = 0;
    BOOL autoThreshold = YES;
    [self.beautyEngine setGreenScreen:backgroundImgPath blueScreenEnabled:enableBlue threshold:threshold autoThresholdEnabled:autoThreshold];
    
//    // 开启蓝幕抠图功能
//    enableBlue = YES;
//    [self.beautyEngine setGreenScreen:backgroundImgPath blueScreenEnabled:enableBlue threshold:threshold autoThresholdEnabled:autoThreshold];
    
//    // 取消幕布抠图功能
//    [self.beautyEngine setGreenScreen:nil blueScreenEnabled:enableBlue threshold:threshold autoThresholdEnabled:autoThreshold];
}

- (void)testBackgroundCutout
{
//    // 人像背景虚化开启
//    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeBackgroundProcess enable:YES];
//    // 人像背景虚化关闭
//    [self.beautyEngine setQueenBeautyType:kQueenBeautyTypeBackgroundProcess enable:NO];
//
    NSString *backgroundResPath = @"background/static_changlang";//也可以是资源的绝对路径
    // 替换人像背景为静态图，相同资源不能重复添加
    [self.beautyEngine addMaterialWithPath:backgroundResPath];
//    // 取消人像背景设置为静态图
//    [self.beautyEngine removeMaterialWithPath:backgroundResPath];
}

- (void)testDebug
{
    // 展示人脸识别特征点
    [self.beautyEngine showFaceDetectPoint:YES];
    // 展示美妆三角剖分信息, 需要先开启美妆功能
    [self.beautyEngine showMakeupLine:YES];
}

- (CVPixelBufferRef)getProcessedPixelBufferRefWithCurrentPixelBufferRef:(CVPixelBufferRef)pixelBufferRef
{
    if (self.beautyEngine && pixelBufferRef)
    {
        QEPixelBufferData *bufferData = [QEPixelBufferData new];
        bufferData.bufferIn = pixelBufferRef;
        bufferData.bufferOut = pixelBufferRef;
#if kEnableCustomSettingImgAngle
        bufferData.inputAngle = self.cameraRotate; //要正确传入pixelBufferRef的方向，否则人脸识别会失败，如果不知道pixelBufferRef的方向，可参考此demo属性cameraRotate取值的方法
        bufferData.outputAngle = self.cameraRotate; //一般和inputAngle取值一样就可以了
#endif
        // 对pixelBuffer进行图像处理，输出处理后的buffer
        kQueenResultCode resultCode = [self.beautyEngine processPixelBuffer:bufferData];//执行此方法的线程需要始终是同一条线程
        if (resultCode == kQueenResultCodeOK && bufferData.bufferOut)
        {
            return bufferData.bufferOut;
        }
        else if (resultCode == kQueenResultCodeInvalidLicense)
        {
            NSLog(@"license校验失败。");
        }
        else if (resultCode == kQueenResultCodeInvalidParam)
        {
            NSLog(@"非法参数");
        }
        else if (resultCode == kQueenResultCodeNoEffect)
        {
            NSLog(@"没有开启任何特效");
        }
        return pixelBufferRef;
    }
    else
    {
        return pixelBufferRef;
    }
}

- (void)captureReset
{
    if (self.beautyEngine)
    {
        //释放queen，确保当前线程与执行processPixelBuffer:是同一条线程
        [self.beautyEngine destroyEngine];
        self.beautyEngine = nil;
    }
}
```

## License

Queen is available under the Apache License, Version 2.0. See the LICENSE file for more info.
