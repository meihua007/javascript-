### 禁止别人以iframe加载你的页面
---------- 

#### 代码块
``` python
if (window.location != window.parent.location) window.parent.location = window.location;
```
