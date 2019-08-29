# react-native-kakao-logins

<p align="left">
  <a href="https://npmjs.org/package/react-native-kakao-logins">
    <img alt="npm version" src="http://img.shields.io/npm/v/react-native-kakao-logins.svg?style=flat-square">
  </a>
  <a href="https://npmjs.org/package/react-native-kakao-logins">
    <img src="http://img.shields.io/npm/dm/react-native-kakao-logins.svg?style=flat-square">
  </a>
  <a href="https://npmjs.org/package/react-native-kakao-logins">
    <img src="http://img.shields.io/npm/l/react-native-kakao-logins.svg?style=flat-square">
  </a>
</p>
React Native 카카오 로그인 라이브러리 입니다.
세부 예제는 KakaoLoginExample 폴더 안의 예제 프로젝트를 확인해주시면 감사하겠습니다.
`react-native-kakao-login`과 `react-native-kakao-signin`이 관리가 안되고 오래되어 최신 버전 kakao sdk로 새로 만들었습니다.


[readme-deprecated]: /README_DEPRECATED.md

## [Deprecated README][readme-deprecated]
If you are using RN < 0.60, please follow the above README.

## Getting started

`$ npm install react-native-kakao-logins --save`

### Mostly automatic installation

`$ react-native link react-native-kakao-logins`

### Manual installation

#### iOS

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-kakao-logins` and add `RNKakaoLogins.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNKakaoLogins.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Add paths as below
   - Header Search Paths: `"$(SRCROOT)/../node_modules/react-native-kakao-logins/ios/**"`
   - Framework search Paths `"$(SRCROOT)/../node_modules/react-native-kakao-logins/ios/Frameworks"`
5. Refer to `Post installation`

#### Android

1. Open up `android/app/src/main/java/[...]/MainApplication.java`

   - Add `import com.dooboolab.kakaologins.RNKakaoLoginsPackage;` to the imports at the top of the file
   - Add `new RNKakaoLoginsPackage()` to the list returned by the `getPackages()` method

2. Append the following lines to `android/settings.gradle`:
   ```
   include ':react-native-kakao-logins'
   project(':react-native-kakao-logins').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-kakao-logins/android')
   ```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
   ```
     implementation project(':react-native-kakao-logins')
   ```

### Post installation

#### iOS

1. xcode를 열고 Libraries 안에 있는 KakaoOpenSDK.framework를 project의 `Framework`폴더 안으로 복사합니다.
2. ios 카카오 sdk 설치 후의 설정과 관련해서는 [카카오 개발자 페이지 - 앱생성](https://developers.kakao.com/docs/ios/getting-started#앱-생성)을 참고해주세요. <b>앱생성</b> 가이드를 따라하고 성공적으로 build가 되는 것을 확인하시면 아래를 진행하시면 됩니다.
3. Project => Targets 아래 앱 선택 => General 탭으로 이동해서 Bundle Identifier가 본인의 카카오 앱과 동일한지 확인해주세요.
4. [SDK의 공식문서](https://developers.kakao.com/docs/ios/user-management#%EB%A1%9C%EA%B7%B8%EC%9D%B8)를 참조하여 `AppDelegate.m` 파일에 아래와 같은 내용을 추가합니다.

   ```
   #import <KakaoOpenSDK/KakaoOpenSDK.h>

   - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url
                                         sourceApplication:(NSString *)sourceApplication
                                                 annotation:(id)annotation {
       if ([KOSession isKakaoAccountLoginCallback:url]) {
           return [KOSession handleOpenURL:url];
       }

       return false;
   }

   - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url
                                                   options:(NSDictionary<NSString *,id> *)options {
       if ([KOSession isKakaoAccountLoginCallback:url]) {
           return [KOSession handleOpenURL:url];
       }

       return false;
   }

   - (void)applicationDidBecomeActive:(UIApplication *)application
   {
       [KOSession handleDidBecomeActive];
   }
   ```

5. Expo Kit을 사용하여 개발하는 경우 `RNNaverLogin.xcodeproj`의 Build Settings => Header Search Paths에 `$(SRCROOT)/../../../ios/Pods/Headers/Public`를 `recursive`로 추가해주셔야 합니다.

6. 잘 안되시면 Example Project를 확인하여 비교해보시면 되겠습니다.

#### Android

1. 안드로이드에서는 카카오 SDK가 모듈의 gradle 경로에 잡혀있어서 별도로 sdk를 설치하지 않아도 됩니다.
2. manifest 파일에서 allowBackup을 true로 설정해주세요.
3. 안드로이드 카카오 SDK 설치 후의 설정과 관련해서는 [카카오 개발자 페이지 - 앱생성](https://developers.kakao.com/docs/android/getting-started#앱-생성)을 참고해주세요. <b>앱생성</b> 가이드를 따라하고 성공적으로 build가 되는 것을 확인하시면 아래를 진행하시면 됩니다.
4. `app/src/main/res/values/strings.xml` 을 열어 `kakao_app_key` 에 본인의 application key를 등록합니다.
```xml
<resources>
    <string name="app_name">KakaoLoginExample</string>
    <string name="kakao_app_key">your_app_key</string>
</resources>
```
5. 컴파일 에러가 나면 `build.gradle`에서 android sdk compile version 등 빌드 sdk 버전을 맞춰주세요.
6. 아래와 같은 에러가 발생할 경우 [키 해시 등록](https://developers.kakao.com/docs/android/getting-started#키해시-등록)을 진행해주세요. 자바 코드로 구하는 방법이 제일 확실합니다.
```
AUTHORIZATION_FAILED: invalid android_key_hash or ios_bundle_id or web_site_url
```
|React Native 0.60.x 부터 기본적으로 들어있는 디버깅 키의 해시는 다음과 같습니다.
`Xo8WBi6jzSxKDVR4drqm84yr9iU=`

## Changelogs

- **[0.3.0]**
  - 앱 깔려있을 때 튕기는 버그 수정. [issue](https://github.com/react-native-seoul/react-native-kakao-logins/issues/6).

#### Methods

| Func       | Param |                         Return                         | Description      |
| :--------- | :---: | :----------------------------------------------------: | :--------------- |
| login      |       | `callback (err: string, result: JSONObject in string)` | 로그인.          |
| getProfile |       | `callback (err: string, result: JSONObject in string)` | 프로필 불러오기. |
| logout     |       |        `callback (err: string, result: string)`        | 로그아웃.        |

#### params in result when `getProfile`

|                      | iOS | Android | Comment            |
| -------------------- | --- | ------- | ------------------ |
| `id`                 | ✓   | ✓       | 카카오 고유 아이디 |
| `nickname`           | ✓   | ✓       | 별칭               |
| `email`              | ✓   | ✓       | 이메일 주소        |
| `display_id`         |     | ✓       | 별칭 id            |
| `phone_number`       |     | ✓       | 휴대폰 번호        |
| `email_verified`     | ✓   | ✓       | 이메일 인증 여부   |
| `kakaotalk_user`     |     | ✓       | 카카오톡 유저 여부 |
| `profile_image_path` | ✓   | ✓       | 프로필 이미지      |
| `thumb_image_path`   | ✓   | ✓       | 썸네일 이미지      |
| `has_signed_up`      |     | ✓       | 가입 여부          |

- 4가지 attribute 대해 아직 ios에서 아직 어떻게 받는지 확인이 안되어 `android`와 상이한 부분이 있습니다.

## Usage

아래 예제는 `KakaoLoginExample` 프로젝트의 `App.js`파일과 동일합니다. 로그인 후 result에 들어오는 결과값은 `{token:kakao_token}`입니다.

```javascript
import React, {useState} from 'react';
import RNKakaoLogins from 'react-native-kakao-logins';

export default App = () => {
    const [loginLoading, setLoginLoading] = useState(false);
    const [logoutLoading, setLogoutLoading] = useState(false);
    const [profileLoading, setProfileLoading] = useState(false);

    const [token, setToken] = useState(TOKEN_EMPTY);
    const [profile, setProfile] = useState(PROFILE_EMPTY);

    const kakaoLogin = () => {
        logCallback('Login Start', setLoginLoading(true));

        RNKakaoLogins.login((err, result) => {
            if (err){
                return logCallback(`Login Failed:${err.toString()}`, setLoginLoading(false));
            }
            setToken(result.token);
            logCallback(`Login Finished:${result.token}`, setLoginLoading(false));
        });
    };

    const kakaoLogout = () => {
        logCallback('Logout Start', setLogoutLoading(true));

        RNKakaoLogins.logout((err, result) => {
            if (err){
                return logCallback(`Logout Failed:${err.toString()}`, setLogoutLoading(false));
            }
            setToken(TOKEN_EMPTY);
            setProfile(PROFILE_EMPTY);
            logCallback(`Logout Finished:${result}`, setLogoutLoading(false));
        });
    };

    const getProfile = () => {
        logCallback('Get Profile Start', setProfileLoading(true));

        RNKakaoLogins.getProfile((err, result) => {
            if (err){
                return logCallback(`Get Profile Failed:${err.toString()}`, setProfileLoading(false));
            }
            setProfile(result);
            logCallback(`Get Profile Finished:${JSON.stringify(result)}`, setProfileLoading(false));
        });
    };

    const {id, email, profile_image_path:photo} = profile;

    return (
        <View style={ styles.container }>
            <View style={styles.profile}>
                <Image
                    style={styles.profilePhoto}
                    source={{uri:photo}}
                />
                <Text>{`id : ${id}`}</Text>
                <Text>{`email : ${email}`}</Text>
            </View>
            <View style={ styles.content}>
                <Text style={styles.token}>{token}</Text>
                <NativeButton
                    isLoading={loginLoading}
                    onPress={kakaoLogin}
                    activeOpacity={0.5}
                    style={styles.btnKakaoLogin}
                    textStyle={styles.txtKakaoLogin}
                >
                    LOGIN
                </NativeButton>
                <NativeButton
                    isLoading={logoutLoading}
                    onPress={kakaoLogout}
                    activeOpacity={0.5}
                    style={styles.btnKakaoLogin}
                    textStyle={styles.txtKakaoLogin}
                >
                    Logout
                </NativeButton>
                <NativeButton
                    isLoading={profileLoading}
                    onPress={getProfile}
                    activeOpacity={0.5}
                    style={styles.btnKakaoLogin}
                    textStyle={styles.txtKakaoLogin}
                >
                    getProfile
                </NativeButton>
            </View>
        </View>
    );
}
```
