## 49 Babel/Webpack

- Babel 이란? Babel은 자바스크립트 컴파일러이다. Babel은 현재 및 이전 브라우저 또는 환경에서 ECMAScript 2015+(이상)의 코드를 이전 버전의 JavaScript로 변환하는 데 주로 사용되는 도구이다.

- webpack 이란? JS 애플리케이션을 위한 정적 모듈 번들러이다. webpack이 애플리케이션을 처리할 때, 내부적으로는 프로젝트에 필요한 모든 모듈을 매핑하고 하나 이상의 번들을 생성하는 디펜던시 그래프를 만든다.
- Webpack은 의존 관계에 있는 자바스크립트, css, 이미지 등 리소스들을 하나의 파일로 번들링하는 모듈 번들러이다. Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요없다. HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움이 사라진다.

- Webpack 설치: $ npm install --save-dev webpack webpack-cli

```
// webpack.config.js 설정 파일 작성
const path = require('path');

module.exports = {
  // entry file
  entry: './src/js/main.js',
  //번들링된 js파일의 이름(filename)과 지정될 경로(path)를 지정
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [
          path.resolve(__dirname, 'src/js')
          ],
        exclude: /node_module/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-proposal-class-properties']
          }
        }
      }
      }
      },
  devtool: 'source-map',
  mode: 'development'
};
```

- Webpack Loader란? 웹팩이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 것들을 변환할 수 있도록 도와준다. 파일이 다른 언어를 Js로 변환하거나 인라인 이미지를 데이터 URL로 로드 할 수있다. 자바스크립트 모듈에서 직접 css파일을 import할 수 있다.
