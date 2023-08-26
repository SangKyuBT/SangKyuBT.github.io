---
layout: post
title: 웹팩을 활용한 리액트 프로젝트 구성 - 빌드 시스템 이해와 초기 구성
date: 2023-08-07 18:47:00 +0900
categories: [Developer, React]
tags: [Webpack, React, Babel]
published: true
---

## 리액트 프로젝트를 위한 빌드 시스템 이해와 중요성
---
CRA(create-react-app)는 리액트(React) 애플리케이션을 쉽고 빠르게 생성하고 설정할 수 있는 공식적인 도구다.
이는 리액트 프로젝트를 시작하고 설정하는데 드는 작업을 최소화하고 최적화에 필요한 개발 환경을 제공한다.
하지만 CRA는 높은 추상화 단계 때문에 흐름을 알기 어렵고, 빌드 시스템을 모르고 CRA로 만들어진 프로젝트를 다룬다면 성능 저하, 브라우저 호환성 문제, 환경 분리 등 여러 문제가 발생할 수 있다.  

### 빌드 시스템 이해와 중요성
빌드 시스템은 프론트엔드 프로젝트 구성에서 빼놓을 수 없는 중요한 역할을 하는 도구들의 집합의 일부로 애플리케이션의 소스 코드와 리소스를 효율적으로 관리하고 최적화하는 핵심 요소다. 이를 효과적으로 관리하고 최적화하기 위해 빌드 시스템을 이해하고 프로젝트를 구성하는 것은 다음과 같은 이유로 중요하다.

1. **커스터마이징 가능성**: CRA 같은 프로젝트 생성 도구들은 일반적인 설정을 제공하여 편리하지만, 프로젝트 성격에 따라 요구사항에 맞게 빌드 시스템을 커스터마이징해야 하는 경우가 생길 수 있다. 이러한 경우 CRA를 사용하는 것 보다 직접 빌드 시스템을 구축했을 때 요구사항을 더욱 자세히 처리할 수 있다.

2. **최적화**: 웹 애플리케이션의 성능을 최적화하려면 코드 스플리팅, 번들 최소화, 이미지 최적화 등 다양한 작업이 필요하다. 이를 효과적으로 처리하기 위해 빌드 시스템을 이해하는 것이 중요하다.

3. **새로운 도구와 기술 통합**: 프론트엔드 개발 분야는 빠르게 변화하고 새로운 도구와 기술이 등장하기 때문에 우리는 새로운 기술과 트렌드를 적극적으로 받아들이고 적용해야한다. 빌드 시스템을 직접 구축하면 새로운 도구와 기술을 더 쉽게 통합하고 적용할 수 있다.

**물론** CRA로 프로젝트를 생성한다고 위의 모든 사항이 불가하거나 불편한 것은 아니다. 하지만 우리가 직접 프로젝트를 구성하는 것 만큼 빌드 시스템을 이해하는데 효과적인 것은 없을 것이다. 이는 더 복잡한 프로젝트를 다룰 때 더 유용할 것이고, CRA를 사용하더라도 내부 동작을 이해하는데 도움이 될 것이다.

<br/>

### 초기 구성에 필요한 요소와 도구 
<= 하위 작성 글과 연관성 있게 써야할거 같음 이게 필요해!라고 해놓고 밑에 설명이 연관짓기가 힘듬 1.웹팩 2.바벨 이런식이 어떨까 싶음, node 까지 쓸까?, css와 정적 리소스는 로더와 플러그인에 묶으면 될거 같음, index관련 파일 설명이 따로 필요할거 같음 

이 포스팅에서는 우리가 프로젝트를 구성함에 있어 필요한 초기 구성에 대해 설명한다. 필요한 요소와 도구는 다음과 같다.
1. **모듈 번들러**: 대표적으로 Webpack이나 Parcel이 있으며 여러 파일들을 하나의 번들 파일로 묶는 역할을 한다.
2. **로더(Loader)와 플러그인(Plugin)**: 로더는 js파일이 아닌 다른 파일의 전처리, 플러그인은 번들링된 파일에 대한 추가 작업을 수행한다.
3. **바벨(Babel)**: 최신 Javascript 문법을 구형 브라우저에서도 동작할 수 있는 코드로 변환해주는 트랜스컴파일러다.
4. **개발 서버**: 개발 중에 코드 수정 시에 실시간으로 변경 사항을 확인할 수 있게 해준다.

<br/>

[웹팩 공식 문서](https://yamoo9.gitbook.io/webpack/react/create-your-own-react-app)를 참고하였으며, 웹팩 5를 기준으로 서술하였다. 그리고 모든 과정이 일치하지 않음을 참고바란다.

<br/>

## 초기 구성
---

### 1. 프로젝트 초기화 및 웹팩 패키지 설치
웹팩 패키지를 설치한다. 프로젝트 디렉토리에서 다음 명령어를 입력한다.

```console
mkdir react-app-by-webpack
cd react-app-by-webpack
npm init -y 
npm i webpack -D
```

<br/>

### 2. 웹팩 설정 파일 생성
웹팩의 동작 방식과 빌드 옵션을 제어하는 파일이다. 일반적으로 webpack-cli를 사용하여 루트 디렉토리에 있지만 우리는 webpack-cli 없이 별도의 빌드 스크립트를 작성할 것이기 때문에 config 디렉토리로 분리하였다. 자세한 설명은 [여기](https://webpack.kr/configuration/)를 참고 바란다.

- /config/webpack.config.js

```js
const path = require( 'path' )
const HtmlWebpackPlugin = require( 'html-webpack-plugin' )

const appDir = process.cwd()
const getAbsolutePath = pathDir => path.resolve( appDir, pathDir )

module.exports = ( webpackEnv ) => {
  const isProd = webpackEnv === 'production'
  const isDev = !isProd

  return {
    entry: { //프로젝트의 진입점(entry point) 파일의 경로를 지정한다.
      main: './src/index.js',
    },
    output: { //빌드 결과물의 출력 경로와 파일 이름을 정의한다.
      path: getAbsolutePath( 'dist' ),
      filename: 'assets/js/[name].[contenthash:8].js',
      publicPath: '/',
    },
    mode: isProd ? 'production' : 'development', //빌드 모드를 지정하며, development, production, none 중 하나를 선택한다.
    devtool: isDev && 'cheap-module-source-map', //소스맵을 생성하고 제어하기 위한 설정 옵션이다.
    resolve: { //모듈을 해석하고 찾는 방식을 설정한다.
      extensions: ['.js', '.jsx', '.json'],
      alias: {
        '@components': getAbsolutePath( 'src/components/' ),
        '@contexts': getAbsolutePath( 'src/contexts/' ),
        '@hooks': getAbsolutePath( 'src/hooks/' ),
        '@pages': getAbsolutePath( 'src/pages/' ),
      },
    },
    
    module: { //로더를 통해 파일을 변환하고 처리하는 규칙을 설정한다.
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

### 3. 바벨 패키지 설치
바벨은 최신 자바스크립트을 구형 브라우저에서도 호환되게 하거나, 리액트의 JSX의 문법을 브라우저에서 실행가능한 파일로 변경한다.
웹팩 빌드 시 작성한 자바스크립트 파일을 읽어와 바벨을 통해 변환한 후 결과를 번들에 포함시키도록 해야한다. 다음 명령어를 입력한다.

```console
npm i babel-loader @babel/core @babel/preset-env @babel/preset-react -D
```

1. **babel-loader**: 웹팩 빌드 과정에서 자바스크립트 파일을 잃어와 바벨 변환을 자동으로 처리 한다. 
2. **@babel/core**: 바벨의 핵심 엔진으로, 코드 변환 작업을 담당하며, 프리셋과 플러그인을 설정하여 변환 규칙을 지정한다.
3. **babel/preset-env**: 브라우저 호환성을 유지하면서 최신 문법을 사용할 수 있도록 해준다.
4. **@babel/preset-react**: 리액트의 JSX 문법을 구형 브라우저에서 동작 가능한 코드로 변환한다.

<br/>

### 4. 바벨 설정 파일 생성
프로젝트에서 사용하는 바벨 설정 파일이다. 필요한 프리셋과 플러그인을 설정한다. 자세한 설정 방법은 [여기](https://babeljs.io/docs/configuration)를 참고바란다.

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

<br/>

### 5. HTMLWebpackPlugin 패키지 설치 및 설정
웹팩에서 html파일을 생성하고 관리하기 위한 플러그인이다. 다음 명령어를 입력하고 웹팩 설정 파일을 수정한다.

```console
npm i html-webpack-plugin -D
```

- /config/webpack.config.js

```js
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

### 6. 환경 변수 설정
애플리케이션의 환경 변수를 설정한다. 웹팩 설정 파일을 수정한다.

- /config/webpack.config.js

```js
const webpack = require('webpack')
...
module.exports = ( webpackEnv ) => {
  ...
  return {
    ...
    plugins: [ 
      new webpack.EnvironmentPlugin({
        NODE_ENV: isDev ? 'development' : 'production'
      } ),
      ...
    ],
    ...
  }
}

```

<br/>

### 7. index.html 생성
브라우저가 처음으로 로딩할 때 보여지는 페이지의 기본 구조를 정의하고, 리액트가 UI를 구성할 HTML를 생성한다. 웹팩 설정에 추가한 HTMLWebpackPlugin은 이를 사용하여 웹팩의 빌드 결과물을 연결하고 있는 HTML를 생성할 것 이다. 

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

### 8. 리액트 패키지 설치
리액트 패키지를 설치한다. 다음 명령어를 입력한다.

```console
npm i react react-dom 
```

<br/>

### 9. index.js 생성
웹팩 설정 파일에 주입한 프로젝트 진입점(entry point) 파일이며, 리액트가 컴포넌트를 실제로 DOM에 렌더링할 곳이다. 

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

### 10. 빌드 스크립트 생성
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

### 11. 빌드 명령어 추가
package.json에 다음 명령을 추가한다.

- /package.json

```json
...
  "scripts": {
    "build": "node scripts/build.js"
  },
...
```

<br/>

### 12. 빌드 실행
빌드를 실행하고 결과를 확인한다. 다음 명령어를 입력한다.

```console
npm run build
```

우리는 웹팩 설정 파일의 output에 dist 폴더안에 빌드된 결과물이 들어가도록 설정하였다. 빌드된 결과물이 dist폴더 안에 생성되었고, 초기 구성이 완료되었다.

![react_by_webpack_1](/assets/img/posts/react_by_webpack_1.png)

<br/>

## 개발 서버 구성
---

### 1. 개발 서버 패키지 설치 및 설정
웹팩에서 제공하는 개발 서버 패키지를 설치한다. 다음 명령을 입력하고, 개발 서버 웹팩 설정 파일을 생성한다.

```console
npm i webpack-dev-server -D
```

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

### 2. 개발 서버 실행 스크립트 생성
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

### 3. 개발 서버 실행 명령 추가
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

### 4. 개발 서버 실행
웹팩 개발 서버를 실행하여 localhost:3000으로 접속하면 실시간으로 변경된 내용을 확인 할 수 있다. 다음 명령어를 입력한다. 
```
npm run dev
```
![react_by_webpack_2](/assets/img/posts/react_by_webpack_2.png)

<br/>

## 정적 리소스 연결
---
웹팩은 기본적으로 자바스크립트 모듈 번들러이기 때문에 다른 유형의 파일을 자동적으로 로드하지 못 한다. 따라서 정적 리소스를 사용하기 위해서는 별도의 설정이 필요하다. 

### 1. CSS 스타일 패키지 설치
CSS를 웹팩에서 처리하기 위해서는 해당 로더와 플러그인이 필요하다. 아래의 명령어를 입력해 패키지를 설치한다.
```
npm i style-loader css-loader mini-css-extract-plugin -D
```
1. **css-loader**: CSS 파일을 자바스크립트 모듈로 변환, 이를 웹팩 빌드 시 번들에 포함시킨다.
2. **style-loader**: CSS 모듈을 HTML에 `<style>` 태그로 주입한다. 주로 개발 모드에서 주로 사용된다.
3. **mini-css-extract-plugin**: CSS 모듈을 별도의 CSS 파일로 분리하여 관리하는 플러그인이다. 이를 연결하는 `<link>` 태그를 HTML에 주입한다. 

<br/>

### 2. CSS 로더 설정 
우리는 CSS 파일을 css-loader로 읽어 자바스크립트로 변환하여 모듈로 번들에 포함시켜야한다. 번들에 포함된 CSS 모듈을 개발 환경에서는 `<style>` 태그로, 프로덕션 환경에서는 별도의 CSS 파일로 추출 후 `<link>` 태그로 HTML에 주입해야한다. 웹팩 설정을 아래와 같이 변경한다.

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
    ...
  }
}
```

<br/>

### 3. index.css 생성 및 적용
적용할 CSS를 생성하고 적용 한 뒤 결과를 확인한다.

- src/index.css

```css
.app {
  color: red;
}
```

- src/index.js

```js
import React from 'react'
import ReactDOM from 'react-dom/client'

import './index.css'

import App from './app'

...
```

![react_by_webpack_3](/assets/img/posts/react_by_webpack_3.png)

<br/>

### 4. 웹 폰트, 이미지 로더 설정
웹팩 5는 웹 폰트, 이미지와 같은 정적 리소스를 읽어오는 Asset Module 기능을 제공한다. 아래와 같이 웹팩 설정을 수정한다. 자세한 설명은 [여기](https://webpack.kr/guides/asset-modules/)를 참고 바란다.

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
    ...
  }
}
```

<br/>

## 결론
---
웹 개발에 있어서 빌드 시스템의 이해와 활용은 프로젝트의 성능, 유지보수성, 개발 효율성을 결정 짓는 핵심적인 요소다. 초기 설정 부터 개발 서버 구축, 정적 리소스 연결까지 설명하였다. 다음 포스팅은 이어서 최적화 방법에 대해서 살펴 보도록 하겠다.

이번 포스팅은 

