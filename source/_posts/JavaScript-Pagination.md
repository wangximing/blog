title: JavaScript Pagination
date: 2015-10-17 14:34:10
tags:
---
### 闲谈
写这段文字的时候，想起了之前的一个小故事。以前在做一个项目的时候，要做一个翻页功能，就**翻页**关键字再一直搜索，可想而知搜出来的都是一些CSDN上的博客，帮助不大，没有直观认识。
自从学会了**Pagination**关键字，一搜索就出来了可以用的插件。对，你没有看错，这段字就是想说我不知道翻页的英语是pagination,也想说下英语多么的重要
。翻页===pagination
### 正文
这里主要介绍一下一个jQuery pagination plugin，[twbspagination](http://esimakin.github.io/twbs-pagination/)
一个非常好用的翻页工具。
* 下载twbspagination文件，引入到要用的HTML文件中。
* 在HTML和js中分别添加下面的代码，就可以显示出分页效果。
```html
<ul id="pagination-demo" class="pagination-sm"></ul>
```
  启动并初始化插件
  ```js
  $('#pagination-demo').twbsPagination({
          totalPages: 35,
          visiblePages: 7,
          onPageClick: function (event, page) {
              $('#page-content').text('Page ' + page);
          }
      });
  ```
* 一个使用pagination插件的实例代码(差一点掉入回调地狱的代码)；
```js
$.ajax('/apps/totalNumber')
      .done(function (total) {
          var totalPages = total % 20 === 0 ? total / 20 : total / 20 + 1;
          $('#pagination-app').twbsPagination({
              totalPages: totalPages,
              visiblePages: 5 > totalPages ? 5 : totalPages,
              first: '首页',
              prev: '前一页',
              next: '下一页',
              last: '末页',
              onPageClick: function (event, page) {
                  $.ajax('/apps', {data: {pageSize: 20, pageNumber: page}})
                      .done(function (apps) {
                          var html;
                          apps.content.forEach(function (app) {
                              html += getAppTr(app);
                          });

                          var result = '<tbody>' + html + '</tbody>';
                          $('tbody').replaceWith(result);

                          setUpBootstrapSwitch();
                          imageOnError();
                      })
                      .error(function () {

                      });
              }
          });
      });
```
