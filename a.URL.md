#利用a标签自动解析URL

#很多时候我们有从一个URL中提取域名，查询关键字，变量参数值等的需要，而万万没想到可以让浏览器方便地帮我们完成这一任务而不用我们写正则去抓取。方法就在JS代码里先创建一个a标签然后将需要解析的URL赋值给a的href属性，然后就得到了一切我们想要的了。

#利用这一原理，稍微扩展一下，就得到了一个更加健壮的解析URL各部分的通用方法了。下面代码来自James的博客。

function parseURL(url) {
    var a =  document.createElement('a');
    a.href = url;
    return {
        source: url,
        protocol: a.protocol.replace(':',''),
        host: a.hostname,
        port: a.port,
        query: a.search,
        params: (function(){
            var ret = {},
                seg = a.search.replace(/^\?/,'').split('&'),
                len = seg.length, i = 0, s;
            for (;i<len;i++) {
                if (!seg[i]) { continue; }
                s = seg[i].split('=');
                ret[s[0]] = s[1];
            }
            return ret;
        })(),
        file: (a.pathname.match(/\/([^\/?#]+)$/i) || [,''])[1],
        hash: a.hash.replace('#',''),
        path: a.pathname.replace(/^([^\/])/,'/$1'),
        relative: (a.href.match(/tps?:\/\/[^\/]+(.+)/) || [,''])[1],
        segments: a.pathname.replace(/^\//,'').split('/')
    };
}
