# 攻克 12306 前端加密算法

## 效果预览

```json
{
  "key": "&FMQw=0&q4f3=zh-CN&VPIf=1&custID=133&VEek=unknown&dzuS=0&yD16=0&EOQP=c227b88b01f5c513710d4b9f16a5ce52&jp76=52d67b2a5aa5e031084733d5006cc664&hAqN=MacIntel&platform=WEB&ks0Q=d22ca0b81584fbea62237b14bd04c866&TeRS=777x1280&tOHY=24xx800x1280&Fvje=i1l1o1s1&q5aJ=-8&wNLf=99115dfb07133750ba677d055874de87&0aew=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.87 Safari/537.36&E3gR=9f7fa43e794048f6193187756181b3b9",
  "value": "owRJc8M4EkFMvcTkzibRFJoDSkUKCx6N9ictZIJLIeY"
}
```

## 算法分析

### step 1 : 获取浏览器基本信息

```js
chromeHelper.prototype = {
    /**
     * 获取浏览器基本信息,来源于getDfpMoreInfo中的b.cfp.get和aa的get
     */
    getBasicInfoArr: function() {
      // 基本信息,若数据无效则返回 void 0,即 undefined
      var basicInfoArr = [];

      // 用户代理 
      basicInfoArr.push(this.getUserAgentKeyAndValue(1));
      // 语言
      basicInfoArr.push(this.getLanguageKeyAndValue(1));
      // 颜色深度 
      basicInfoArr.push(this.getColorDepthKeyAndValue(1));
      // 像素比例
      basicInfoArr.push(this.getPixelRatioKeyAndValue(1));
      // 屏幕分辨率
      basicInfoArr.push(this.getScreenResolutionKeyAndValue(1));
      // 可用屏幕分辨率
      basicInfoArr.push(this.getAvailableScreenResolutionKeyAndValue(1));
      // 时区偏移量
      basicInfoArr.push(this.getTimezoneOffsetKeyAndValue(1));
      // Session存储
      basicInfoArr.push(this.getSessionStorageKeyAndValue(1));
      // Local存储
      basicInfoArr.push(this.getLocalStorageKeyAndValue(1));
      // IndexedDb存储
      basicInfoArr.push(this.getIndexedDbKeyAndValue(1));
      // websql存储
      basicInfoArr.push(this.getOpenDatabaseKeyAndValue(1));
      // cpu类型
      basicInfoArr.push(this.getCpuClassKeyAndValue(1));
      // 平台类型
      basicInfoArr.push(this.getPlatformKeyAndValue(1));
      // 反追踪
      basicInfoArr.push(this.getDoNotTrackKeyAndValue(1));
      // 插件
      basicInfoArr.push(this.getPluginsKeyAndValue(1));
      // TODO 画布
      basicInfoArr.push(this.getCanvasKeyAndValue(0));
      // webgl画布
      basicInfoArr.push(this.getWebglKeyAndValue(1));
      // adBlock广告拦截
      basicInfoArr.push(this.getAdBlockKeyAndValue(1));
      // 说谎语言
      basicInfoArr.push(this.getHasLiedLanguagesKeyAndValue(1));
      // 说谎分辨率
      basicInfoArr.push(this.getHasLiedResolutionKeyAndValue(1));
      // 说谎操作系统
      basicInfoArr.push(this.getHasLiedOsKeyAndValue(1));
      // 说谎浏览器
      basicInfoArr.push(this.getHasLiedBrowserKeyAndValue(1));
      // 触摸支持
      basicInfoArr.push(this.getTouchSupportKeyAndValue(1));
      // 字体
      basicInfoArr.push(this.getFontsKeyAndValue(1));

      return basicInfoArr;
    }
}
```

-  用户代理

```js
chromeHelper.prototype = {
    /**
     * 获取用户代理键值对
     */
    getUserAgentKeyAndValue: function(trueOrFake) {
      return {
        "key": "user_agent",
        "value": this.getUserAgent(trueOrFake)
      }
    },
    /**
     * 获取用户代理,注意并不是原始userAgent而是去掉了&+?%#/=等特殊字符,例如: "Mozilla5.0 (Macintosh; Intel Mac OS X 10_15_2) AppleWebKit537.36 (KHTML, like Gecko) Chrome80.0.3987.87 Safari537.36"
     */
    getUserAgent: function(trueOrFake) {
      if (trueOrFake) {
        return navigator.userAgent.replace(/\&|\+|\?|\%|\#|\/|\=/g, "")
      }
      return "Mozilla5.0 (Macintosh; Intel Mac OS X 10_15_2) AppleWebKit537.36 (KHTML, like Gecko) Chrome80.0.3987.87 Safari537.36"
    }
}
```

- 语言

```js
chromeHelper.prototype = {
    /**
     * 获取语言类型键值对
     */
    getLanguageKeyAndValue: function(trueOrFake) {
      return {
        "key": "language",
        "value": this.getLanguage(trueOrFake)
      }
    },
    /**
     * 获取语言类型,例如: "zh-CN"
     */
    getLanguage: function(trueOrFake) {
      if (trueOrFake) {
        return navigator.language
      }
      return "zh-CN"
    }
}
```

- 颜色深度 

```js
chromeHelper.prototype = {
    /**
     * 获取颜色深度键值对
     */
    getColorDepthKeyAndValue: function(trueOrFake) {
      return {
        "key": "color_depth",
        "value": this.getColorDepth(trueOrFake)
      }
    },
    /**
     * 获取颜色深度,例如: 24
     */
    getColorDepth: function(trueOrFake) {
      if (trueOrFake) {
        return screen.colorDepth
      }
      return 24
    }
}
```

- 像素比例

```js
chromeHelper.prototype = {
    /**
     * 获取像素比例键值对
     */
    getPixelRatioKeyAndValue: function(trueOrFake) {
      return {
        "key": "pixel_ratio",
        "value": this.getPixelRatio(trueOrFake)
      }
    },
    /**
     * 获取像素比例,例如: 2
     */
    getPixelRatio: function(trueOrFake) {
      if (trueOrFake) {
        return window.devicePixelRatio
      }
      return 2
    }
}
```

- 屏幕分辨率

```js
chromeHelper.prototype = {
    /**
     * 获取屏幕分辨率键值对
     */
    getScreenResolutionKeyAndValue: function(trueOrFake) {
      return {
        "key": "resolution",
        "value": this.getScreenResolution(trueOrFake)
      }
    },
    /**
     * 获取屏幕分辨率,例如: [1280, 800]
     */
    getScreenResolution: function(trueOrFake) {
      if (trueOrFake) {
        return [screen.width, screen.height]
      }
      return [1280, 800]
    }
}
```

- 可用屏幕分辨率

```js
chromeHelper.prototype = {
    /**
     * 获取可用屏幕分辨率键值对
     */
    getAvailableScreenResolutionKeyAndValue: function(trueOrFake) {
      return {
        "key": "available_resolution",
        "value": this.getAvailableScreenResolution(trueOrFake)
      }
    },
    /**
     * 获取可用屏幕分辨率,例如: [1280, 777]
     */
    getAvailableScreenResolution: function(trueOrFake) {
      if (trueOrFake) {
        return [screen.availWidth, screen.availHeight]
      }
      return [1280, 777]
    }
}
```

- 时区偏移量

```js
chromeHelper.prototype = {
    /**
     * 获取时区偏移量键值对
     */
    getTimezoneOffsetKeyAndValue: function(trueOrFake) {
      return {
        "key": "timezone_offset",
        "value": this.getTimezoneOffset(trueOrFake)
      }
    },
    /**
     * 获取时区偏移量,例如: -480
     */
    getTimezoneOffset: function(trueOrFake) {
      if (trueOrFake) {
        return (new Date).getTimezoneOffset()
      }
      return -480
    }
}
```

- Session存储

```js
chromeHelper.prototype = {
    /**
     * 获取Session存储键值对
     */
    getSessionStorageKeyAndValue: function(trueOrFake) {
      return {
        "key": "session_storage",
        "value": this.getSessionStorage(trueOrFake)
      }
    },
    /**
     * 获取Session存储,例如: 1
     */
    getSessionStorage: function(trueOrFake) {
      if (trueOrFake) {
        return !!window.sessionStorage ? 1 : 0;
      }
      return void 0;
    }
}
```

- Local存储

```js
chromeHelper.prototype = {
    /**
     * 获取Local存储键值对
     */
    getLocalStorageKeyAndValue: function(trueOrFake) {
      return {
        "key": "local_storage",
        "value": this.getLocalStorage(trueOrFake)
      }
    },
    /**
     * 获取Local存储,例如: 1
     */
    getLocalStorage: function(trueOrFake) {
      if (trueOrFake) {
        return !!window.localStorage ? 1 : 0;
      }
      return void 0;
    }
}
```

- IndexedDb存储

```js
chromeHelper.prototype = {
    /**
     * 获取indexedDb存储键值对
     */
    getIndexedDbKeyAndValue: function(trueOrFake) {
      return {
        "key": "indexed_db",
        "value": this.getIndexedDb(trueOrFake)
      }
    },
    /**
     * 获取indexedDb存储,例如: 1
     */
    getIndexedDb: function(trueOrFake) {
      if (trueOrFake) {
        return !!window.indexedDB ? 1 : 0;
      }
      return void 0;
    }
}
```

- AddBehavior存储

```js
chromeHelper.prototype = {
    /**
     * 获取addBehavior存储键值对
     */
    getAddBehaviorKeyAndValue: function(trueOrFake) {
      return {
        "key": "add_behavior",
        "value": this.getAddBehavior(trueOrFake)
      }
    },
    /**
     * 获取addBehavior存储(Chrome 不支持!!!),例如: 0
     */
    getAddBehavior: function(trueOrFake) {
      if (trueOrFake) {
        return !!document.body.addBehavior ? 1 : 0;
      }
      return void 0
    }
}
```

- websql存储

```js
chromeHelper.prototype = {
    /**
     * 获取Websql存储键值对
     */
    getOpenDatabaseKeyAndValue: function(trueOrFake) {
      return {
        "key": "open_database",
        "value": this.getOpenDatabase(trueOrFake)
      }
    },
    /**
     * 获取Websql存储,例如: 1
     */
    getOpenDatabase: function(trueOrFake) {
      if (trueOrFake) {
        return !!window.openDatabase ? 1 : 0;
      }
      return void 0
    }
}
```

- cpu类型

```js
chromeHelper.prototype = {
    /**
     * 获取cpu类型键值对
     */
    getCpuClassKeyAndValue: function(trueOrFake) {
      return {
        "key": "cpu_class",
        "value": this.getCpuClass(trueOrFake)
      }
    },
    /**
     * 获取cpu类型(Chrome 不支持!!!),例如: "unknown"
     */
    getCpuClass: function(trueOrFake) {
      if (trueOrFake) {
        return navigator.cpuClass || "unknown"
      }
      return "unknown"
    }
}
```

- 平台类型

```js
chromeHelper.prototype = {
    /**
     * 获取平台类型键值对
     */
    getPlatformKeyAndValue: function(trueOrFake) {
      return {
        "key": "navigator_platform",
        "value": this.getPlatform(trueOrFake)
      }
    },
    /**
     * 获取平台类型,例如: "MacIntel"
     */
    getPlatform: function(trueOrFake) {
      if (trueOrFake) {
        return navigator.platform || "unknown"
      }
      return "MacIntel"
    }
}
```

- 反追踪

```js
chromeHelper.prototype = {
    /**
     * 获取反追踪键值对
     */
    getDoNotTrackKeyAndValue: function(trueOrFake) {
      return {
        "key": "do_not_track",
        "value": this.getDoNotTrack(trueOrFake)
      }
    },
    /**
     * 获取反追踪,例如: "unknown"
     */
    getDoNotTrack: function(trueOrFake) {
      if (trueOrFake) {
        return navigator.doNotTrack ? navigator.doNotTrack : navigator.msDoNotTrack ? navigator.msDoNotTrack : window.doNotTrack ? window.doNotTrack : "unknown"
      }
      return "unknown"
    }
}
```

- 插件

```js
chromeHelper.prototype = {
    /**
     * 获取插件键值对
     */
    getPluginsKeyAndValue: function(trueOrFake) {
      return {
        "key": "regular_plugins",
        "value": this.getPlugins(trueOrFake)
      }
    },
    /**
     * getRegularPlugins 1381 TODO 获取插件,例如: ["Chrome PDF Plugin::Portable Document Format::application/x-google-chrome-pdf~pdf","Chrome PDF Viewer::::application/pdf~pdf","Native Client::::application/x-nacl~,application/x-pnacl~"]
     */
    getPlugins: function(trueOrFake) {
      return [
        "Chrome PDF Plugin::Portable Document Format::application/x-google-chrome-pdf~pdf",
        "Chrome PDF Viewer::::application/pdf~pdf",
        "Native Client::::application/x-nacl~,application/x-pnacl~"
      ]
    }
}
```

- TODO 画布

```js
chromeHelper.prototype = {
    /**
     * 获取画布键值对
     */
    getCanvasKeyAndValue: function(trueOrFake) {
      return {
        "key": "canvas",
        "value": this.getCanvas(trueOrFake)
      }
    },
    /**
     * 画布,例如: "canvas winding:yes~canvas fp:data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAB9AAAADICAYAAACwGnoBAAAgAElEQVR4XuzdeXxU9b3/8deZzGQDEpawhF1ARJAdxLWVarXWtddCN2vdCFq197q0tfW2TX+3t6211bZWK8GtWm2r3ta2aqtd6L2uKDuCoCA7YQlLICQkM5nze3zOzAknYZJMQhKIvr+P2yvMnPM93/M8Cf+8z+fzdTjGh4s7ABgLnAiMAAYDhUABMKSR5W8AyoBSwP68FngHWO7gbPHPcXG7AKOBMYH/9gBygOzkf+3P/v/s1KoG/zuY/PseYAWw0v+vg3Ogbn1u294HzqH7OMYfoZYnAQlIQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEpCABCQgAQlIoFMIOMfaKl1cC8rPBs4ETgUGtfEa9wMVQBbQs43nrpvO0vrn89j94iiqF55C1z1n0c27I4v922ZsAl4HXgb+gePYJdMabhFuWgd+wA5ySjhmft6Liwl9aQiZte5JvWNubl+jDjuV23fsfbvs1H1UO8XEg/wuOA4fzuf2Afsx1O1IQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEpCABCQgAQkcwwLHRKDo4p4CfAq4KFlpfgyTNb60N4A/AH9OlrunPHICcDpwWvIVgbZ7PcACdLv0H3AcW0qjQwH60f3xeu+mnnk5Uwsuye7d4+IufQdOCOcVFOCGiO3fWXZg+5ZlB3bs+dP7i7f9cfpP9+49uivV1SUgAQlIQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEpCABCTw4RI4agG6i2u12FcC1wCjOiu79Yl/FHgIWNWam7BXBy4ALgbGtWaClOfYUmxJj+I4tsR6QwF6mzm3eKIFxUNHDRkVvq3HxGlXZ4y80IGTgO7JeSwvX0Xtu8+ze/Hrj2x+v+rHk7650bYE0JCABCQgAQlIQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEpCABDpA4PAAvbg4xJb+5wEfBc7AcfcB84iFH+Dha6z9+RENF9f2M78BmJ3uRA+xiidZQzVxZjKMPVTzHSane3qzxz3LeuaxlZ9xGrup5j94jdsYz7gmOrwvB+4D5jQ7e6oD3gUsLD25/pfnJmvwrQ6/sd3dG063vwqeeQPOGw/9U3aktyXeh+PYkr3RWID+BsN4gI9wP0+SS02r7qyjTqohzGr6Mpa6Le2bvXSLWrjPKrkSxz2BktnfaHbiNA/4538ed8K4sc4vep1x2jn0v4LaWHcyMtxEY3bXhRDU1kJGeB9s+y3lr73yz7Urq2+c/K0NabfnT3MpOkwCEpCABCQgAQlIQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEkgh0CBAdx2KSiwXvh54H3gR1xmH456O4/6Twm0fp7i43t7M6aq6uFZf/VXg8nTPseNWsZcTeYrT6cd0+rOTKl5nO0v5dEumafLY77GIH7GUfVzFZg4wiCf4E+dxUYoUexlwF/DrI7r6q8BWYEbqWbIDQfrnbHPsJi5Wth/u+A185XwY02Q/eFvyXTjOssYC9KeZzEyK2MmtFHjbxB+74yPcxmm8zw/5fdqLbFGAXjTnbuBsSmaPT/sCTRz4wk0j8k46qfqngz564lWMvJQDu6tY8fJCho0fTcHgId4O57s2bGDNkrcZ85GJdO2VB2uep3Tessfnr+zxlU/9dInaubfFg9AcEpCABCQgAQlIQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEmhCoH6APvuB7+I638Z1iphbNLfuvNkPXI7rPA5cR8nsFhVdu7i9ge8kq85b/DD+zhY+zvNeYN5URXiLJw6ckE6AvhP4brLq/EiulTi3mQA9eAHr8P2lZLN7a3rfcKQfoPtn3rdjtnODPZSGozMF6MP5b2awsP0C9CN/yHUzuMXFoaU5v/rCmPGZj4THnZFBjzzeXbCaP931N06beQqnfOYMcGD+U6/y6m9e4+Jbz2HktFGw9wCxZa/G31lafc2qtyY9PvPpp2vbcFmaSgISkIAEJCABCUhAAhKQgAQkIAEJSEACEpCABCQgAQlIQAISaCBQP0AvmrMd13mduUWX1j/OdZg19wc47vs47m6vQr0y92Iev+IARXNycdw/Ay8x57o7vfOK5lhp9VW1JUV/+WL4nz/5eGxg5GVKeYnNnEgPfsDJbOGAV/Vt7divZCS3MI4MSxEDw1qr38FbrGQPp9CHGQzzvn2PffySM7w/L2M3xSzgVbbTjxz+jeP4OhPIJoPtVPFZ/sFnGMZdLOME8vkNZ3tV7XezjH9RykcpJI9MnmJtvQr0bzCB19nhXXsghbzHKeyna3J11cCSZBW5FQbb56OBMcnv5wH9ge3AJiAn+b2/1XvDAN3m2pxs6d4nxQ/pDujxNpywBc7tDWcPhrfeh1suhF2BCvSKg/DKarj+45CblZinOgr3vwhnj4Md5bCviqwX76QbkzmRPVzBG1zLK96hfoA+h1/zCKexmR6cxbvcw1N1FelPMI2f8TE20Ivj2cE1vMJVvHbYmudyJv/DRP7I/WQR874vJZ/Pcy138ALn8A7/YiTf55MsZaA31038k8+wABeHq7mCXXTlNzxIF6rZSneu5ErOYjXr6YXN34f9nMcKHuORw64fI+TNbWuwcT3/y/XO55/CdR6jZPbzzH7g67jOQEpm31R3cqLqvIyS2d9nVsm/E4pPYM51VzGr5CfApMMuEor/njnX3dvcvyqv3XxqzsAhm58YdHKPT9FnKPRwWPH6ZuY9tIRx54/g9C9N9AL01x5bwpLn3+VjV45nzOmDYa8LOzex8c1df162ovdnLypZWNnctfS9BCQgAQlIQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEpCABCTQeoFDiXXRnMHAhmarzK+dO45QfCmOez5zrvsrsx84G9f5u9fyvWT2cFtK90sf+b+Jr/Y64Z87L+oznN/yPvv4FEP5CIX8hGWUU0MOYb7MaMo4yC9Ywatcwmn0rXcn71HO/azkpyznx5zitXH/Fe/yGtu8ivQ17ON4fstAuvDvjOUd9vAwq72g/SnOYSMVDOFJb87PM4IYce5kGuN4hkJyuYmTvOD8ZbbRjUi9AN3O+QTDeJ/evIs1brfh91O327Vg3PZht37ra5IBuG1eXgj8FrCt4y1Et9DfuuFby3Z7L8EC8mCAvghYAHwE8AP2IINtO/8MkJsI4bM2wYjNcIJ9XFQ/QO/ZDYqfgi99FE6zA4D5a+Dhf8IPPw9/XQL/WgnsAV6gkBGUMo3HeZjLmV8XoNtp1hr9AFn8FxdwCUt5lvu9oHsC3+IaXuXjrOSvjOFRTuNFfsa52LyHxgKGMJVvegH6xSz1vriX6XyFz3ot4pczgI9xixfQf443eZYJ/IWT6tbyGsM5na9xM3/nxzzDJ7kJ+2wV3/HW8RlmcQZrKOJlLvVeZqg/buPT/ISPcyt/88L/73M++71nxWxKZpcwq+RBHHccJbMPbUQ/q2QejruNktmfo2iOBeMfo2T2GIrmXIDjJvrju47tWH4NMBXX+Rxzi+xhNzle/s6wwSN77/9Xn7E9jyM/H3qFeW/ZHl58+F2mXTyEyZfZrx4s+sNGXn92A+dedTwnjO8Ju2NQvp+dS3duWrur4COnFq9a39y19L0EJCABCUhAAhKQgAQkIAEJSEACEpCABCQgAQlIQAISkIAEJNB6gUMB+qyS83HcF4DzKJn9UuNTevukbwR+R8ns2yia8wPg9uTxQzb86gufGlLzxE8fc6fzRY7HAnSrBl/KZYQJ8WOW8VXe8CrIr/OqtiGPR7iZsXyXKYdd9q9s4nz+wjo+x1C6cT2v1AXot/A697C87js7+dss4L9Y5AXs3cn0AvQbGcO9nO7N/S0W8DOWs5XL6UrE++wM/sQydtUL0CczjKWck6ydtupwozkLGAFexXY/YGRyvQeAJ4AzgROTAbptXH4ZXmkxVjhsW5D73/sBus31VnJef66GBG8mq92vSIb19v0zEN4N35kFsyrg24E90O98FsJhuPXCxEQ/fQFCwFc+Cb95JRmgfy/5AoCt7jZ60YOd3FEXoD9Fidce3cYVXMWLjGE7t9V9/0/uZjqrsSrvn/Mx788TvRcK6o8xFDOGrdh8NsbzLU5gu/d328N8Nf3YzNeJkOhMfjE3sJhBbEr+OBVzEd/lQr7E6/yKU3mBezmft71jm2rhXkkmXbiXG5nHvd7LDPAbpnrV760K0IO3ZWE6PIfrfI+5Rd9q/Pfk0Df/un3M1NEDNv6t9wk988nvBvkh9u6rZdGrezhxancKR1iHAihdU8XKN/cy6fTu9OgehvI4lFewc1VZxfvb+n38lP9e80Y619MxEpCABCQgAQlIQAISkIAEJCABCUhAAhKQgAQkIAEJSEACEpBA6wQOBejX/fIk4qHlOO7VzLnu8J7YwfkTra4voGT2CRTNsfJiK5H+f9/7/ZR548p6Tb+YF9nBFfQm2wvQz6Qfj3rhM/yedVzG33iPzzKCPO+zE/gdn2AQP+O0w+6iqQD9ZP7AAWKswDrGJ8Yb7OBUnvVatVtFuwXoj5EI821cyF+9/z7HJ+rO+SFL+D6LG1Sg23r9UNtakD/sRcAwzcqQkxXntjN6ebJVu1WcW0hvbdwttB2QDMz9y1iIfCowNlmBviL5hYWnnwEyG3mCz4EX4we76lvFulWuz4IpFdDzN3D3+TBmELy6Ch77P/j+5yHkwO1PwLVnw9ThiQB94TrYb5uq2z3YON+b+0r+nU8yhpkUsY5vMpRd3rc/42z+g5m4zGYn3ZjEHV5r95Fs51Ms4d9YxMmkLoz2z93NzWyhO2P5jheCf4IVhHjAa8H+Ud6tu2+rLH+XvuznK3SlmigZnMLtLGKwV4l+N0/XHdtUgP4WQzmZb/A/PMC/sdg7x9rH9+dH9seWV6D7V7127mRC8QU47lMUb"
     */
    getCanvas: function(trueOrFake) {
      if (trueOrFake) {
        var a = [],
          b = document.createElement("canvas");
        b.width = 2E3;
        b.height = 200;
        b.style.display = "inline";
        var c = b.getContext("2d");
        c.rect(0, 0, 10, 10);
        c.rect(2, 2, 6, 6);
        a.push("canvas winding:" + (!1 === c.isPointInPath(5, 5, "evenodd") ? "yes" : "no"));
        c.textBaseline = "alphabetic";
        c.fillStyle = "#f60";
        c.fillRect(125, 1, 62, 20);
        c.fillStyle = "#069";
        c.font = "11pt Arial";
        c.fillText("Cwm fjordbank glyphs vext quiz, \ud83d\ude03", 2, 15);
        c.fillStyle = "rgba(102, 204, 0, 0.2)";
        c.font = "18pt Arial";
        c.fillText("Cwm fjordbank glyphs vext quiz, \ud83d\ude03", 4, 45);
        c.globalCompositeOperation = "multiply";
        c.fillStyle = "rgb(255,0,255)";
        c.beginPath();
        c.arc(50, 50, 50, 0, 2 * Math.PI, !0);
        c.closePath();
        c.fill();
        c.fillStyle = "rgb(0,255,255)";
        c.beginPath();
        c.arc(100, 50, 50, 0, 2 * Math.PI, !0);
        c.closePath();
        c.fill();
        c.fillStyle = "rgb(255,255,0)";
        c.beginPath();
        c.arc(75, 100, 50, 0, 2 * Math.PI, !0);
        c.closePath();
        c.fill();
        c.fillStyle = "rgb(255,0,255)";
        c.arc(75, 75, 75, 0, 2 * Math.PI, !0);
        c.arc(75, 75, 25, 0, 2 * Math.PI, !0);
        c.fill("evenodd");
        a.push("canvas fp:" + b.toDataURL());
        return a.join("~")
      }
      fakeCanvas = "canvas winding:yes~canvas fp:data:image/png;base64,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fJK39A1x5rTb8+67t+17von9C3x06u05adkg9l8oKX/dfwNmEiBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAYGIL9Bqgz+y8yn37vra1z1/ckJ98ade+5po0mMC+R9+Qq/+tX/tbO69uXzhYE2YTIECAAAECBAgQIECAAAECBAgQIECAAAECBAgQIEBg4gr0FKDX26hSnZDkk31taaMrfpPf7r9VNuhrtkn9CjyWZJO5v8kjL9yqzxInlpSz+pxrGgECBAgQIECAAAECBAgQIECAAAECBAgQIECAAAECBFoh0HOAXu+qSvXlJG/oa4ff2HxJjljuS+h94fU56YIpS/Lq+/s1/0pJeWOfK5tGgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgACB1gj0G6DXr3C/LMnuPe/0pcf+IN8558Ce55nQv8DLjvlBvnt2P+bXJTmopNSvcPcQIECAAAECBAgQIECAAAECBAgQIECAAAECBAgQIEBgvRboK0CvRapUhyS5pGedjW6/Jsu32zMb9zzThH4EHk4y5bZr8si2e/Yx/dCScmkf80whQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIBA6wT6DtDrnVapZieZ0/Ou37r/vHzmilk9zzOhd4G3vXBePju3H+tTS8rpvS9oBgECBAgQIECAAAECBAgQIECAAAECBAgQIECAAAECBNopMFCAXm+5SvUfSV7d0/Y3mn9nbt57m/T7Ve6eFpvEg5ckecb/3plH9tqmR4VvlJQ/73GO4QQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIEGi1wDAC9JlJvpNkl54kjjpgQb42d7ee5hjcm8Br91+Q8y7v1fjGJC8rKQt7W8xoAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQItFtg4AC93n6V6kVJvpVki645yjWP5arnbpB9up5hYC8CVyd53s8fS7XnBj1Muy/JK0vKD3uYYygBAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgTWC4GhBOi1RJXqyCTn9aRywBE35off7O3mek8LTOLBL3rVjbn8gl5tjyop509iNVsnQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQGASCwwtQK8Nq1RvTnJO9553JRdu/2AOe3TT7ucYOarARRs+mMNv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kkunLMk/H/nLfO/0aXlk27rfefWfJvlWSblmvFqzLgECBAgQIECAAAECBAgQIECAAAECBAgQIECAAAECBCa7wHoVoK8Wptcvcd8/yfOTvCAbzZ2R53/21rz+u1Nz6G+2z/ZjdPS31tH4Vrfmay+9J1e+dbs8sn/90vYfJbkyydySUv+/hwABAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgTGWWC9DdBXd61STesE6i/M5j89IFvWQfqFD+SN1z4lL3joqUM9hx89+Y58eY+7csnhm+Xel96d+//48iRXdALzZUNdSzECBAgQIECAAAECBAgQIECAAAECBAgQIECAAAECBAgQGIrApAnQ16RVpdoiyX7Zdv5BKQtelLuX7p5Nb/xdpt/4UHZeWmWXezbMs3+zZWYm2alTYVGShUmu3+re3Lj1o7lph5JbdtkkD+6yQabucF2y2w9z2171t8x/XFLuG8opKUKAAAECBAgQIECAAAECBAgQIECAAAECBAgQIECAAAECjQtM6gC9cV0LECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgEBrBATorTkqjRIgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIBAkwIC9CZ11SZAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgACB1ggI0FtzVBolQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAgSYFBOhN6qpNgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAq0REKC35qg0SoAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQJNCgjQm9RVmwABAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgRaIyBAb81RaZQAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIEmhQQoDepqzYBAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQItEZAgN6ao9IoAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECDQpIEBvUldtAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIEGiNgAC9NUelUQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBBoUkCA3qSu2gQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECDQGgEBemuOSqMECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAg0KSAAL1JXbUJECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAoDUCAvTWHJVGCRAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQKBJAQF6k7pqEyBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgEBrBATorTkqjRIgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIBAkwIC9CZ11SZAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgACB1ggI0FtzVBolQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAgSYFBOhN6qpNgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAq0REKC35qg0SoAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQJNCgjQm9RVmwABAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgRaIyBAb81RaZQAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIEmhQQoDepqzYBAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQItEZAgN6ao9IoAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECDQpIEBvUldtAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIEGiNgAC9NUelUQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBBoUkCA3qSu2gQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECDQGgEBemuOSqMECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAg0KSAAL1JXbUJECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAoDUCAvTWHJVGCRAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQKBJAQF6k7pqEyBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgEBrBATorTkqjRIgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIBAkwIC9CZ11SZAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgACB1ggI0FtzVBolQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAgSYFBOhN6qpNgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAq0REKC35qg0SoAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQJNCgjQm9RVmwABAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgRaIyBAb81RaZQAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIEmhQQoDepqzYBAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQItEZAgN6ao9IoAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECDQpIEBvUldtAgQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIEGiNgAC9NUelUQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBBoUkCA3qSu2gQIECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECDQGgEBemuOSqMECBAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAg0KSAAL1JXbUJECBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAoDUCAvTWHJVGCRAgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQKBJAQF6k7pqEyBAgAABAgQIECBAgAABAgQIECBAgAABAgQIECBAgEBrBATorTkqjRIgQIAAAQIECBAgQIAAAQIECBAgQIAAAQIECBAgQIBAkwL/Dyu9q22HnlUDAAAAAElFTkSuQmCC";
      return fakeCanvas
    }
}
```

- webgl画布

```js
chromeHelper.prototype = {
    /**
     * 获取webgl键值对
     */
    getWebglKeyAndValue: function(trueOrFake) {
      return {
        "key": "webgl",
        "value": this.getWebgl(trueOrFake)
      }
    },
    /**
     * webgl,例如: "data:image/png;base64,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~extensions:ANGLE_instanced_arrays;EXT_blend_minmax;EXT_color_buffer_half_float;EXT_disjoint_timer_query;EXT_float_blend;EXT_frag_depth;EXT_shader_texture_lod;EXT_texture_filter_anisotropic;WEBKIT_EXT_texture_filter_anisotropic;EXT_sRGB;OES_element_index_uint;OES_fbo_render_mipmap;OES_standard_derivatives;OES_texture_float;OES_texture_float_linear;OES_texture_half_float;OES_texture_half_float_linear;OES_vertex_array_object;WEBGL_color_buffer_float;WEBGL_compressed_texture_s3tc;WEBKIT_WEBGL_compressed_texture_s3tc;WEBGL_compressed_texture_s3tc_srgb;WEBGL_debug_renderer_info;WEBGL_debug_shaders;WEBGL_depth_texture;WEBKIT_WEBGL_depth_texture;WEBGL_draw_buffers;WEBGL_lose_context;WEBKIT_WEBGL_lose_context~webgl aliased line width range:[1, 1]~webgl aliased point size range:[1, 255.875]~webgl alpha bits:8~webgl antialiasing:yes~webgl blue bits:8~webgl depth bits:24~webgl green bits:8~webgl max anisotropy:16~webgl max combined texture image units:80~webgl max cube map texture size:16384~webgl max fragment uniform vectors:1024~webgl max render buffer size:16384~webgl max texture image units:16~webgl max texture size:16384~webgl max varying vectors:15~webgl max vertex attribs:16~webgl max vertex texture image units:16~webgl max vertex uniform vectors:1024~webgl max viewport dims:[16384, 16384]~webgl red bits:8~webgl renderer:WebKit WebGL~webgl shading language version:WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)~webgl stencil bits:0~webgl vendor:WebKit~webgl version:WebGL 1.0 (OpenGL ES 2.0 Chromium)~webgl vertex shader high float precision:23~webgl vertex shader high float precision rangeMin:127~webgl vertex shader high float precision rangeMax:127~webgl vertex shader medium float precision:23~webgl vertex shader medium float precision rangeMin:127~webgl vertex shader medium float precision rangeMax:127~webgl vertex shader low float precision:23~webgl vertex shader low float precision rangeMin:127~webgl vertex shader low float precision rangeMax:127~webgl fragment shader high float precision:23~webgl fragment shader high float precision rangeMin:127~webgl fragment shader high float precision rangeMax:127~webgl fragment shader medium float precision:23~webgl fragment shader medium float precision rangeMin:127~webgl fragment shader medium float precision rangeMax:127~webgl fragment shader low float precision:23~webgl fragment shader low float precision rangeMin:127~webgl fragment shader low float precision rangeMax:127~webgl vertex shader high int precision:0~webgl vertex shader high int precision rangeMin:31~webgl vertex shader high int precision rangeMax:30~webgl vertex shader medium int precision:0~webgl vertex shader medium int precision rangeMin:31~webgl vertex shader medium int precision rangeMax:30~webgl vertex shader low int precision:0~webgl vertex shader low int precision rangeMin:31~webgl vertex shader low int precision rangeMax:30~webgl fragment shader high int precision:0~webgl fragment shader high int precision rangeMin:31~webgl fragment shader high int precision rangeMax:30~webgl fragment shader medium int precision:0~webgl fragment shader medium int precision rangeMin:31~webgl fragment shader medium int precision rangeMax:30~webgl fragment shader low int precision:0~webgl fragment shader low int precision rangeMin:31~webgl fragment shader low int precision rangeMax:30"
     */
    getWebgl: function(trueOrFake) {
      if (trueOrFake) {
        function a(a) {
          b.clearColor(0, 0, 0, 1);
          b.enable(b.DEPTH_TEST);
          b.depthFunc(b.LEQUAL);
          b.clear(b.COLOR_BUFFER_BIT | b.DEPTH_BUFFER_BIT);
          return "[" + a[0] + ", " + a[1] + "]"
        }
        var b;
        b = this.getWebglCanvas();
        if (!b)
          return null;
        var c = [],
          d = b.createBuffer();
        b.bindBuffer(b.ARRAY_BUFFER, d);
        var e = new Float32Array([-.2, -.9, 0, .4, -.26, 0, 0, .732134444, 0]);
        b.bufferData(b.ARRAY_BUFFER, e, b.STATIC_DRAW);
        d.itemSize = 3;
        d.numItems = 3;
        var e = b.createProgram(),
          f = b.createShader(b.VERTEX_SHADER);
        b.shaderSource(f, "attribute vec2 attrVertex;varying vec2 varyinTexCoordinate;uniform vec2 uniformOffset;void main(){varyinTexCoordinate\x3dattrVertex+uniformOffset;gl_Position\x3dvec4(attrVertex,0,1);}");
        b.compileShader(f);
        var g = b.createShader(b.FRAGMENT_SHADER);
        b.shaderSource(g, "precision mediump float;varying vec2 varyinTexCoordinate;void main() {gl_FragColor\x3dvec4(varyinTexCoordinate,0,1);}");
        b.compileShader(g);
        b.attachShader(e, f);
        b.attachShader(e, g);
        b.linkProgram(e);
        b.useProgram(e);
        e.vertexPosAttrib = b.getAttribLocation(e, "attrVertex");
        e.offsetUniform = b.getUniformLocation(e, "uniformOffset");
        b.enableVertexAttribArray(e.vertexPosArray);
        b.vertexAttribPointer(e.vertexPosAttrib, d.itemSize, b.FLOAT, !1, 0, 0);
        b.uniform2f(e.offsetUniform, 1, 1);
        b.drawArrays(b.TRIANGLE_STRIP, 0, d.numItems);
        null != b.canvas && c.push(b.canvas.toDataURL());
        c.push("extensions:" + b.getSupportedExtensions().join(";"));
        c.push("webgl aliased line width range:" + a(b.getParameter(b.ALIASED_LINE_WIDTH_RANGE)));
        c.push("webgl aliased point size range:" + a(b.getParameter(b.ALIASED_POINT_SIZE_RANGE)));
        c.push("webgl alpha bits:" + b.getParameter(b.ALPHA_BITS));
        c.push("webgl antialiasing:" + (b.getContextAttributes().antialias ? "yes" : "no"));
        c.push("webgl blue bits:" + b.getParameter(b.BLUE_BITS));
        c.push("webgl depth bits:" + b.getParameter(b.DEPTH_BITS));
        c.push("webgl green bits:" + b.getParameter(b.GREEN_BITS));
        c.push("webgl max anisotropy:" + function(a) {
          var b, c = a.getExtension("EXT_texture_filter_anisotropic") || a.getExtension("WEBKIT_EXT_texture_filter_anisotropic") || a.getExtension("MOZ_EXT_texture_filter_anisotropic");
          return c ? (b = a.getParameter(c.MAX_TEXTURE_MAX_ANISOTROPY_EXT),
            0 === b && (b = 2),
            b) : null
        }(b));
        c.push("webgl max combined texture image units:" + b.getParameter(b.MAX_COMBINED_TEXTURE_IMAGE_UNITS));
        c.push("webgl max cube map texture size:" + b.getParameter(b.MAX_CUBE_MAP_TEXTURE_SIZE));
        c.push("webgl max fragment uniform vectors:" + b.getParameter(b.MAX_FRAGMENT_UNIFORM_VECTORS));
        c.push("webgl max render buffer size:" + b.getParameter(b.MAX_RENDERBUFFER_SIZE));
        c.push("webgl max texture image units:" + b.getParameter(b.MAX_TEXTURE_IMAGE_UNITS));
        c.push("webgl max texture size:" + b.getParameter(b.MAX_TEXTURE_SIZE));
        c.push("webgl max varying vectors:" + b.getParameter(b.MAX_VARYING_VECTORS));
        c.push("webgl max vertex attribs:" + b.getParameter(b.MAX_VERTEX_ATTRIBS));
        c.push("webgl max vertex texture image units:" + b.getParameter(b.MAX_VERTEX_TEXTURE_IMAGE_UNITS));
        c.push("webgl max vertex uniform vectors:" + b.getParameter(b.MAX_VERTEX_UNIFORM_VECTORS));
        c.push("webgl max viewport dims:" + a(b.getParameter(b.MAX_VIEWPORT_DIMS)));
        c.push("webgl red bits:" + b.getParameter(b.RED_BITS));
        c.push("webgl renderer:" + b.getParameter(b.RENDERER));
        c.push("webgl shading language version:" + b.getParameter(b.SHADING_LANGUAGE_VERSION));
        c.push("webgl stencil bits:" + b.getParameter(b.STENCIL_BITS));
        c.push("webgl vendor:" + b.getParameter(b.VENDOR));
        c.push("webgl version:" + b.getParameter(b.VERSION));
        if (!b.getShaderPrecisionFormat)
          return c.join("~");
        c.push("webgl vertex shader high float precision:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.HIGH_FLOAT).precision);
        c.push("webgl vertex shader high float precision rangeMin:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.HIGH_FLOAT).rangeMin);
        c.push("webgl vertex shader high float precision rangeMax:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.HIGH_FLOAT).rangeMax);
        c.push("webgl vertex shader medium float precision:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.MEDIUM_FLOAT).precision);
        c.push("webgl vertex shader medium float precision rangeMin:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.MEDIUM_FLOAT).rangeMin);
        c.push("webgl vertex shader medium float precision rangeMax:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.MEDIUM_FLOAT).rangeMax);
        c.push("webgl vertex shader low float precision:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.LOW_FLOAT).precision);
        c.push("webgl vertex shader low float precision rangeMin:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.LOW_FLOAT).rangeMin);
        c.push("webgl vertex shader low float precision rangeMax:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.LOW_FLOAT).rangeMax);
        c.push("webgl fragment shader high float precision:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.HIGH_FLOAT).precision);
        c.push("webgl fragment shader high float precision rangeMin:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.HIGH_FLOAT).rangeMin);
        c.push("webgl fragment shader high float precision rangeMax:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.HIGH_FLOAT).rangeMax);
        c.push("webgl fragment shader medium float precision:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.MEDIUM_FLOAT).precision);
        c.push("webgl fragment shader medium float precision rangeMin:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.MEDIUM_FLOAT).rangeMin);
        c.push("webgl fragment shader medium float precision rangeMax:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.MEDIUM_FLOAT).rangeMax);
        c.push("webgl fragment shader low float precision:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.LOW_FLOAT).precision);
        c.push("webgl fragment shader low float precision rangeMin:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.LOW_FLOAT).rangeMin);
        c.push("webgl fragment shader low float precision rangeMax:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.LOW_FLOAT).rangeMax);
        c.push("webgl vertex shader high int precision:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.HIGH_INT).precision);
        c.push("webgl vertex shader high int precision rangeMin:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.HIGH_INT).rangeMin);
        c.push("webgl vertex shader high int precision rangeMax:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.HIGH_INT).rangeMax);
        c.push("webgl vertex shader medium int precision:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.MEDIUM_INT).precision);
        c.push("webgl vertex shader medium int precision rangeMin:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.MEDIUM_INT).rangeMin);
        c.push("webgl vertex shader medium int precision rangeMax:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.MEDIUM_INT).rangeMax);
        c.push("webgl vertex shader low int precision:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.LOW_INT).precision);
        c.push("webgl vertex shader low int precision rangeMin:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.LOW_INT).rangeMin);
        c.push("webgl vertex shader low int precision rangeMax:" + b.getShaderPrecisionFormat(b.VERTEX_SHADER, b.LOW_INT).rangeMax);
        c.push("webgl fragment shader high int precision:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.HIGH_INT).precision);
        c.push("webgl fragment shader high int precision rangeMin:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.HIGH_INT).rangeMin);
        c.push("webgl fragment shader high int precision rangeMax:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.HIGH_INT).rangeMax);
        c.push("webgl fragment shader medium int precision:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.MEDIUM_INT).precision);
        c.push("webgl fragment shader medium int precision rangeMin:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.MEDIUM_INT).rangeMin);
        c.push("webgl fragment shader medium int precision rangeMax:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.MEDIUM_INT).rangeMax);
        c.push("webgl fragment shader low int precision:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.LOW_INT).precision);
        c.push("webgl fragment shader low int precision rangeMin:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.LOW_INT).rangeMin);
        c.push("webgl fragment shader low int precision rangeMax:" + b.getShaderPrecisionFormat(b.FRAGMENT_SHADER, b.LOW_INT).rangeMax);
        return c.join("~")
      }
      return "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASwAAACWCAYAAABkW7XSAAAM80lEQVR4Xu2dXYgkVxXHT/XMIBJEQUSDBF1Qwj7ETxKEPFgj5CEoKARRQR+CgoLmIaAoKEy3+iBBIiioEEEfVERBRURFwRkVP2A1s8wsOzCzZDYZHTeJGM3GXZKNU3K7e+yanv6o7q6695x7f/M61XXP+f8PP+49dW9VJvyhAAqggBEFMiNxEiYKoAAKCMCiCFAABcwoALDMWEWgKIACAIsaQAEUMKMAwDJjFYGiAAoALGoABVDAjAIAy4xVBIoCKACwqAEUQAEzCgAsM1YRKAqgAMCiBlAABcwoALDMWEWgKIACAIsaQAEUMKMAwDJjFYGiAAoALGoABVDAjAIAy4xVBIoCKACwqAEUQAEzCgAsM1YRKAqgAMCiBlAABcwoALDMWEWgKIACAIsaqF2BG4XkyyJ5lkm79ptzw6QVAFhJ299M8n1grYvIapbJRjOjcNcUFQBYKbrecM5HhaxnIrkbJsv4bkDDcid1e4CVlN1+ki0DS0Q6LA396J7CKAArBZc953hUSDFUWEDLswexDgewYnU2UF6uf7Uk3SXh8B/QCuRJTMMCrJjcVJDL84Wst9wTwtGx0IRX4JHlEACWZfcUxj4FWDThFXpmKSSAZcktA7E+X0jRck8Hx8e6kWWyaiAVQlSoAMBSaIrVkFz/KpPuknDaXgb6WVZNDhw3wApsQEzD3+jvv6oALJc20IrJfE+5ACxPQqcwzIzAcpLQhE+hMGrMEWDVKGbqt7rR339VcYbVlYud8KlXzWz5A6zZ9OLqMQpc7x147u6/mgVYIkITnqqqrADAqiwVF05S4Hoh7WWRtTmART+L0qqsAMCqLBUXTlLguX7DfU5gAS3Kq5ICAKuSTFw0TYFnS/uvZlwSlm/Nk8NpQif+f4CVeAHUkb7rX7VK+68WABZN+DoMifgeACtic32l9p9+/+oYVIsAiya8L9dsjgOwbPqmKurr/QPPNQGLfpYqd3UFA7B0+WEymuv9/lWNwAJaJiuh+aABVvMaRz3C1f7+KwermoEFtKKunPmSA1jz6cav+gq4/lVLZK0hYNGEp9JOKACwKIiFFLhWOvDcwAzLxcZO+IUciuvHACsuP71nc610frAhYLE09O6q3gEBll5v1Efm+lfH729vaklYEoFNpeorovkAAVbzGkc7wtVC2kul84MNzrCONQRa0VZTtcQAVjWduGqEAs+UPjjhYYZ1HAHv0Eq4GgFWwuYvmvozQ+cHPcywaMIvaprx3wMs4waGCv+pQvKVofODnoBFEz6U6QrGBVgKTLAYQmBgAS2LRVNDzACrBhFTvMXTI84Pepxh0YRPseimf40pUVVIe6oCT484PxgAWC5OmvBT3YrnAmZY8XjpLRO3HHT7r4afDAYCFsd3vDkffiCAFd4DcxFoAxbfODRXQnMHDLDmli7dH7r+lUj3LaMn3tAQaobVd4JNpQmUJMBKwOS6U/z3mPODgYHFk8O6jVZ4P4Cl0BTNIbnloHt/+/HXccqQUgAsmvCai6eG2ABWDSKmdAsDwKIJH3FBAqyIzW0itacmnB9UMsNyafMOrSbMV3BPgKXABEshGAEW/SxLRTVDrABrBrFSv/TJ0vvbFfewyjbx5DCyogVYkRnaZDoGgUUTvsmCCHBvgBVAdKtD/rP0/nYjM6yu1Fkm1LnVohuKGyMjMdJHGlaBRRPeR3X4GQNg+dHZ/ChuOej2X03a3a7oKeEovelnma9CYaocgYdeUogAWDw59FIpzQ7CDKtZfaO5+5Ol91+N292ufIZ17AUzLcNVCbAMm+cz9IiARRPeZ+HUPBbAqlnQGG/399L+K8M9rLI17IQ3WqgAy6hxPsOOEFj0s3wWUI1jAawaxYz1Vk+U9l9FMsOin2W0WAGWUeN8hh0xsJhp+SykGsYCWDWIGPstnhh6YZ/xp4Sn7GInvJ0KBlh2vAoSqetfuQ9OlI/ixAYsdsIHKa25BgVYc8mWzo+uHMl6q5A8cmCJHEknW5F2Os7azBRg2fTNW9RXnusex4kfWE7RQjrZC4CWt+KaYyCANYdoKf3kyjUphpeAES4JB5Y6aN0EtLTWOMDS6oyCuA6uSr7SP/Ac/ZLwpN6r2YtkQ4EFhDCkAMCiJMYqcPiv3nIwqRlWT42N7CWySmnoUwBg6fNETUSH/0gWWL1+1stYGqopxn4gAEubI4riOXy8179KcIbVc8E9ObwZaCkqSd6HpckMTbEcHEi+1Br9wr6om+7DJjho3QK0tNQmMywtTiiL4/AxWZd+/yrZGdaxJ/+V1ewMTXgNJQqwNLigMIbDfYBVtiU7w2pEQ5kCLA0uKIzhb3tSjPsyTlJLwmNv3NLwVpaGoUsVYIV2QOH4BzuSt7KT5wcT24c12hX35PAs0ApZsgArpPpKx350W9rLmawxwxphkIPWbUArVOkCrFDKKx73YOv0gWdmWCXDjmQ1eyNN+BAlDLBCqK58zIOHT58fBFgnTcveRBM+RBkDrBCqKx5z/4+SLy/19l+xJJxo1EZ2B8d3fJcywPKtuPLx9n8v7eWWrAGsCka5J4d30s+qoFRtlwCs2qSM40aP/a77dPDUgWeWhGP8ddB6K9DyVf0Ay5fSRsZ5dGP0+UGANcHAQlazVZrwPkocYPlQ2cgY+7+SvDXm/CDAmmCim2XdxSzLR5kDLB8qGxlj/+fSbvX7V/SwKprmYHU3sKqo1sKXAayFJYznBpd/NuhfAawpvroNpG8HVL6rH2D5VlzxeJd/Mv78IEvCvnEOVO8EVKHKGGCFUl7ZuHs/6r2/fdzeq+SB5ZZ+9wCq0GULsEI7oGT8vR9IvjLhwHOywHIzqncDKiVlyvECLUaEjuOR753+/mDSO90dqN4HqELX5fD4zLC0ORIonke+M/n8YDIzLLf0+wCgClSGU4cFWFMliv+CvW8O3t+ebA/rSDrO6exeYKW54gGWZnc8xbb3kORLUw48Rz3Dcsu/DwIqT+W20DAAayH54vjxpYdGf38w+h6WW/59GFBZqmKAZcmthmK99LXp5wejmmE5UH0UUDVUTo3eFmA1Kq/+m+98pbedYdQHU6ObYbml332ASn9Vjo8QYFl2r4bYd76UALAcqO4HVDWUS/BbAKzgFoQNYPfB3vvbo5xhuaXfJwBV2Aqrd3SAVa+e5u62+8Cgf1Xle4NVwRa0sNyM6pOAylwxVgg4aF1ViI9LGlRg53OD7QxVQVT1uiCFVfT3Un0aWDVYNkFvHaSugmbM4P9XoAssGTTcTc+w3PJvDVDFXt4AK3aHJ+S3uybr0n9/e9WZU9XrvBSWG8SB6rOAKpUy9lJXqYhpLc/dz5gGVif7PKCyVnOLxguwFlXQ6O93PiV5a+h1MiaWhK6h/gVAZbTsFg4bYC0soc0b7HzcGLAcqL4IqGxWW31RA6z6tDR1p537Zb2VST7rkZsAPaxO9iCgMlVcDQYLsBoUV/Otd+5TD6xO9mVApbmGQsQGsEKoHnjM7Y9Jvlz0tjOom2Ed76X6KrAKXCYqhwdYKm1pNqjtj0i+3N9/pQxYnezrgKpZ923fHWDZ9m+u6C9+aPD9QSXA6mTfAFRzmZnYjwBWYoa7dC/eqwRY7snftwBVgiU4d8oAa27pbP5w+/297QyjnvZ524flQPVtQGWzgsJGDbDC6u999O33St5qBQNWJ/suoPJuekQDAqyIzKySyoX3DJaD3p4SuhnV9wFVFX+4ZrICACuxCrlwj0dgOVD9EFAlVmKNpguwGpVX380vvEuKRd7VXmWneybS6c7efgys9FWA7YgAlm3/Zop+8x2Sr0h3hiVNNd1FpLPyU0A1kzFcXFkBgFVZKvsXbt3d+/5gE8CSQjov/AWgsl8lujMAWLr9qTW6rbtOnx9cdOOoW/7d9EtAVatR3GysAgAroeLYelvvgxN1zLDc0u/FvwZUCZWPilQBlgobmg9iM5d8acSB51lnWG7p99LfAqrmHWOEUQoArETq4vydvf7VvDOsopDOy/8AqBIpF7VpAiy11tQb2Pm3zAcs16O6+U+Aql43uNu8CgCseZUz9rvztw8+mFqlh+WWfm429so/AytjVkcdLsCK2t5ecptvkDwbc+B5VA/LwepV5wFVAqVhLkWAZc6y2QP+y+ukvSSyVmGXeufMFqCaXWF+4UsBgOVL6YDjbJ6V9SzrNdxHNd1dn+q1FwFVQIsYuqICAKuiUJYv27x19PlBt/Q7uwuoLHubWuwAK3LHz72m9/72cq+qEOncdglQRW59lOkBrChtHSR17tXSXhZZc0a7pd/rLwOqyC2POj2AFbW9Ig/fIutFIb95818BVeRWJ5EewIrc5nOvkPbtV4BV5DYnkx7ASsZqEkUB+woALPsekgEKJKMAwErGahJFAfsKACz7HpIBCiSjAMBKxmoSRQH7CgAs+x6SAQokowDASsZqEkUB+wr8D8aUFbX98HGDAAAAAElFTkSuQmCC~extensions:ANGLE_instanced_arrays;EXT_blend_minmax;EXT_color_buffer_half_float;EXT_disjoint_timer_query;EXT_float_blend;EXT_frag_depth;EXT_shader_texture_lod;EXT_texture_filter_anisotropic;WEBKIT_EXT_texture_filter_anisotropic;EXT_sRGB;OES_element_index_uint;OES_fbo_render_mipmap;OES_standard_derivatives;OES_texture_float;OES_texture_float_linear;OES_texture_half_float;OES_texture_half_float_linear;OES_vertex_array_object;WEBGL_color_buffer_float;WEBGL_compressed_texture_s3tc;WEBKIT_WEBGL_compressed_texture_s3tc;WEBGL_compressed_texture_s3tc_srgb;WEBGL_debug_renderer_info;WEBGL_debug_shaders;WEBGL_depth_texture;WEBKIT_WEBGL_depth_texture;WEBGL_draw_buffers;WEBGL_lose_context;WEBKIT_WEBGL_lose_context~webgl aliased line width range:[1, 1]~webgl aliased point size range:[1, 255.875]~webgl alpha bits:8~webgl antialiasing:yes~webgl blue bits:8~webgl depth bits:24~webgl green bits:8~webgl max anisotropy:16~webgl max combined texture image units:80~webgl max cube map texture size:16384~webgl max fragment uniform vectors:1024~webgl max render buffer size:16384~webgl max texture image units:16~webgl max texture size:16384~webgl max varying vectors:15~webgl max vertex attribs:16~webgl max vertex texture image units:16~webgl max vertex uniform vectors:1024~webgl max viewport dims:[16384, 16384]~webgl red bits:8~webgl renderer:WebKit WebGL~webgl shading language version:WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)~webgl stencil bits:0~webgl vendor:WebKit~webgl version:WebGL 1.0 (OpenGL ES 2.0 Chromium)~webgl vertex shader high float precision:23~webgl vertex shader high float precision rangeMin:127~webgl vertex shader high float precision rangeMax:127~webgl vertex shader medium float precision:23~webgl vertex shader medium float precision rangeMin:127~webgl vertex shader medium float precision rangeMax:127~webgl vertex shader low float precision:23~webgl vertex shader low float precision rangeMin:127~webgl vertex shader low float precision rangeMax:127~webgl fragment shader high float precision:23~webgl fragment shader high float precision rangeMin:127~webgl fragment shader high float precision rangeMax:127~webgl fragment shader medium float precision:23~webgl fragment shader medium float precision rangeMin:127~webgl fragment shader medium float precision rangeMax:127~webgl fragment shader low float precision:23~webgl fragment shader low float precision rangeMin:127~webgl fragment shader low float precision rangeMax:127~webgl vertex shader high int precision:0~webgl vertex shader high int precision rangeMin:31~webgl vertex shader high int precision rangeMax:30~webgl vertex shader medium int precision:0~webgl vertex shader medium int precision rangeMin:31~webgl vertex shader medium int precision rangeMax:30~webgl vertex shader low int precision:0~webgl vertex shader low int precision rangeMin:31~webgl vertex shader low int precision rangeMax:30~webgl fragment shader high int precision:0~webgl fragment shader high int precision rangeMin:31~webgl fragment shader high int precision rangeMax:30~webgl fragment shader medium int precision:0~webgl fragment shader medium int precision rangeMin:31~webgl fragment shader medium int precision rangeMax:30~webgl fragment shader low int precision:0~webgl fragment shader low int precision rangeMin:31~webgl fragment shader low int precision rangeMax:30"
    }
}
```

- adBlock广告拦截

```js
chromeHelper.prototype = {
    /**
     * webgl画布,例如: WebGLRenderingContext
     */
    getWebglCanvas: function() {
      var a = document.createElement("canvas"),
        b = null;
      try {
        b = a.getContext("webgl") || a.getContext("experimental-webgl")
      } catch (c) {}
      b || (b = null);
      return b
    },
    /**
     * 获取adBlock广告拦截键值对
     */
    getAdBlockKeyAndValue: function(trueOrFake) {
      return {
        "key": "adblock",
        "value": this.getAdBlock(trueOrFake)
      }
    }
}
```

- 说谎语言

```js
chromeHelper.prototype = {
    /**
     * 获取adBlock广告拦截,例如: "0"
     */
    getAdBlock: function(trueOrFake) {
      if (trueOrFake) {
        var a = document.createElement("div");
        a.innerHTML = "\x26nbsp;";
        a.className = "adsbox";
        var b = "0";
        try {
          document.body.appendChild(a),
            0 === document.getElementsByClassName("adsbox")[0].offsetHeight && (b = "1"),
            document.body.removeChild(a)
        } catch (c) {
          b = "0"
        }
        return b
      }
      return "0"
    },
    /**
     * 获取说谎语言键值对
     */
    getHasLiedLanguagesKeyAndValue: function(trueOrFake) {
      return {
        "key": "has_lied_languages",
        "value": this.getHasLiedLanguages(trueOrFake)
      }
    }
}
```

- 说谎分辨率

```js
chromeHelper.prototype = {
    /**
     * 获取说谎语言键值对
     */
    getHasLiedLanguagesKeyAndValue: function(trueOrFake) {
      return {
        "key": "has_lied_languages",
        "value": this.getHasLiedLanguages(trueOrFake)
      }
    },
    /**
     * 获取说谎语言,例如: false
     */
    getHasLiedLanguages: function(trueOrFake) {
      if (trueOrFake) {
        if ("undefined" !== typeof navigator.languages)
          try {
            if (navigator.languages[0].substr(0, 2) !== navigator.language.substr(0, 2))
              return !0
          } catch (a) {
            return !0
          }
        return !1
      }
      return !1
    }
}
```

- 说谎操作系统

```js
chromeHelper.prototype = {
    /**
     * 获取说谎分辨率键值对
     */
    getHasLiedResolutionKeyAndValue: function(trueOrFake) {
      return {
        "key": "has_lied_resolution",
        "value": this.getHasLiedResolution(trueOrFake)
      }
    },
    /**
     * 获取说谎分辨率,例如: false
     */
    getHasLiedResolution: function(trueOrFake) {
      if (trueOrFake) {
        return screen.width < screen.availWidth || screen.height < screen.availHeight ? !0 : !1
      }
      return !1
    }
}
```

- 说谎浏览器

```js
chromeHelper.prototype = {
    /**
     * 获取说谎操作系统键值对
     */
    getHasLiedOsKeyAndValue: function(trueOrFake) {
      return {
        "key": "has_lied_os",
        "value": this.getHasLiedOs(trueOrFake)
      }
    },
    /**
     * 获取说谎操作系统,例如: false
     */
    getHasLiedOs: function(trueOrFake) {
      if (trueOrFake) {
        var a = navigator.userAgent.toLowerCase(),
          b = navigator.oscpu,
          c = navigator.platform.toLowerCase(),
          a = 0 <= a.indexOf("windows phone") ? "Windows Phone" : 0 <= a.indexOf("win") ? "Windows" : 0 <= a.indexOf("android") ? "Android" : 0 <= a.indexOf("linux") ? "Linux" : 0 <= a.indexOf("iphone") || 0 <= a.indexOf("ipad") ? "iOS" : 0 <= a.indexOf("mac") ? "Mac" : "Other";
        return ("ontouchstart" in window || 0 < navigator.maxTouchPoints || 0 < navigator.msMaxTouchPoints) && "Windows Phone" !== a && "Android" !== a && "iOS" !== a && "Other" !== a || "undefined" !== typeof b && (b = b.toLowerCase(),
          0 <= b.indexOf("win") && "Windows" !== a && "Windows Phone" !== a || 0 <= b.indexOf("linux") && "Linux" !== a && "Android" !== a || 0 <= b.indexOf("mac") && "Mac" !== a && "iOS" !== a || 0 === b.indexOf("win") && 0 === b.indexOf("linux") && 0 <= b.indexOf("mac") && "other" !== a) ? !0 : 0 <= c.indexOf("win") && "Windows" !== a && "Windows Phone" !== a || (0 <= c.indexOf("linux") || 0 <= c.indexOf("android") || 0 <= c.indexOf("pike")) && "Linux" !== a && "Android" !== a || (0 <= c.indexOf("mac") || 0 <= c.indexOf("ipad") || 0 <= c.indexOf("ipod") || 0 <= c.indexOf("iphone")) && "Mac" !== a && "iOS" !== a || 0 === c.indexOf("win") && 0 === c.indexOf("linux") && 0 <= c.indexOf("mac") && "other" !== a ? !0 : "undefined" === typeof navigator.plugins && "Windows" !== a && "Windows Phone" !== a ? !0 : !1
      }
      return !1
    }
}
```

- 触摸支持

```js
chromeHelper.prototype = {
    /**
     * 获取触摸支持键值对
     */
    getTouchSupportKeyAndValue: function(trueOrFake) {
      return {
        "key": "touch_support",
        "value": this.getTouchSupport(trueOrFake)
      }
    },
    /**
     * 获取触摸支持,例如: [0,false,false]
     */
    getTouchSupport: function(trueOrFake) {
      if (trueOrFake) {
        var a = 0,
          b = !1;
        "undefined" !== typeof navigator.maxTouchPoints ? a = navigator.maxTouchPoints : "undefined" !== typeof navigator.msMaxTouchPoints && (a = navigator.msMaxTouchPoints);
        try {
          document.createEvent("TouchEvent"),
            b = !0
        } catch (c) {}
        return [a, b, "ontouchstart" in window]
      }
      return [0, !1, !1]
    }
}
```

- 字体

```js
chromeHelper.prototype = {
    /**
     * 获取字体键值对
     */
    getFontsKeyAndValue: function(trueOrFake) {
      return {
        "key": "js_fonts",
        "value": this.getFonts(trueOrFake)
      }
    },
    /**
     * 获取字体,例如: [0,false,false]
     */
    getFonts: function(trueOrFake) {
      if (trueOrFake) {
        function d() {
          var a = document.createElement("span");
          a.style.position = "absolute";
          a.style.left = "-9999px";
          a.style.fontSize = "72px";
          a.style.lineHeight = "normal";
          a.innerHTML = "mmmmmmmmmmlli";
          return a
        }
        var e = ["monospace", "sans-serif", "serif"],
          f = "Andale Mono;Arial;Arial Black;Arial Hebrew;Arial MT;Arial Narrow;Arial Rounded MT Bold;Arial Unicode MS;Bitstream Vera Sans Mono;Book Antiqua;Bookman Old Style;Calibri;Cambria;Cambria Math;Century;Century Gothic;Century Schoolbook;Comic Sans;Comic Sans MS;Consolas;Courier;Courier New;Garamond;Geneva;Georgia;Helvetica;Helvetica Neue;Impact;Lucida Bright;Lucida Calligraphy;Lucida Console;Lucida Fax;LUCIDA GRANDE;Lucida Handwriting;Lucida Sans;Lucida Sans Typewriter;Lucida Sans Unicode;Microsoft Sans Serif;Monaco;Monotype Corsiva;MS Gothic;MS Outlook;MS PGothic;MS Reference Sans Serif;MS Sans Serif;MS Serif;MYRIAD;MYRIAD PRO;Palatino;Palatino Linotype;Segoe Print;Segoe Script;Segoe UI;Segoe UI Light;Segoe UI Semibold;Segoe UI Symbol;Tahoma;Times;Times New Roman;Trebuchet MS;Verdana;Wingdings;Wingdings 2;Wingdings 3".split(";"),
          g = "Abadi MT Condensed Light;Academy Engraved LET;ADOBE CASLON PRO;Adobe Garamond;ADOBE GARAMOND PRO;Agency FB;Aharoni;Albertus Extra Bold;Albertus Medium;Algerian;Amazone BT;American Typewriter;American Typewriter Condensed;AmerType Md BT;Andalus;Angsana New;AngsanaUPC;Antique Olive;Aparajita;Apple Chancery;Apple Color Emoji;Apple SD Gothic Neo;Arabic Typesetting;ARCHER;ARNO PRO;Arrus BT;Aurora Cn BT;AvantGarde Bk BT;AvantGarde Md BT;AVENIR;Ayuthaya;Bandy;Bangla Sangam MN;Bank Gothic;BankGothic Md BT;Baskerville;Baskerville Old Face;Batang;BatangChe;Bauer Bodoni;Bauhaus 93;Bazooka;Bell MT;Bembo;Benguiat Bk BT;Berlin Sans FB;Berlin Sans FB Demi;Bernard MT Condensed;BernhardFashion BT;BernhardMod BT;Big Caslon;BinnerD;Blackadder ITC;BlairMdITC TT;Bodoni 72;Bodoni 72 Oldstyle;Bodoni 72 Smallcaps;Bodoni MT;Bodoni MT Black;Bodoni MT Condensed;Bodoni MT Poster Compressed;Bookshelf Symbol 7;Boulder;Bradley Hand;Bradley Hand ITC;Bremen Bd BT;Britannic Bold;Broadway;Browallia New;BrowalliaUPC;Brush Script MT;Californian FB;Calisto MT;Calligrapher;Candara;CaslonOpnface BT;Castellar;Centaur;Cezanne;CG Omega;CG Times;Chalkboard;Chalkboard SE;Chalkduster;Charlesworth;Charter Bd BT;Charter BT;Chaucer;ChelthmITC Bk BT;Chiller;Clarendon;Clarendon Condensed;CloisterBlack BT;Cochin;Colonna MT;Constantia;Cooper Black;Copperplate;Copperplate Gothic;Copperplate Gothic Bold;Copperplate Gothic Light;CopperplGoth Bd BT;Corbel;Cordia New;CordiaUPC;Cornerstone;Coronet;Cuckoo;Curlz MT;DaunPenh;Dauphin;David;DB LCD Temp;DELICIOUS;Denmark;DFKai-SB;Didot;DilleniaUPC;DIN;DokChampa;Dotum;DotumChe;Ebrima;Edwardian Script ITC;Elephant;English 111 Vivace BT;Engravers MT;EngraversGothic BT;Eras Bold ITC;Eras Demi ITC;Eras Light ITC;Eras Medium ITC;EucrosiaUPC;Euphemia;Euphemia UCAS;EUROSTILE;Exotc350 Bd BT;FangSong;Felix Titling;Fixedsys;FONTIN;Footlight MT Light;Forte;FrankRuehl;Fransiscan;Freefrm721 Blk BT;FreesiaUPC;Freestyle Script;French Script MT;FrnkGothITC Bk BT;Fruitger;FRUTIGER;Futura;Futura Bk BT;Futura Lt BT;Futura Md BT;Futura ZBlk BT;FuturaBlack BT;Gabriola;Galliard BT;Gautami;Geeza Pro;Geometr231 BT;Geometr231 Hv BT;Geometr231 Lt BT;GeoSlab 703 Lt BT;GeoSlab 703 XBd BT;Gigi;Gill Sans;Gill Sans MT;Gill Sans MT Condensed;Gill Sans MT Ext Condensed Bold;Gill Sans Ultra Bold;Gill Sans Ultra Bold Condensed;Gisha;Gloucester MT Extra Condensed;GOTHAM;GOTHAM BOLD;Goudy Old Style;Goudy Stout;GoudyHandtooled BT;GoudyOLSt BT;Gujarati Sangam MN;Gulim;GulimChe;Gungsuh;GungsuhChe;Gurmukhi MN;Haettenschweiler;Harlow Solid Italic;Harrington;Heather;Heiti SC;Heiti TC;HELV;Herald;High Tower Text;Hiragino Kaku Gothic ProN;Hiragino Mincho ProN;Hoefler Text;Humanst 521 Cn BT;Humanst521 BT;Humanst521 Lt BT;Imprint MT Shadow;Incised901 Bd BT;Incised901 BT;Incised901 Lt BT;INCONSOLATA;Informal Roman;Informal011 BT;INTERSTATE;IrisUPC;Iskoola Pota;JasmineUPC;Jazz LET;Jenson;Jester;Jokerman;Juice ITC;Kabel Bk BT;Kabel Ult BT;Kailasa;KaiTi;Kalinga;Kannada Sangam MN;Kartika;Kaufmann Bd BT;Kaufmann BT;Khmer UI;KodchiangUPC;Kokila;Korinna BT;Kristen ITC;Krungthep;Kunstler Script;Lao UI;Latha;Leelawadee;Letter Gothic;Levenim MT;LilyUPC;Lithograph;Lithograph Light;Long Island;Lydian BT;Magneto;Maiandra GD;Malayalam Sangam MN;Malgun Gothic;Mangal;Marigold;Marion;Marker Felt;Market;Marlett;Matisse ITC;Matura MT Script Capitals;Meiryo;Meiryo UI;Microsoft Himalaya;Microsoft JhengHei;Microsoft New Tai Lue;Microsoft PhagsPa;Microsoft Tai Le;Microsoft Uighur;Microsoft YaHei;Microsoft Yi Baiti;MingLiU;MingLiU_HKSCS;MingLiU_HKSCS-ExtB;MingLiU-ExtB;Minion;Minion Pro;Miriam;Miriam Fixed;Mistral;Modern;Modern No. 20;Mona Lisa Solid ITC TT;Mongolian Baiti;MONO;MoolBoran;Mrs Eaves;MS LineDraw;MS Mincho;MS PMincho;MS Reference Specialty;MS UI Gothic;MT Extra;MUSEO;MV Boli;Nadeem;Narkisim;NEVIS;News Gothic;News GothicMT;NewsGoth BT;Niagara Engraved;Niagara Solid;Noteworthy;NSimSun;Nyala;OCR A Extended;Old Century;Old English Text MT;Onyx;Onyx BT;OPTIMA;Oriya Sangam MN;OSAKA;OzHandicraft BT;Palace Script MT;Papyrus;Parchment;Party LET;Pegasus;Perpetua;Perpetua Titling MT;PetitaBold;Pickwick;Plantagenet Cherokee;Playbill;PMingLiU;PMingLiU-ExtB;Poor Richard;Poster;PosterBodoni BT;PRINCETOWN LET;Pristina;PTBarnum BT;Pythagoras;Raavi;Rage Italic;Ravie;Ribbon131 Bd BT;Rockwell;Rockwell Condensed;Rockwell Extra Bold;Rod;Roman;Sakkal Majalla;Santa Fe LET;Savoye LET;Sceptre;Script;Script MT Bold;SCRIPTINA;Serifa;Serifa BT;Serifa Th BT;ShelleyVolante BT;Sherwood;Shonar Bangla;Showcard Gothic;Shruti;Signboard;SILKSCREEN;SimHei;Simplified Arabic;Simplified Arabic Fixed;SimSun;SimSun-ExtB;Sinhala Sangam MN;Sketch Rockwell;Skia;Small Fonts;Snap ITC;Snell Roundhand;Socket;Souvenir Lt BT;Staccato222 BT;Steamer;Stencil;Storybook;Styllo;Subway;Swis721 BlkEx BT;Swiss911 XCm BT;Sylfaen;Synchro LET;System;Tamil Sangam MN;Technical;Teletype;Telugu Sangam MN;Tempus Sans ITC;Terminal;Thonburi;Traditional Arabic;Trajan;TRAJAN PRO;Tristan;Tubular;Tunga;Tw Cen MT;Tw Cen MT Condensed;Tw Cen MT Condensed Extra Bold;TypoUpright BT;Unicorn;Univers;Univers CE 55 Medium;Univers Condensed;Utsaah;Vagabond;Vani;Vijaya;Viner Hand ITC;VisualUI;Vivaldi;Vladimir Script;Vrinda;Westminster;WHITNEY;Wide Latin;ZapfEllipt BT;ZapfHumnst BT;ZapfHumnst Dm BT;Zapfino;Zurich BlkEx BT;Zurich Ex BT;ZWAdobeF".split(";");
        for (var f = f.concat([]), g = document.getElementsByTagName("body")[0], p = document.createElement("div"), h = document.createElement("div"), m = {}, l = {}, k = [], n = 0, q = e.length; n < q; n++) {
          var r = d();
          r.style.fontFamily = e[n];
          p.appendChild(r);
          k.push(r)
        }
        g.appendChild(p);
        n = 0;
        for (q = e.length; n < q; n++)
          m[e[n]] = k[n].offsetWidth,
          l[e[n]] = k[n].offsetHeight;
        k = {};
        n = 0;
        for (q = f.length; n < q; n++) {
          for (var r = [], t = 0, v = e.length; t < v; t++) {
            var x;
            x = f[n];
            var z = e[t],
              B = d();
            B.style.fontFamily = "'" + x + "'," + z;
            x = B;
            h.appendChild(x);
            r.push(x)
          }
          k[f[n]] = r
        }
        g.appendChild(h);
        n = [];
        q = 0;
        for (r = f.length; q < r; q++) {
          t = k[f[q]];
          v = !1;
          for (x = 0; x < e.length && !(v = t[x].offsetWidth !== m[e[x]] || t[x].offsetHeight !== l[e[x]]); x++)
          ;
          v && n.push(f[q])
        }
        g.removeChild(h);
        g.removeChild(p);

        return n
      }
      return [
        "Andale Mono",
        "Arial",
        "Arial Black",
        "Arial Hebrew",
        "Arial Narrow",
        "Arial Rounded MT Bold",
        "Arial Unicode MS",
        "Comic Sans MS",
        "Courier",
        "Courier New",
        "Geneva",
        "Georgia",
        "Helvetica",
        "Helvetica Neue",
        "Impact",
        "LUCIDA GRANDE",
        "Microsoft Sans Serif",
        "Monaco",
        "Palatino",
        "Tahoma",
        "Times",
        "Times New Roman",
        "Trebuchet MS",
        "Verdana",
        "Wingdings",
        "Wingdings 2",
        "Wingdings 3"
      ]
    }
}
```



## 源码下载

## 回顾总结

## 启发感想
