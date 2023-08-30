---
layout: post
title: 웹팩을 활용한 리액트 프로젝트 구성 - 빌드 시스템의 이해와 초기 구성
date: 2023-08-07 18:47:00 +0900
categories: [Developer, React]
tags: [Webpack, React, Babel]
published: true
---

## 리액트 프로젝트를 위한 빌드 시스템의 이해와 중요성
---
CRA(create-react-app)는 리액트(React) 애플리케이션을 쉽고 빠르게 생성하고 설정할 수 있는 공식적인 도구다.
이는 리액트 프로젝트를 시작하고 설정하는데 드는 작업을 최소화하고 최적화에 필요한 개발 환경을 제공한다.
하지만 CRA는 높은 추상화 단계 때문에 흐름을 알기 어렵고, 빌드 시스템을 모르고 CRA로 만들어진 프로젝트를 다룬다면 성능 저하, 브라우저 호환성 문제, 환경 분리 등 여러 문제가 발생할 수 있다.

### 빌드 시스템의 이해와 중요성
빌드 시스템은 프론트엔드 프로젝트 구성에서 빼놓을 수 없는 중요한 역할을 하는 도구들 집합의 일부로 애플리케이션의 소스 코드와 리소스를 효율적으로 관리하고 최적화하는 핵심 요소다. 이를 효과적으로 관리하고 최적화하기 위해 빌드 시스템을 이해하고 프로젝트를 구성하는 것은 다음과 같은 이유로 중요하다.

1. **커스터마이징 가능성**: CRA 같은 프로젝트 생성 도구들은 일반적인 설정을 제공하여 편리하지만, 프로젝트 성격에 따라 요구사항에 맞게 빌드 시스템을 커스터마이징해야 하는 경우가 생길 수 있다. 이러한 경우 CRA를 사용하는 것보다 직접 빌드 시스템을 구축했을 때 요구사항을 더욱 자세히 처리할 수 있다.

2. **최적화**: 웹 애플리케이션의 성능을 최적화하려면 코드 스플리팅, 번들 최소화, 이미지 최적화 등 다양한 작업이 필요하다. 이를 효과적으로 처리하기 위해 빌드 시스템을 이해하는 것이 중요하다.

3. **새로운 도구와 기술 통합**: 프론트엔드 개발 분야는 빠르게 변화하고 새로운 도구와 기술이 등장하기 때문에 우리는 새로운 기술과 트렌드를 적극적으로 받아들이고 적용해야 한다. 빌드 시스템을 직접 구축하면 새로운 도구와 기술을 더 쉽게 통합하고 적용할 수 있다.

**물론** CRA로 프로젝트를 생성한다고 위의 모든 사항이 불가하거나 불편한 것은 아니다. 하지만 우리가 **직접 프로젝트를 구성하는 것만큼 빌드 시스템을 이해하는 데 효과적인 것은 없을 것이다.** 이는 더 복잡한 프로젝트를 다룰 때 더 유용할 것이고, CRA를 사용하더라도 내부 동작을 이해하는 데 도움이 될 것이다.

<br/>

### 초기 구성에 필요한 요소와 도구 
**이 포스팅에서는 앞으로 웹팩을 중심으로 프로젝트를 구성하는 데 필요한 초기 구성에 관해 설명한다.** 필요한 요소와 도구는 다음과 같다.

1. **모듈 번들러(웹팩)**: 대표적으로 Webpack이나 Parcel이 있으며 여러 파일을 하나의 번들 파일로 묶는 역할을 한다. 이 포스팅에서는 웹팩에 대해서 다룬다.
2. **사용자 인터페이스(리액트)**: 사용자 인터페이스는 웹 애플리케이션이 사용자와 상호작용하는 요소들로 우리는 리액트로 프로젝트를 구성한다. 웹팩의 번들링 대상이 된다.
3. **트랜스파일러(바벨)**: 최신 Javascript 문법을 구형 브라우저에서도 동작할 수 있는 코드로 변환해 주는 역할을 한다. 일반적으로 웹팩과 리액트에서는 바벨을 사용한다.
4. **로더(Loader)와 플러그인(Plugin)**: 웹팩이 번들링 할 때 로더는 여러 파일 형식에 대한 전처리, 플러그인은 번들링된 파일에 대한 추가 작업을 수행한다.
5. **개발 서버**: 개발 중에 코드 수정 시 실시간으로 변경 사항을 확인할 수 있게 해준다.

<br/>

[웹팩 러닝 가이드](https://yamoo9.gitbook.io/webpack/react/create-your-own-react-app)를 참고하였다. 해당 링크는 웹팩 4를 기준으로 하고 있으며, **이 포스팅은 웹팩 5를 기준으로 서술하였다. 이에 따라 모든 과정이 일치하지 않음을 참고바란다.**

<br/>

## 1. 모듈 번들러(웹팩)
---
웹 애플리케이션을 구성하는 리소스들을 모듈 단위로 분리하고, 이를 효율적으로 묶는 것을 모듈 번들링이라고 한다. 웹팩은 이런한 모듈 번들링을 수행하는 도구로, 의존성 관리와 최적화를 도와준다.


### 1.1 프로젝트 초기화 및 웹팩 패키지 설치
프로젝트 디렉토리에서 다음 명령어를 입력하여 프로젝트를 초기화하고 웹팩 패키지를 설치한다.

```console
mkdir react-app-by-webpack
cd react-app-by-webpack
npm init -y 
npm i webpack -D
```

<br/>

### 1.2 웹팩 설정 파일 생성
웹팩의 동작 방식과 빌드 옵션을 제어하는 파일이다. 일반적으로 webpack-cli를 사용하여 루트 디렉토리에 있지만 우리는 webpack-cli 없이 별도의 빌드 스크립트를 작성할 것이기 때문에 config 디렉토리로 분리하였다. 자세한 설명은 [여기](https://webpack.kr/configuration/)를 참고 바란다.

- /config/webpack.config.js

```js
const path = require( 'path' )

const appDir = process.cwd()
const getAbsolutePath = pathDir => path.resolve( appDir, pathDir )

module.exports = () => {
  return {
    entry: { //프로젝트의 진입점(entry point)
      main: './src/index.js',
    },
    output: { //번들링된 결과물 출력
      path: getAbsolutePath( 'dist' ),
      filename: 'assets/js/[name].[contenthash:8].js',
      publicPath: '/',
    },
    resolve: { //모듈을 해석하고 찾는 방식
      extensions: ['.js', '.jsx', '.json'],
      alias: {
        '@components': getAbsolutePath( 'src/components/' ),
        '@contexts': getAbsolutePath( 'src/contexts/' ),
        '@hooks': getAbsolutePath( 'src/hooks/' ),
        '@pages': getAbsolutePath( 'src/pages/' ),
        '@assets': getAbsolutePath( 'src/assets/' )
      },
    },
  }
}
```
1. **entry**: 웹팩은 애플리케이션의 시작점을 정의한 후, 해당 모듈에서부터 시작하여 **의존성 그래프**를 생성하여 번들링을 진행한다. 그 시작점을 Entry Point라 한다.
2. **ouput**: 번들링된 최종 결과물을 지정된 경로와 이름으로 출력한다.
3. **resolve**: 확장자를 제외하고 import를 했다거나, 별칭을 사용하여 import 경로를 지정하는 등의 모듈을 해석하고 찾는 방식을 설정할 수 있다.

<br/>

### 1.3 Enty Point 생성
웹팩 설정 파일의 entry에 /src/index.js라고 설정하였다. 프로젝트의 진입점으로 번들링의 시작점이 될 파일을 생성한다.

- /src/index.js

```js
console.log( 'Hello Word!' )
```

### 1.4 빌드 스크립트 생성
웹팩 빌드를 실행시킬 스크립트를 생성한다. 

- /scripts/build.js

```js
const webpack = require('webpack')
const configFactory = require( '../config/webpack.config' )

const mode = 'production'
const webpackConfig = configFactory( mode )

webpack( webpackConfig, ( err, stats ) => {
  if( err || stats.hasErrors() ) {
    console.error( err || stats.toString() )
    return
  }
  console.log( stats.toString() )
} )
```

<br/>

### 1.5 빌드 명령어 추가
package.json에 다음 명령을 추가한다.

- /package.json

```json
...
  "scripts": {
    "build": "node scripts/build.js"
  },
...
```

### 1.6 빌드 실행
다음 명령어을 입력하여 빌드를 실행하고 결과를 확인한다.

```console
npm run build
```

우리는 웹팩 설정 파일의 output에 dist 디렉토리 안에 빌드된 결과물이 들어가도록 설정하였다. 빌드된 결과물이 dist 디렉토리 안에 생성되었다.

![react_app_by_webpack(1.6)](/assets/img/posts/react_app_by_webpack(1.6).png)

<br/>

## 2.사용자 인터페이스(리액트)
---
프로젝트 구성에서 사용자 인터페이스를 구축하기 위한 라이브러리다. 빌드 시스템에서 웹팩은 이를 UI 구성요소로 번들에 포함할 것이다.

### 2.1 리액트 패키지 설치
다음 명령어를 입력하여 리액트 패키지를 설치한다.

```console
npm i react react-dom 
```

### 2.2 웹 애플리케이션 진입점 생성
브라우저가 처음으로 로딩할 때 보여지는 페이지의 기본 구조를 정의하고, 리액트가 UI를 구성할 HTML을 생성한다.

- /public/index.html

```html
<!DOCTYPE html>
<html lang="ko-KR">

  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App 매뉴얼 구성</title>
  </head>

  <body>
    <div id="root"></div>
  </body>
</html>
```

<br/>

### 2.3 리액트 주입
index.js를 다음과 같이 수정하고 app.js를 생성하여 생성한 index.html에 리액트 컴포넌트를 실제로 DOM에 렌더링하도록 한다.

- /src/index.js

```js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './app'

//DOM 노드
const root = ReactDOM.createRoot(
  document.getElementById( 'root' )
)

//DOM 노드에 React 요소 렌더링
root.render(
  <React.StrictMode>
    <App/>
  </React.StrictMode>
)
```

- /src/app.js

```js
function App() {
  return (
    <div className="app">React App 매뉴얼 구성</div>
  )
}

export default App
```

<br/>

리액트에서 사용되는 JSX 문법은 기본적으로 브라우저가 이해하지 못한다. 따라서 트랜스파일러를 사용하여 JSX 문법을 일반적인 자바스크립트로 변환해야 한다. 

<br/>

## 3.트랜스파일러(바벨)
---
바벨(Babel)은 자바스크립트 트랜스파일러로, 최신 자바스크립트를 구형 브라우저에서도 호환되게 하거나, 리액트의 JSX의 문법을 브라우저에서 실핼할 수 있는 파일로 변경한다.

### 3.1 바벨 패키지 설치
다음 명령어를 입력하여 필요한 패키지를 설치한다.

```console
npm i babel-loader @babel/core @babel/preset-env @babel/preset-react -D
```

1. **babel-loader**: 웹팩 빌드 과정에서 자바스크립트 파일을 잃어와 바벨 변환을 자동으로 처리한다. 
2. **@babel/core**: 바벨의 핵심 엔진으로, 코드 변환 작업을 담당하며, 프리셋과 플러그인을 설정하여 변환 규칙을 지정한다.
3. **babel/preset-env**: 브라우저 호환성을 유지하면서 최신 문법을 사용할 수 있도록 해준다.
4. **@babel/preset-react**: 리액트의 JSX 문법을 브라우저에서 동작 동작할 수 있는 코드로 변환한다.

<br/>

### 3.2 바벨 설정 파일 생성
프로젝트에서 사용하는 바벨 설정 파일이다. 필요한 프리셋과 플러그인을 설정한다. 자세한 설정 방법은 [여기](https://babeljs.io/docs/configuration)를 참고바란다.

- /config/babel.config.js

```js
module.exports = {
  presets: [ //플러그인들의 집합인 프리셋을 설정한다. 
    [
      '@babel/preset-env',
      { modules: false }
    ],
    [ 
      "@babel/preset-react", 
      { runtime: "automatic" }
    ]
  ]
}
```

1.**플러그인**: 바벨이 코드를 어떤 방식으로 변환해야 하는지에 대한 규칙이다.\
2.**프리셋**: 플러그인을 묶어 정의한 플러그인의 모음이다.

<br/>

추가적으로 어떤 파일을 바벨을 통해 변환할 것인지를 설정 해줘야 한다. 이에 대한 설명은 하단에 로더와 플러그인에서 설명하겠다.

<br/>

## 4.로더와 플러그인
---
로더는 웹팩에서 다양한 파일 형식을 모듈로 취급하는데, 이를 웹팩이 이해할 수 있는 형태로 변환하기 위한 것이다. 예를 들어 JSX, CSS, 이미지, 폰트 등이 있다.
로더가 파일 단위의 변환을 담당한다면 플러그인은 번들링된 결과물에 추가적인 작업을 수행한다. 예를 들어 HTML을 생성하여 번들 파일을 자동으로 추가해주는 HthmlWebpackPlugin 등이 있다.

### 4.1 바벨 로더 구성
웹팩 설정에 다음 코드를 삽입하여 바벨이 해당 파일을 트랜스파일링할 수 있도록 한다.

- /config/webpack.config.js

```javascript

module.exports = ( webpackEnv ) => {
  const isProd = webpackEnv === 'production'
  
  return {
    ...
    module: {
      rules: [
        {
          test: /\.(js|jsx)$/,
          exclude: /node_modules/,
          use: [
            {
              loader: 'babel-loader',
              options: {
                cacheDirectory: true,
                cacheCompression: false,
                configFile: getAbsolutePath( 'config/babel.config.js' ),
                envName: isProd ? 'production' : 'development'
              }
            }
          ]
        }
      ]
    }
  }
}
```

<br/>

### 4.2 빌드 결과 자동 주입 구성
HTMLWebpackPlugin은 웹팩이 빌드 시 번들링된 자바스크립트 파일이 추가된 HTML을 생성하게 해주는 플러그인이다. 해당 명령어를 입력하고 웹팩 설정 파일에 해당 플러그인을 구성한다.

```console
npm i html-webpack-plugin -D
```

- /config/webpack.config.js

```js
...
const HtmlWebpackPlugin = require( 'html-webpack-plugin' )
...
module.exports = ( webpackEnv ) => {
  ...
  return {
    ...
    plugins: [ //빌드 과정을 확장, 추가 기능을 제공한다.
      new HtmlWebpackPlugin( {
        template: getAbsolutePath( 'public/index.html' ),
        inject: true
      } )
    ],
    ...
  }
}
```

<br/>

### 4.3 환경 변수 구성
웹팩 설정 파일을 수정하여 웹 애플리케이션의 환경 변수를 구성한다.

- /config/webpack.config.js

```js
...
const webpack = require('webpack')
...
module.exports = ( webpackEnv ) => {
  ...
  return {
    ...
    plugins: [ 
      new webpack.DefinePlugin( {
        'process.env': {
          'NODE_ENV': JSON.stringify( isProd ? 'production' : 'development' )
        } 
      } ),
      ...
    ],
    ...
  }
}

```

<br/>

### 4.4 빌드 자동 정리 구성
웹팩은 기본적으로 이전에 빌드 했던 결과물을 삭제해주지 않는다. 
CleanWebpackPlugin은 웹팩 빌드 시 이전 결과물을 자동으로 정리해주는 플러그인이다.
해당 명령어를 입력해 패키지를 설치하고, 웹팩 설정에 해당 플러그인을 구성한다.

```console
npm i clean-webpack-plugin -D
```

- /config/webpack/config.js

```js
...
const { CleanWebpackPlugin } = require( 'clean-webpack-plugin' )
...
module.exports = ( webpackEnv ) => {
  ...
  return {
    ...
    plugins: [
      ...
      new CleanWebpackPlugin( {
        // dry 기본 값: false
        // dry: true,
        // verbose 기본 값: false
        verbose: true,
        // cleanOnceBeforeBuildPatterns 기본 값: ['**/*']
        cleanOnceBeforeBuildPatterns: [
          '**/*',
          getAbsolutePath( 'dist/**/*' ) // dist 폴더 안의 모든 것을 지우도록 설정
        ]
      } ),
    ],
    ...
  }
}

```

```console
npm i clean-webpack-plugin -D
```

<br/>

### 4.5 CSS 스타일 패키지 설치
CSS를 웹팩에서 처리하기 위해서는 해당 로더와 플러그인이 필요하다. 아래의 명령어를 입력해 패키지를 설치한다.

```console
npm i style-loader css-loader mini-css-extract-plugin -D
```

1. **css-loader**: CSS 파일을 자바스크립트 모듈로 변환, 이를 웹팩 빌드 시 번들에 포함한다.
2. **style-loader**: CSS 모듈을 HTML에 `<style>` 태그로 주입한다. 주로 개발 모드에서 주로 사용된다.
3. **mini-css-extract-plugin**: CSS 모듈을 별도의 CSS 파일로 분리하여 관리하는 플러그인이다. 이를 연결하는 `<link>` 태그를 HTML에 주입한다. 

<br/>

### 4.6 CSS 로더, 플러그인 구성
CSS 파일을 css-loader로 읽어 자바스크립트로 변환하여 모듈로 번들에 포함해야 한다. 번들에 포함된 CSS 모듈을 개발 환경에서는 `<style>` 태그로, 프로덕션 환경에서는 별도의 CSS 파일로 추출 후 `<link>` 태그로 HTML에 주입해야 한다. 해당 로더와 플러그인을 아래와 같이 구성한다.

- /config/webpack.config.js

```js
const MiniCssExtractPlugin = require( 'mini-css-extract-plugin' )
...
module.exports = ( webpackEnv ) => {
  ...
  return {
    ...
    plugins: [
      new MiniCssExtractPlugin( {
        filename: 'assets/css/[name].[contenthash:8].css',
        chunkFilename: 'assets/css/[name].[contenthash:8].chunk.css',
      } ),
    ],
    module: {
      rules: [
        ...
        {
          test: /\.css$/i,
          use: [
            isProd ? MiniCssExtractPlugin.loader : 'style-loader',
            'css-loader',
          ],
        },
      ]
    }
  }
}
```

<br/>

### 4.7 웹 폰트, 이미지 로더 구성
웹팩 5는 웹 폰트, 이미지와 같은 정적 리소스를 읽어오는 **Asset Module** 기능을 제공한다. 아래와 같이 웹팩 설정을 수정한다. 자세한 설명은 [여기](https://webpack.kr/guides/asset-modules/)를 참고 바란다.

- /config/webpack.config.js
```js
...
module.exports = ( webpackEnv ) => {
  ...
  return {
    ...
    module: {
      rules: [
        ...
        // 이미지 로더
        {
          test: /\.(jpe?g|png|gif)$/i,
          type: 'asset/resource',
          generator: {
            filename: './assets/images/[name][hash:8][ext]',
          },
        },
        // 웹폰트 로더
        {
          test: /\.(woff|woff2|eot|ttf|otf)$/i,
          type: 'asset/resource',
          generator: {
            filename: './assets/fonts/[name][hash:8][ext]',
          },
        }
      ]
    }
  }
}
```

<br/>


### 4.8 CSS, 웹 폰트, 이미지 추가 및 환경 변수 확인
위에서 구성한 로더와 플러그인들의 결과를 확인하기 위해 필요한 웹 폰트와 이미지를 가져와 /src/assets 디렉토리 안에 넣어주고, 해당 파일에 맞게 구성한다.

![react_app_by_webpack(4.8.1)](/assets/img/posts/react_app_by_webpack(4.8.1).png)

나는 [NotoSansKR](https://fonts.google.com/noto/specimen/Noto+Sans+KR)를 다운로드 받았다.

- /src/assets/css/font.css

```css
@font-face {
  font-family: 'NotoSansKR';
  font-weight: 900;
  src: url('@assets/fonts/NotoSansKR/NotoSansKR-Black.otf') format('opentype');
}

@font-face {
  font-family: 'NotoSansKR';
  font-weight: 700;
  src: url('@assets/fonts/NotoSansKR/NotoSansKR-Bold.otf') format('opentype');
}

@font-face { 
  font-family: 'NotoSansKR';
  font-weight: 500;
  src: url('@assets/fonts/NotoSansKR/NotoSansKR-Medium.otf') format('opentype');
}

@font-face {
  font-family: 'NotoSansKR';
  font-weight: 400;
  src: url('@assets/fonts/NotoSansKR/NotoSansKR-Regular.otf') format('opentype');
}

@font-face { 
  font-family: 'NotoSansKR';
  font-weight: 300;
  src: url('@assets/fonts/NotoSansKR/NotoSansKR-Light.otf') format('opentype');
}

@font-face { 
  font-family: 'NotoSansKR';
  font-weight: 100;
  src: url('@assets/fonts/NotoSansKR/NotoSansKR-Thin.otf') format('opentype');
}

*:not(i) {
  font-family: 'NotoSansKR' !important;
}
```

- /src/index.css

```css
.app {
  color: red;
}

.black {
  font-weight: 900;
}

.bold {
  font-weight: 700;
}

.medium {
  font-weight: 500;
}

.regular {
  font-weight: 400;
}

.light {
  font-weight: 300;
}

.thin {
  font-weight: 100;
}

img {
  width: 450px;
}
```

- /src/index.js

```js
...
import '@assets/css/font.css'
import './index.css'

console.log( process.env )
...
```

- /src/app.js

```js
import img from '@assets/images/bt_trust_me.jpg'

function App() {
  return (
    <div className="app">
      <div className="black">React App 매뉴얼 구성</div>
      <div className="bold">React App 매뉴얼 구성</div>
      <div className="medium">React App 매뉴얼 구성</div>
      <div className="regular">React App 매뉴얼 구성</div>
      <div className="light">React App 매뉴얼 구성</div>
      <div className="thin">React App 매뉴얼 구성</div>
      <img src={img}/>
    </div>
  )
}
...
```

### 4.9 빌드 후 결과 확인
위에서 설정한 로더와 플러그인 구성의 결과를 확인하기 위해 다음 명령어를 입력하고, 결과를 확인한다.

```console
npm run build
```

- 빌드된 결과물이 index.html이 생성되었고, 이전 빌드 결과물이 삭제되었다.

<br/>

![react_app_by_webpack(4.8.2)](/assets/img/posts/react_app_by_webpack(4.8.2).png)

<br/>

- 환경 변수도 정상 적용되었다. 

<br/>

![react_app_by_webpack(4.8.6)](/assets/img/posts/react_app_by_webpack(4.8.6).png)

<br/>

- 위에서 설정한 CSS 로더와 플러그인이 적용되고, 폰트와 이미지도 정상적으로 적용되었다.

<br/>

![react_app_by_webpack(4.8.4)](/assets/img/posts/react_app_by_webpack(4.8.4).png)

<br/>

![react_app_by_webpack(4.8.3)](/assets/img/posts/react_app_by_webpack(4.8.3).png)

<br/>

## 5. 개발 서버
---
위에서 구성하면서 설정하고 빌드하여 확인하는 과정이 매우 귀찮았을 것이다. 개발 서버를 사용한다면 코드 수정 시 자동으로 다시 빌드하여 실시간으로 변경 사항을 확인할 수 있다.

### 5.1 모드와 소스맵 구성
mode 옵션은 빌드된 코드의 최적화 수준을 지정하는 설정으로 development, production, none 옵션을 제공한다.
\
devtool 옵션은 디버깅에 사용되는 소스맵을 생성하고 관리하는 방식을 설정한다. 제공하는 옵션이 많으므로 자세한 설정 정보는 [웹팩 설정 Devtool](https://webpack.kr/configuration/devtool/)를 참고바란다.

- /config/webpack.config.js

```js
...
module.exports = ( webpackEnv  ) => {
  ...
  return {
    ...
    mode: isProd ? 'production' : 'development',
    devtool: !isProd && 'eval-source-map', //소스맵을 생성하고 제어하기 위한 설정 
    ...
  }
}
```
1. **development**: 
- 개발 환경을 위한 모드로, 빠른 빌드와 디버깅을 위한 설정을 적용한다.
- 번들링 시 코드가 난독화 되지 않아 가독성이 높고, 디버깅이 유용한 코드를 유지한다.
- 소스맵을 생성하여 원본 소스 코드와 번들링된 코드 사이의 매핑을 제공한다.
2. **production**: 
- 프로덕션 환경을 위한 모드로, 최적화된 번들링과 압축을 통해 성능을 향상시킨다. 
- 코드가 최적화되며, 중복되는 코드를 제거하고 불필요한 부분을 최소화하여 번들 크기를 줄인다. 
- 소스맵을 일반적으로 제외되지만, 필요한 경우 제한적으로 소스맵을 사용할 수 있다.
3. **none**: 
- 모든 최적화를 비활성화하여 웹팩이 어떠한 변형도 가하지 않는다.
- 주로 사용자 정의 최적화 설정을 적용하거나 디버깅 목적으로 활용된다.

<br/>

### 5.2 개발 서버 패키지 설치
다음 명령어를 입력하여 웹팩에서 제공하는 개발 서버 패키지를 설치한다.

```console
npm i webpack-dev-server -D
```

<br/>

### 5.3 개발 서버 설정 파일 생성
개발 서버 설정 파일을 생성한다.

- /config/webpackDevServer.config.js

```js
const path = require( 'path' )
const appDir = process.cwd()
const getAbsolutePath = pathDir => path.resolve( appDir, pathDir )

module.exports = {
  static: {
    directory: getAbsolutePath( 'public' )
  },
  hot: true,
  compress: true,
  historyApiFallback: true,
  open: true
}
```

<br/>

### 5.4 개발 서버 실행 스크립트 생성
개발 서버를 실행 시킬 스크립트를 생성한다.

- /scripts/devServer.js

```js
const webpack = require('webpack')
const configFactory = require( '../config/webpack.config' )
const WebpackDevServer = require( 'webpack-dev-server' )
const devServerConfig = require( '../config/webpackDevServer.config' )

const mode = 'development'
const port = 3000

const webpackConfig = configFactory( mode )
const compiler = webpack( webpackConfig )
const server = new WebpackDevServer( compiler, { ...devServerConfig, port } )

server.startCallback( ( err ) => {
  if( err ) {
    console.log( err )
  }
} )
```

<br/>

### 5.5 개발 서버 실행 명령 추가
package.json에 다음 명령을 추가한다.

- /package.json

```json
...
  "scripts": {
    "dev": "node scripts/devServer.js",
    "build": "node scripts/build.js"
  },
...
```

<br/>

### 5.6 개발 서버 실행
다음 명령어를 웹팩 개발 서버를 실행하여 localhost:3000으로 접속하면 실시간으로 변경된 내용을 확인할 수 있다.

```console
npm run dev
```

<p>위에 사진이 개발 빌드 아래 사진이 프로덕션 빌드다. CSS 스타일 패키지 적용에서 했던 개발 구성이 정상 적용되었다.</p>
![react_app_by_wepback(5.6.1)](/assets/img/posts/react_app_by_wepback(5.6.1).png)

<br/>

![react_app_by_wepback(5.6.2)](/assets/img/posts/react_app_by_wepback(5.6.2).png)

<br/>

<p>위에 파일이 프로덕션 빌드, 아래 파일이 개발 빌드다. 용량이 확실히 차이 나는 것을 확인할 수 있다.</p>

![react_app_by_wepback(5.6.3)](/assets/img/posts/react_app_by_wepback(5.6.3).png)

<br/>

<p>소스맵 또한 정상 생성되었다.</p>

![react_app_by_wepback(5.6.4)](/assets/img/posts/react_app_by_wepback(5.6.4).png)

<br/>

## 마치며
---
앞서 설명했듯이 이번 포스팅은 앞으로 프로젝트를 구성하는 데 있어 필요한 기본적인 초기 구성에 대해서 설명하였다.
웹 개발에 있어서 빌드 시스템의 이해와 활용은 프로젝트의 성능, 유지보수성, 개발 효율성을 결정짓는 핵심 요소다.
프로젝트를 구성하면서 빌드 시스템를 이해하는 데 도움이 됐기를 바란다. 다음 포스팅은 최적화 방법에 대해서 살펴보도록 하겠다.
