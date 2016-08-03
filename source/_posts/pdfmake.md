---
title: pdfmake 로 PDF 파일 생성하기
date: 2016-07-20 20:40:19
tags: [Java Script, js, pdfmake, pdf]
---
<p>사용한 Library 는 [pdfmake](http://pdfmake.org/#/) 이다.<br />프로젝트를 하면서, 원래는 Server-side (Node JS 기반) 에서 PDF 를 생성하기 위한 모듈로 PDFKit 을 사용했었다.<br />하지만, 한글이 제대로 출력되지 않는 현상이 있어서, 다른 모듈을 사용하려고 이런 저런 모듈(Wkhtmltopdf, pdfmake(npm), Phantom JS 등..)을 찾아보다가, 난이도 떄문에.. 그냥 Client-side 에서 PDF 를 생성하기로 했다.&nbsp;&nbsp;\- **PDFKit 을 사용할 떄는,** Server-side 에서 PDF 생성 관련 URL 을 호출 시, response 로 blob 데이터를 전달해줬지만,<br />&nbsp;&nbsp;\- **pdfmake 를 사용할 떄는,** Server-side 에서 PDF 생성에 필요한 text 를 JSON 으로 전달해주는 식으로 PDF 를 동적 생성했다.</p>

## pdfmake 사용하기
<p>먼저 bower 를 통해 pdfmake 를 설치한다.<br />*bower 가 설치되어있다고 가정한다.*</p>

```sh
$ bower install pdfmake
```

<p>설치 후, 파일 구조도를 보면 다음과 같다.</p>
```sh
bower_components/pdfmake/
├── LICENSE
├── bower.json
├── build
│   ├── pdfmake.js
│   ├── pdfmake.min.js
│   ├── pdfmake.min.js.map
│   └── vfs_fonts.js
└── libs
    └── FileSaver.js
```

<p>위에 보이는 pdfmake.min.js 와 vfs_fonts.js 를 사용할 것이다.</p>

```html
<!DOCTYPE>
<html>
<head></head>
<body>
  ... 
  <script src="PATH/TO/jquery.min.js"></script>
  <script src="PATH/TO/pdfmake.min.js"></script>
  <script src="PATH/TO/vfs_fonts.js"></script>
</body>
</html>
```

<p>vfs_fonts 보다 pdfmake 를 먼저 불러와야 한다.<br />*이 예제에서 jQuery 를 사용할 것이기 때문에, jQuery 도 슬며시..*</p>

## Grunt Task 수행하기
vfs_fonts.js 를 생성하기 위해,
&nbsp;&nbsp;\- Grunt, grunt-cli 를 설치하고,
&nbsp;&nbsp;\- Gruntfile.js 를 만들어,
&nbsp;&nbsp;\- Grunt Task 를 수행한다.

```sh
$ touch Gruntfile.js
```

```js
/***** Gruntfile.js *****/
module.exports = function (grunt) {
  /* ... */

  grunt.initConfig({
    dump_dir: {
      fonts: {
        options: {
          pre: 'window.pdfMake = window.pdfMake || {}; window.pdfMake.vfs = ',
          rootPath: 'resources/'
        },
        files: {
          'dist/plugins/js/vfs_fonts.js': ['resources/fonts/*' ]
        }
      }
    },
  });

  grunt.loadNpmTasks('grunt-dump-dir');

  /* ... */
};
```

```sh
$ grunt dump_dir
```

<p>위 결과, resources/fonts 경로에 있는 파일들이 포함된 vfs_fonts.js 가 dist/plugins/js/vfs_fonts.js 경로에 생성된다.</p>

## Java Script 코드 작성하기

```js
$.ajax({
  url: '/question_bank/download',
  method: 'POST',
  responseType: 'blob',
  data: 'text=' + String(questionArr),
  success: function (res) {
    var fonts = {
      FONTNAME: {
        normal: 'FONTNAME-Regular.ttf',
        bold: 'FONTNAME-Bold.ttf',
        italics: 'FONTNAME-Regular.ttf',
        bolditalics: 'FONTNAME-Regular.ttf'
      }
    };

    var docDefinition = {
      content: [
        res.str
      ],
      defaultStyle: {
        font: FONTNAME
      }
    };

    pdfMake.fonts = fonts;
    pdfMake.createPdf(docDefinition, fonts).open();
  }
});
```

<p>변수 fonts 는 JSON 객체로, FONTNAME property 를 가지게 된다. FONTNAME property 는 사용자가 마음대로 설정할 수 있으며, **normal, bold, italics, bolditalics property 를 모두 설정**해주어야 한다.<br />docDefinition 을 통해 출력될 text 나 image 를 설정한다.<br />*[DOCS](http://pdfmake.org/#/gettingstarted) 에서 더 자세한 출력 방식을 볼 수 있다.*</p><p>pdfMake.createPdf(...).open(); 을 통해 새 창으로 PDF 가 로드된다.</p>

