title: image upload and download with jQuery and SpringMVC MySQL Blob
date: 2015-10-17 11:21:19
tags:
---
### 需求
* 图片以blob 格式存储在MySQL 数据库中
* 浏览器中使用jQuery读取图片，显示到网页上。
* 将读取到的图片连同其他信息，如图片描述等信息一同上传到服务器
* 从数据库读取图片，显示到浏览器。
* **新建一个app并将其显示出来，app包含字段name,icon(图片),description**

### 实现
#### 读取图片显示到浏览器中。
* 使用 `<input type='file' id='icon'>` 来上传图片。
* 使用下面的JS代码进行获取。
```js
$('#fileIcon').on('change', function (evt) {
var files = evt.target.files;
var file = files[0]；
});
```
* 改变默认的上传外观
HTML中设置上传框隐藏
```html
<input name="" type="file" id="fileIcon" style="display:none"/>
<button id="fileIconButton" type="button" class="btn btn-default btn-sm">上传图标
</button>
```
  JS中调用上传框
  ```javascript
  var fileIcon = document.getElementById('fileIcon'),
          fileIconButton = document.getElementById('fileIconButton');
      if (fileIconButton) {
          fileIconButton.addEventListener('click', function () {
              if (fileIcon) {
                  fileIcon.click();
              }
          }, false);
      }
  ```
  设置input的`change`事件
  ```js
      $('#fileIcon').on('change', function (evt) {
          var files = evt.target.files;
          for (var i = 0, f; f = files[i]; i++) {
              if (!f.type.match('image.*')) {
                  toastr.error('文件格式不符合要求');
                  continue;
              }
              //显示到页面
              if (f.size < 100 * 1024) {
                  var reader = new FileReader();

                  reader.onload = (function (theFile) {
                      return function (e) {
                          $('#app-image-id').attr('src', e.target.result);
                          $('#app-icon-name').val(theFile.name);
                      };
                  })(f);

                  reader.readAsDataURL(f);
              } else {
                  toastr.error('文件过大,请选择小于100k的图片!');
              }
          }
      });
  ```
#### 上传图片和其他信息到服务器
 再次明确下需求，这里不是简单的上传一张图片到服务器，而是图片和其他信息一起。比如说：上传一个app的icon和app的名称，描述以及其他信息，一块到服务器
 这里在搜索解决办法的时候 是 `form image`。
 ```js
 var formData = new FormData();             
 formData.append('icon', icon); //图片            
 formData.append('name', name);//其他信息             
 formData.append('describes', describes);             
 formData.append('isAvailable', isAvailable);
 var xhr = new XMLHttpRequest();
 xhr.open('POST', '/apps', true);
  xhr.onload = function () {
      if (xhr.status === 200) {
          var appId = xhr.response;
          insertAppToAppTable({id: appId, name: name, describes: describes, isAvailable: isAvailable});
          $('.first').trigger('click');
      }
  };
  xhr.send(formData);
 ```
 ```Java
 @RequestMapping(value = "/apps", method = RequestMethod.POST)
  @ResponseBody
  public long createApp(@RequestBody MultipartFile icon, String name, String describes, boolean isAvailable) {
      byte[] iconFile = new byte[0];
      if (icon != null) {
          try {
              iconFile = icon.getBytes();
          } catch (IOException e) {
              e.printStackTrace();
          }
      }

      App app = new App();
      app.setIcon(iconFile);
      app.setName(name);
      app.setDescribes(describes);
      app.setIsAvailable(isAvailable);
      App result = appService.createApp(app);

      return result.getId();
  }
 ```
#### 从数据库取出Blob图片，显示到页面
在这里的时候，开始想的是从后台获取完整的app对象，然后得到app.icon字段。使用JS转码显示到页面。
但是在实现的时候遇到了很多的问题，查了很多资料，尝试了很多次，还是不能正确的显示出来。
最后改变了实现方式，**图片显示的时候单独向后台发请求** 就很迅速的完成了，总结一下是思路很重要。
前台代码
```html
<img class="app-icon" src="'/apps/' + ${app.id} + '/icon'"/></td>
```
SpringBoot Controller 代码
```java
@RequestMapping("apps/{appId}/icon")
  public ResponseEntity<?> getAppIcon(@PathVariable long appId) {
      HttpHeaders headers = new HttpHeaders();
      headers.setContentType(MediaType.IMAGE_JPEG);
      HttpStatus httpStatus = HttpStatus.OK;
      byte[] icon = appService.getAppById(appId).getIcon();
      if (icon == null) {
          httpStatus = HttpStatus.NOT_FOUND;
      }
      return new ResponseEntity<>(
              appService.getAppById(appId).getIcon(), headers,
              httpStatus);

  }
```
