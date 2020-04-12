---
title: XHR Request Intercept
date: 2020-03-19 09:30:28 -0400
categories: jekyll update
---

[https://stackoverflow.com/questions/53004906/how-to-intercept-xmlhttprequest-and-change-request-url](https://stackoverflow.com/questions/53004906/how-to-intercept-xmlhttprequest-and-change-request-url)

```javascript
const intercept = (urlmatch, newurl) => {
    const open = XMLHttpRequest.prototype.open;
    
    XMLHttpRequest.prototype.open = function (method, url, ...rest) {
        url = url.replace(urlmatch, newurl);
        return open.call(this, method, url, ...rest);
    };
}

//intercept('original url', 'change url');
intercept('http://10.217.66.21', 'http://211.252.123.38');
```