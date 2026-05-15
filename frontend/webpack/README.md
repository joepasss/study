[webpack document](https://webpack.kr/concepts/)

----
## project init && webpack install

``` bash
### create npm project
npm init -y

### add packages to the package.json as development dependencies
npm install --save-dev webpack wepback-cli
```

**running webpack (webpack-cli)**

``` bash
$ npx webpack
asset main.js 1.44 KiB [emitted] [minimized] (name: main)
./src/index.js 2.63 KiB [built] [code generated]

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value.
Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/

webpack 5.106.2 compiled with 1 warning in 164 ms
```

``` bash
$ tree --gitignore
.
├── README.md
├── dist
│   └── main.js
├── images
│   ├── checkmark.svg
│   ├── delete.png
│   └── header-image.jpg
├── index.html
├── package-lock.json
├── package.json
└── src
    ├── index.css
    └── index.js

4 directories, 10 files
```

webpack configuration file 이 없어도 build는 정상적으로 이루어지는걸 알 수 있음. `npx webpack` 실행 이후에 configuration mode 관련 메시지가 났는데 [가능한 모드](https://webpack.kr/configuration/mode) 는 크게 `development`, `production`, `none` 이 있음.

configuration mode 를 설정 파일에서,

``` javascript
export default {
	mode: "production"
}
```

같이 주는것도 가능하지만, `npx webpack --mode=development` 같이 CLI의 인수로도 전달 가능함. `development` 와 `production` mode 사이에는 가장 큰 차이가 빌드되는 시간, 최적화의 "강도" 가 다름. `production` 에서는 최적화의 강도가 `development` 보다는 세고 빌드 시간은 `production` 이 `development` 보다는 느림. `none` 모드로서 최적화를 하지 않을 수 도 있음.

``` bash
### build time 비교해봤을때, 약 두배정도 차이
### 만들어진 파일들 비교해봤을때도 약 두배정도 차이남.

$ npx webpack --mode=production
asset main.js 1.44 KiB [compared for emit] [minimized] (name: main)
./src/index.js 2.63 KiB [built] [code generated]
webpack 5.106.2 compiled successfully in 140 ms

$ npx webpack --mode=development
asset main.js 3.93 KiB [emitted] (name: main)
./src/index.js 2.63 KiB [built] [code generated]
webpack 5.106.2 compiled successfully in 54 ms

### --mode=none 옵션
# 아직은 각 mode 별 설정파일이 없어서 developement 모드와 크게 차이가 없음
$ npx webpack --mode=none
asset main.js 2.68 KiB [emitted] (name: main)
./src/index.js 2.63 KiB [built] [code generated]
webpack 5.106.2 compiled successfully in 53 ms
```

webpack을 이용해서 javascript 파일을 빌드했으니, index.html 에서 참조된 기본 javascript file (`./src/index.js`) 를 생성된 파일 (`./dist/main.js`) 로 변경해줘야함.

``` bash
### 파일 구조
$ tree . -I "node_modules/"
.
├── README.md
├── dist
│   └── main.js
├── images
│   ├── checkmark.svg
│   ├── delete.png
│   └── header-image.jpg
├── index.html
├── package-lock.json
├── package.json
└── src
    ├── index.css
    └── index.js

4 directories, 10 files
```

``` html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>To Do List Application</title>
        <link rel="stylesheet" href="./src/index.css">
    </head>
    <body>
        <img class="header-image" src="./images/header-image.jpg" alt="To Do List" />
        <h1>To Do List</h1>
        <div class="todolist-wrapper">
            <input class="new-todo" placeholder="Enter text here" autofocus />
            <ul class="todo-list"></ul>
        </div>

		<!-- javascript file 경로 수정 (./src/index.js -> ./dist/main.js) -->
        <script src="./dist/main.js"></script>
    </body>
</html>
```

---

