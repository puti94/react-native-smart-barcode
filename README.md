# react-native-smart-barcode

[![npm](https://img.shields.io/npm/v/react-native-smart-barcode.svg)](https://www.npmjs.com/package/react-native-smart-barcode)
[![npm](https://img.shields.io/npm/dm/react-native-smart-barcode.svg)](https://www.npmjs.com/package/react-native-smart-barcode)
[![npm](https://img.shields.io/npm/dt/react-native-smart-barcode.svg)](https://www.npmjs.com/package/react-native-smart-barcode)
[![npm](https://img.shields.io/npm/l/react-native-smart-barcode.svg)](https://github.com/react-native-component/react-native-smart-barcode/blob/master/LICENSE)

A smart barcode scanner component for React Native app.
The library uses [https://github.com/zxing/zxing][1] to decode the barcodes for android, and also supports ios.

## Preview

![react-native-smart-barcode-preview-ios][2]

## Installation

```
yarn add react-native-smart-barcode@https://github.com/puti94/react-native-smart-barcode.git
```

## Notice

It can only be used greater-than-equal react-native 0.4.0 for ios, if you want to use the package less-than react-native 0.4.0, use `npm install react-native-smart-barcode@untilRN0.40 --save`
## AutoInstallation

```
react-native link react-native-smart-barcode
```
Add `Privacy - Camera Usage Description` `Privacy - Photo Library Usage Description` property in your info.plist(for ios 10)

* In AndroidManifest.xml, add camera permissions
```
...
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.VIBRATE"/>

<uses-feature android:name="android.hardware.camera"/>
<uses-feature android:name="android.hardware.camera.autofocus"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
...
```


## Full Demo

see [ReactNativeComponentDemos][0]

## Usage

Install the package from npm with `npm install react-native-smart-barcode --save`.
Then, require it from your app's JavaScript files with `import Barcode,{BarcodeUtil} from 'react-native-smart-barcode'`.

```js
// 没有权限需要自己申请，推荐react-native-permissions 这个库
//从相册选取二维码 没有则返回""
BarcodeUtil.decodePictureFromPhotos().then(code=>console.log(code))
//切换闪光灯
BarcodeUtil.switchFlashLightState(true or false)

```

```js


import React, {
    Component,
} from 'react'
import {
    View,
    StyleSheet,
    Alert,
} from 'react-native'

import Barcode from 'react-native-smart-barcode'
import TimerEnhance from 'react-native-smart-timer-enhance'

class BarcodeTest extends Component {

    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            viewAppear: false,
        };
    }

    render() {

        return (
            <View style={{flex: 1, backgroundColor: 'black',}}>
                {this.state.viewAppear ? <Barcode style={{flex: 1, }}
                                                  ref={ component => this._barCode = component }
                                                  onBarCodeRead={this._onBarCodeRead}/> : null}
            </View>
        )
    }

    componentDidMount() {
        let viewAppearCallBack = (event) => {
            this.setTimeout( () => {
                this.setState({
                    viewAppear: true,
                })
            }, 255)

        }
        this._listeners = [
            this.props.navigator.navigationContext.addListener('didfocus', viewAppearCallBack)
        ]

    }

    componentWillUnmount () {
        this._listeners && this._listeners.forEach(listener => listener.remove());
    }

    _onBarCodeRead = (e) => {
        console.log(`e.nativeEvent.data.type = ${e.nativeEvent.data.type}, e.nativeEvent.data.code = ${e.nativeEvent.data.code}`)
        this._stopScan()
        Alert.alert(e.nativeEvent.data.type, e.nativeEvent.data.code, [
            {text: 'OK', onPress: () => this._startScan()},
        ])
    }

    _startScan = (e) => {
        this._barCode.startScan()
    }

    _stopScan = (e) => {
        this._barCode.stopScan()
    }

}

export default TimerEnhance(BarcodeTest)
```

## Props

Prop                   | Type   | Optional | Default   | Description
---------------------- | ------ | -------- | --------- | -----------
barCodeTypes           | array  | Yes      |           | determines the supported barcodeTypes
scannerRectWidth       | number | Yes      | 255       | determines the width of scannerRect
scannerRectHeight      | number | Yes      | 255       | determines the height of scannerRect
scannerRectTop         | number | Yes      | 0         | determines the top shift of scannerRect
scannerRectLeft        | number | Yes      | 0         | determines the left shift of scannerRect
scannerLineInterval    | number | Yes      | 3000      | determines the interval of scannerLine's movement
scannerRectCornerColor | string | Yes      | `#09BB0D` | determines the color of scannerRectCorner

[0]: https://github.com/cyqresig/ReactNativeComponentDemos
[1]: https://github.com/zxing/zxing
[2]: http://cyqresig.github.io/img/react-native-smart-barcode-preview-ios-v1.0.0.gif
[3]: http://cyqresig.github.io/img/react-native-smart-barcode-preview-android-v1.0.0.gif
