Mobile end development

## 1. Responsive design

Mobile devices have different screen sizes, so responsive design for different screen sizes is needed to ensure that the website has a good User Experience on different devices.

### 1.1 Using CSS media queries

Media queries allow you to adjust your stylesheet based on properties such as screen size and resolution of your device. By adding media queries to your stylesheet, you can make your page look and layout differently on different devices.

```JavaScript
/* 在设备宽度小于 600 像素时，调整元素样式 */
@media only screen and (max-width: 600px) {
  body {
    font-size: 14px;
  }
  .header {
    background-color: #333;
    color: #fff;
  }
  .menu {
    display: none;
  }
}

/* 在设备宽度大于 600 像素时，调整元素样式 */
@media only screen and (min-width: 600px) {
  body {
    font-size: 16px;
  }
  .header {
    background-color: #fff;
    color: #333;
  }
  .menu {
    display: block;
  }
}
```

### 1.2 Fluid Layout

Fluid layout refers to the technology of automatically adjusting the layout according to the browser window or device screen size. With fluid layout, you can ensure that the elements of the page will display optimally on devices of different sizes.

```JavaScript
<div class="container">
  <div class="left-column">左边栏</div>
  <div class="right-column">右边栏</div>
</div>

.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto; //居中显示
  display: flex; //使用了 flexbox 布局来创建一个包含两列的容器
  flex-wrap: wrap; //允许换行
}

//在左侧列中，将 width 属性设置为 100%，并使用 max-width 属性将其最大宽度设置为 300px
.left-column {
  width: 100%;
  max-width: 300px;
  margin-right: 20px;
}

//在右侧列中，我们使用 calc() 函数将其宽度设置为窗口宽度减去左侧列的宽度和右侧列的边距（320px）
.right-column {
  width: calc(100% - 320px);
}

//这样，当浏览器窗口或设备屏幕变化时，两列就会自动调整其宽度和位置，以适应不同的设备和屏幕大小。
```

### 1.3 Image Optimization

On mobile devices, the size of image files can have a significant impact on page loading speed. Therefore, images should be optimized so that they load as fast as possible on different devices.

#### 1.3.1 Compressed Images

Using an image compression tool, such as TinyPNG or JPEGmini, you can reduce the file size of your images for faster loading.

#### 1.3.2 Using responsive images

Use responsive image technology, such as `srcset `or `picture `tags, to automatically resize images based on device size and resolution. For example:

```JavaScript
<picture>
  <source media="(min-width: 1024px)" srcset="large.jpg">
  <source media="(min-width: 768px)" srcset="medium.jpg">
  <img src="small.jpg" alt="example">
</picture>
```

#### 1.3.3 Using the WebP format

WebP is a web-based image format that provides smaller file sizes and higher image quality. In some modern browsers, images in WebP format can be used directly.

##### 1.3.3.1 Embed WebP Images into HTML Code

```JavaScript
<picture>
  <source srcset="image.webp" type="image/webp">
  <source srcset="image.jpg" type="image/jpeg">
  <img src="image.jpg" alt="example">
</picture>

使用 picture 标记来指定不同设备上应该显示的不同图像
在支持 WebP 格式的浏览器中，会显示 image.webp
在不支持 WebP 格式的浏览器中，会显示 image.jpg
如果想确保照顾到所有浏览器，也可以在 picture 标记中添加其他类型的图像，例如 PNG 或 GIF
```

##### 1.3.3.2 use WebP formatted image as background

```JavaScript
.element {
  background-image: url('image.webp');
  background-image: -webkit-image-set(url('image.webp') 1x, url('image@2x.webp') 2x);
  background-image: image-set(url('image.webp') 1x, url('image@2x.webp') 2x);
}

使用 background-image 属性来设置元素的背景图像
使用 -webkit-image-set 和 image-set 函数，以便在不同设备上都能显示不同的分辨率
```

#### 1.3.4 Lazy loading pictures

Using lazy loading techniques, you can delay loading an image until the user scrolls to the area where the image is located. This reduces page load time and improves User Experience.

## 2. Performance optimization

The hardware performance of mobile devices is limited, so it is necessary to optimize the performance of the website, such as compressing images and reducing page size, to improve the loading speed of the website.

### 2.1 Image optimization

Pictures are one of the most commonly used resources in mobile applications, so you need to optimize pictures to reduce the size of your application. For example, you can use appropriate picture formats, compress pictures, lazy load pictures, etc.

### 2.2 Reduce HTTP requests

Mobile devices often have slow network speeds, so you need to reduce the number of HTTP requests made by your application. For example, you can combine multiple CSS and JavaScript files into a single file (pack multiple files into one file), or use CDN acceleration, which can reduce the number of HTTP requests.

```HTML
<!DOCTYPE html>
<html>
<head>
    <title>使用CDN加速的例子</title>
    <link rel="stylesheet" type="text/css" href="https://cdn.bootcss.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>
<body>
    <h1>使用CDN加速的例子</h1>
    <p>这是一个使用CDN加速的例子。</p>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <script src="https://cdn.bootcss.com/popper.js/1.14.7/umd/popper.min.js"></script>
    <script src="https://cdn.bootcss.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
</body>
</html>

使用 Bootstrap 的 CDN 链接，可以从距离自己最近的服务器上加载这些文件，而不是从源服务器上加载
这可以减少网页的加载时间，提高用户体验
```

#### 2.2.1 Merging CSS and JavaScript Files

Combining multiple CSS and JavaScript files into a single file can reduce the number of HTTP requests. You can use tools such as Gulp or Grunt to automate the merging of files.

#### 2.2.2 Use of Font Icons

Using font icons can reduce HTTP requests for small icons while improving icon resolution without distortion. Font icons can be implemented using tool libraries such as Font Awesome.

#### 2.2.3 Caching static resources

Using browser caching and CDN caching can reduce the number of HTTP requests while improving the responsiveness of your application. You can use HTTP cache headers to set caching policies.

##### 2.2.3.1 cache header

1. Expires: Set the resource expiration time, after which the browser will rerequest the resource. For example, Expires: Fri, 01 Jan 2021 00:00:00 GMT.
2. Cache-Control: Set the cache policy, you can set the cache time, whether to allow cache, whether to allow cache private, whether to allow cache public, etc. For example, Cache-Control: max-age = 3600, public.
3. Last-Modified: Set the resource last modified time, the browser will compare this time with the resource last modified time on the server, if different, the browser will rerequest the resource. For example, Last-Modified: Fri, 01 Jan 2021 00:00:00 GMT.
4. ETag: Set the unique identity of the resource, the browser will compare the identity with the resource identity on the server, if different, the browser will rerequest the resource. For example, ETag: "abc123".

##### 2.2.3.2 Cache Policy

1. Force caching: Set the expiration time of the resource or Cache-Control: max-age = xxx, so that the browser will directly use the local cache during the caching time and will not send requests to the server.

```JavaScript
//在 Node.js 中设置 响应头中的 Expires 或 Cache-Control 字段
const http = require('http');
const fs = require('fs');
const path = './path/to/your/file';
const oneDay = 86400000; // 一天的毫秒数

http.createServer(function (req, res) {
  fs.readFile(path, function (err, data) {
    if (err) {
      console.log(err);
    } else {
      const expires = new Date(Date.now() + oneDay).toUTCString();
      res.setHeader('Expires', expires);
      res.setHeader('Cache-Control', 'public, max-age=' + (oneDay / 1000));

      res.writeHead(200, { 'Content-Type': 'image/png' });
      res.write(data);
      res.end();
    }
  });
}).listen(8080);

//使用 Node.js 的 fs 模块读取文件，然后设置响应头中的 Expires 和 Cache-Control 字段
//Expires 字段的值为资源的过期时间，可以使用 Date 对象来设置
//Cache-Control 字段的值为缓存策略，可以设置缓存时间和是否允许缓存
```

2. Negotiation cache: Set Last-Modified and ETag. The browser will store this information in the local cache. The next request will bring this information back to the server for comparison. If the resource has not been modified, the server will return a 304 status code. The browser will use the local cache directly.

```JavaScript
//在 Node.js 中设置 Last-Modified 和 ETag
const fs = require('fs');
const http = require('http');
const crypto = require('crypto');
const path = './path/to/your/file';

http.createServer(function (req, res) {
  fs.readFile(path, function (err, data) {
    if (err) {
      console.log(err);
    } else {
      const stats = fs.statSync(path);
      const lastModified = stats.mtime.toUTCString();
      const hash = crypto.createHash('md5').update(data).digest('hex');
      const etag = `"${hash}"`;

      res.setHeader('Last-Modified', lastModified);
      res.setHeader('ETag', etag);

      if (req.headers['if-modified-since'] === lastModified || req.headers['if-none-match'] === etag) {
        res.writeHead(304);
        res.end();
      } else {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.write(data);
        res.end();
      }
    }
  });
}).listen(8080);

//使用 Node.js 的 fs 模块读取文件，然后获取文件的最后修改时间和 md5 哈希值，将它们设置为响应头的 Last-Modified 和 ETag 字段
//在请求时，使用 req.headers 对象来检查浏览器是否具有 Last-Modified 或 ETag，如果有，则检查它们是否与本地文件相同
//如果是，则返回 304 状态码，告诉浏览器使用本地缓存；否则，返回 200 状态码和文件内容
```

#### 2.2.4 Loading resources on demand

Loading resources only when needed can reduce the number of HTTP requests. For example, when using modal boxes, you can load the HTML and JavaScript of the modal box only when the modal box is open.

### 2.3 Use of Local Storage

Using local storage can reduce the number of requests to the server and improve the responsiveness of the application

### 2.4 Avoid excessive DOM operations (reduce CPU load, reduce redrawing and rearrangement)

Mobile devices have limited processing power, so you need to avoid excessive DOM manipulation. You can use CSS3 animations instead of JavaScript animations to reduce DOM manipulation.

### 2.5 Code Compression

Compress JavaScript and CSS code to reduce file size. Compression tools such as UglifyJS and CSSNano can be used.

### 2.6 Using the cache

Using caching can reduce the number of requests to the server and improve the responsiveness of the application. For example, browser caching, CDN caching, etc. can be used.

```JavaScript
<!DOCTYPE html>
<html>
<head>
    <title>使用缓存的例子</title>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>使用缓存的例子</h1>
    <p>这是一个使用缓存的例子。</p>
    <img src="image.jpg" alt="图片">
    <script src="script.js"></script>
</body>
</html>
# 缓存样式表文件1个小时
<Files "style.css">
  Header set Cache-Control "max-age=3600, public"
</Files>

# 缓存图像文件1天
<FilesMatch "\.(jpg|jpeg|png|gif)$">
  Header set Cache-Control "max-age=86400, public"
</FilesMatch>

# 缓存 JavaScript 文件1个小时
<Files "script.js">
  Header set Cache-Control "max-age=3600, public"
</Files>

上面的代码是一个 .htaccess 文件，可以将其放置在网站的根目录中
使用 HTTP 缓存来缓存样式表、图像和 JavaScript 文件，以减少网页的加载时间
样式表和 JavaScript 文件将被缓存1个小时，图像文件将被缓存1天
```

## 3. Touch event

The main user interaction pattern of mobile devices is the touch screen, so touch events need to be used to achieve user interaction, such as tapping, swiping, etc.

3.1 Click events

This event is triggered when the user clicks on the screen.

```JavaScript
<button id="myButton">点击我</button>

<script>
var button = document.getElementById("myButton");

button.addEventListener("click", function() {
    alert("您单击了按钮！");
});
</script>
```

3.2 Sliding events

The event is triggered when the user swipes on the screen.

```HTML
<div id="myDiv" style="width: 200px; height: 200px; background-color: yellow;"></div>

<script>
var div = document.getElementById("myDiv");

div.addEventListener("touchmove", function(event) {
    event.preventDefault();
    var touch = event.touches[0];
    div.style.left = touch.pageX + "px";
    div.style.top = touch.pageY + "px";
});
</script>
```

3.3 Long press event

This event is triggered when the user long-presses on the screen.

```JavaScript
<div id="myDiv" style="width: 200px; height: 200px; background-color: yellow;"></div>

<script>
var div = document.getElementById("myDiv");

div.addEventListener("touchstart", function(event) {
    event.preventDefault();
    var touch = event.touches[0];
    var timer = setTimeout(function() {
        alert("您长按了屏幕！");
    }, 1000);
    div.addEventListener("touchend", function(event) {
        clearTimeout(timer);
    });
});
</script>

在"touchstart"事件上添加了一个事件监听器，当用户长按屏幕时，将在1秒钟后触发一个警报框
```

## 4. Appropriate font and button size:

On mobile devices, fonts and buttons need to be sized appropriately so that users can easily read and manipulate them on small screens.

### 4.1 Adjust the font size

Use CSS's @media query to adjust the font size to accommodate different screen sizes.

```JavaScript
body {
    font-size: 16px;
}

@media screen and (max-width: 768px) {
    body {
        font-size: 14px;
    }
}

@media screen and (max-width: 414px) {
    body {
        font-size: 12px;
    }
}

在屏幕宽度小于768px时，将使用14px的字体大小，在屏幕宽度小于414px时，将使用12px的字体大小
```

### 4.2 Resize the button

```JavaScript
button {
    padding: 10px 20px;
    font-size: 16px;
}

@media screen and (max-width: 768px) {
    button {
        padding: 8px 16px;
        font-size: 14px;
    }
}

@media screen and (max-width: 414px) {
    button {
        padding: 6px 12px;
        font-size: 12px;
    }
}

在屏幕宽度小于768px时，将使用较小的按钮大小，在屏幕宽度小于414px时，将使用最小的按钮大小
```

## 5. Browser Compatibility:

Different mobile devices use different browsers, so it is necessary to test the compatibility of the website on various mobile devices to ensure that the website works correctly on different devices and browsers.

## 6. Presentation:

Presentation on mobile devices also requires attention, such as landscape or portrait display, font size, image display ratio, etc.

## 7. Optimize the input method:

On mobile devices, the user input method is usually virtual keyboard or voice input, so it is necessary to optimize the user input method to improve the User Experience.

7.1 Automatic completion and automatic correction

```JavaScript
<label>名字：<input type="text" name="first_name" autocomplete="given-name"></label>
<label>姓氏：<input type="text" name="last_name" autocomplete="family-name"></label>
<label>邮箱：<input type="email" name="email" autocomplete="email"></label>
<label>电话：<input type="tel" name="phone" autocomplete="tel"></label>

使用了 autocomplete 属性，以便移动设备自动完成和自动修正用户输入
```

7.2 Voice input

```JavaScript
<label>搜索：<input type="text" name="search" x-webkit-speech></label>

使用了 x-webkit-speech 属性，以便移动设备可以使用语音输入功能
```