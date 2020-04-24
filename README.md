# CKEditor 5 classic editor build

CKEditor 目前有的版本 

CKEditor 4  &  CKEditor 5

今天要分享的是 CKEditor 5 的實作

先介紹一下 CKEditor

是一個專門使用在網頁上屬於開放原始碼的文字編輯器。

它志於輕量化，不需要太複雜的安裝步驟即可使用。

可和 PHP、JavaScript、ASP、ASP.NET、ColdFusion、Java、以及 ABAP 等不同的程式語言相結合。

優點:

在於加載速度更快、減少 HTTP 請求數量，這些加速了我們的網站；

但缺點是不包含完整功能，CKEditor 5 將功能模組化，想要加入其他模組，

需使用 npm、webpack 重新打包程式 (webpack 是個酷東西)

# 實作開始(預設)

這次直接實作 build  專屬於自己的  CKEditor 5

先從預設版面 在到自定義版面  在到深入研究

首先到 GitHub 下載原始碼：https://github.com/ckeditor/ckeditor5-build-classic

![]();

下載方式 自行選擇

在開始build預設版型之前 要先安裝好 node.js 本人是下載windows版 (不同系統可至官網下載:https://nodejs.org/en/download/)

安裝完成後 開始以下步驟

Step_1 

進入安裝好的資料夾

開啟你的cmd 或是 git bash 確認目錄

![]();

Step_2

執行 npm install

npm install 的東西 會把安裝的資料 都放入 node_modules

目錄 : ckeditor5-build-classic-master\package.json 的 devDependencies 是指定套件 關於安裝的版本

Step_3

在 build 之前 打開設定黨 

目錄:  ckeditor5-build-classic-master\src\ckeditor.js

import 加入你要的 氁組

ClassicEditor.builtinPlugins 加入import 的東西

Step_4

開始 build 

```
npm run build
```
build 完後 會在build 資料夾 產生1個js檔

目錄:  ckeditor5-build-classic-master\build\ckeditor.js

Step_5

在該目錄測試  

建立1個html測試檔

```
<!DOCTYPE html>
<html>
      <head>
            <meta charset="utf-8">
            <title>CKEditor</title>
            <script src="ckeditor.js">
            </script>
      </head>
      <body>
            <div id="editor">This is some sample content.</div>
            <script>
			ClassicEditor.defaultConfig = {
	toolbar: {
		items: [
			'heading',
			'|',
			'bold',
			'italic',
			'link',
			'bulletedList',
			'numberedList',
			'|',
			'indent',
			'outdent',
			'|',
			'imageUpload',
			'blockQuote',
			'insertTable',
			'mediaEmbed',
			'undo',
			'CKFinder',
			'redo'
		]
	},
	image: {
		toolbar: [
			'imageStyle:full',
			'imageStyle:side',
			'|',
			'imageTextAlternative'
		]
	},
	table: {
		contentToolbar: [
			'tableColumn',
			'tableRow',
			'mergeTableCells'
		]
	},
	// This value must be kept in sync with the language defined in webpack.config.js.
	language: 'en'
};
                    ClassicEditor
                            .create(document.querySelector( 
                                    '#editor'))
                            .then(editor=>{
                                   console.log(editor);
                            })
                            .catch(error=>{
                                   console.error(error);
                            });
            </script>
      </body>
</html>
```
以上為預設版本的實作

# 實作中問題 與 進階

1.plugin 使用

參考網路的文章 與官網的範例 

研究許久 po出一些 遇到的問題 與解決的辦法

第1點 不用再引入plugin 

使用build 的方式 不需要在額外填入plugin

在官網的教學中:https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/installing-plugins.html

有不用build的方法 (需要引入plugin)  使用build(不需引入)

2.ClassicEditor.defaultConfig 的切割

在build時 不需要一開始  就先預設好要的東西 可將檔案切割出來自己使用 在透過

以下方法 引入 並自行做調整

以下是我分割出來的檔案

ckeditor.config.js
```

ClassicEditor.defaultConfig = {
    toolbar: {
        items: [
            'heading',
            '|',
            'bold',
            'italic',
            'link',
            'bulletedList',
            'numberedList',
            '|',
            'indent',
            'outdent',
            '|',
            'alignment:left', 
            'alignment:right', 
            'alignment:center', 
            'alignment:justify',
            '|',
            'imageUpload',
            'blockQuote',
            'insertTable',
            'mediaEmbed',
            'undo',
            'CKFinder',
            'redo',
            '|',
            'highlight',
            '|',
            'specialCharacters'
        ]
    },
    image: {
        toolbar: [
            'imageStyle:alignLeft',
            '|',
            'imageStyle:alignCenter',
            '|',
            'imageStyle:alignRight',
            '|',
            'imageStyle:full',
            '|',
            'imageTextAlternative'
        ],
        styles: [
            'full',
            'alignLeft',
            'alignCenter',
            'alignRight'
        ]
    },
    table: {
        contentToolbar: [
            'tableColumn',
            'tableRow',
            'mergeTableCells',
            'tableProperties', 
            'tableCellProperties'
        ]
    },
    highlight: {
        options: [
            { model: 'yellowMarker', class: 'marker-yellow', title: 'Yellow Marker', color: 'var(--ck-highlight-marker-yellow)', type: 'marker' },
            { model: 'greenMarker', class: 'marker-green', title: 'Green marker', color: 'var(--ck-highlight-marker-green)', type: 'marker' },
            { model: 'pinkMarker', class: 'marker-pink', title: 'Pink marker', color: 'var(--ck-highlight-marker-pink)', type: 'marker' },
            { model: 'blueMarker', class: 'marker-blue', title: 'Blue marker', color: 'var(--ck-highlight-marker-blue)', type: 'marker' },
            { model: 'redPen', class: 'pen-red', title: 'Red pen', color: 'var(--ck-highlight-pen-red)', type: 'pen' },
            { model: 'greenPen', class: 'pen-green', title: 'Green pen', color: 'var(--ck-highlight-pen-green)', type: 'pen' }
        ]
    },
    // This value must be kept in sync with the language defined in webpack.config.js.
    language: 'zh'
};


```

```
 <script src="/ckeditor5/ckeditor.config.js"> </script>
```
3.webpack build 變更語言

調整 webpack.config.js  

```
plugins: [
		new CKEditorWebpackPlugin( {
			// UI language. Language codes follow the https://en.wikipedia.org/wiki/ISO_639-1 format.
			// When changing the built-in language, remember to also change it in the editor's configuration (src/ckeditor.js).
			language: 'en',
			additionalLanguages: 'all'
		} ),
		new webpack.BannerPlugin( {
			banner: bundler.getLicenseBanner(),
			raw: true
		} )
	],
```
4.透過官網 修改 或 新增 node_nodule

從import 檔案的地方 知道檔案位置後 去查看並調整

這此範例是 我想調整表情貼圖 想加入一些 特別的圖 

找了好久 才找到 原來 都已經寫好 只要會帶入 設定值就好

調整後 一樣引入即可

## 引用文章:

1.https://medium.com/@charming_rust_oyster_221/ckeditor-5-%E6%96%87%E5%AD%97%E7%B7%A8%E8%BC%AF%E5%99%A8-%E7%A0%94%E7%A9%B6%E5%BF%83%E5%BE%97-519c97f20a4

2.
