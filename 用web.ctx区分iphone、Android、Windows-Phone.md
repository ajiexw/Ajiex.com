Tags: Python Webpy Mobile

webpy中的web.ctx保存每个HTTP请求的特定信息，比如客户端环境变量。

可以使用web.ctx在代码中获取来源页面(referring page)，客户端浏览器类型...

    #示例:用web.ctx.env获取HTTP_REFERER的值，如果HTTP＿REFERER不存在，就将google.com做为默认值。
    #接下来，页面就会被重定向回到之前的来源页面。
    class example:
        def GET(self):
            referer = web.ctx.env.get('HTTP_REFERER', 'http://google.com')
            raise web.seeother(referer)

##区分iphone、ipad、ipod、android、windows Phone、qq浏览器、uc浏览器

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    import web
    ios_markers = ['ipad', 'iphone', 'ipod']
    android_markers = ['android', 'adr']
    wp_markers = ['windows phone', 'wp7']
    qq_markers = ['mqqbrowser']
    uc_markers = ['uc', 'UC']

    def device():
        ua = web.ctx.env.get('HTTP_USER_AGENT', '')
        ua = ua.lower()
        for i in android_markers:
            if i in ua:
                return 'android'

        for i in ios_markers:
            if i in ua:
                return i

        for i in wp_markers:
            if i in ua:
                return 'wp'

        return 'other'

    def browser():
        ua = web.ctx.env.get('HTTP_USER_AGENT', '')
        ac = web.ctx.env.get('HTTP_ACCEPT', '')
        ua = ua.lower()
        for i in uc_markers:
            if i in ac + ua:
                return 'uc'
        for i in qq_markers:
            if i in ua:
                return 'qq'
        return 'other'



整理下html模板页面当中输出web.ctx.env的测试结果，方便以后对比,

包括Ubuntu12.04下的chromium,chrome,Firefox、windows-xp虚拟机下的ie6,ie7,ie8,360-5.0,chrome、win7虚拟机下的ie9,chrome。

##Ubuntu12.04 chromium 版本号：20.0.1132.47 Ubuntu 12.04 (144678)
    {

    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'PATH_INFO': '/cgi/topic/1',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT_CHARSET': 'UTF-8,*;q=0.5',
    'HTTP_USER_AGENT': 'Mozilla/5.0 (X11; Linux i686) AppleWebKit/536.11 (KHTML, like Gecko)
                        Ubuntu/12.04 Chromium/20.0.1132.47 Chrome/20.0.1132.47 Safari/536.11',
    'HTTP_CONNECTION': 'keep-alive',
    'HTTP_COOKIE': 'Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1355293927,1355308469,1355365632;
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355367660;
                    webpy_session_id=377fdabfe90c47d7c8772de5d46bf0acd7f395a5',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '127.0.0.1',
    'SERVER_PORT': '80',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_HOST': 'me.aoaola.com',
    'HTTP_CACHE_CONTROL': 'max-age=0', 
    'REQUEST_URI': '/topic/1',
    'HTTP_ACCEPT': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'REMOTE_PORT': '49683',
    'HTTP_ACCEPT_LANGUAGE': 'zh-CN,zh;q=0.8,ko;q=0.6', 
    'CONTENT_TYPE': '',
    'HTTP_ACCEPT_ENCODING': 'gzip,deflate,sdch'
                     
    }


##Ubuntu12.04 chrome 版本号：21.0.1180.89
    {

    'CONTENT_LENGTH': '', 
    'HTTP_ACCEPT': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'HTTP_USER_AGENT': 'Mozilla/5.0 (X11; Linux i686) AppleWebKit/537.1 (KHTML, like Gecko)
                        Chrome/21.0.1180.89 Safari/537.1',
    'HTTP_CONNECTION': 'keep-alive',
    'SERVER_PORT': '80',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '127.0.0.1',
    'REMOTE_PORT': '51167',
    'HTTP_ACCEPT_LANGUAGE': 'zh-CN,zh;q=0.8', 
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'HTTP_HOST': 'me.aoaola.com',
    'PATH_INFO': '/cgi/topic/1',
    'CONTENT_TYPE': '',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'HTTP_ACCEPT_CHARSET': 'GBK,utf-8;q=0.7,*;q=0.3',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_ACCEPT_ENCODING': 'gzip,deflate,sdch',
    'HTTP_COOKIE': 'myid=28; email=ajie0521%40gmail.com; md5password=8c0d1b48f83669dd65991d55a88c6454;
                    name=%E6%96%87%E5%90%9B; Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1355278959',
    'REQUEST_URI': '/topic/1'

    }


##Ubuntu12.04 Firefox 版本号：13.0.1 Mozilla Firefox for Ubuntu canonical - 1.0
    {
    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'HTTP_USER_AGENT': 'Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:13.0) Gecko/20100101 Firefox/13.0.1',
    'HTTP_CONNECTION': 'keep-alive',
    'SERVER_PORT': '80',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '127.0.0.1',
    'REMOTE_PORT': '53012',
    'HTTP_ACCEPT_LANGUAGE': 'zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'HTTP_HOST': 'me.aoaola.com',
    'PATH_INFO': '/cgi/topic/1',
    'CONTENT_TYPE': '', 
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '', 
    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_ACCEPT_ENCODING': 'gzip, deflate',
    'HTTP_COOKIE': 'Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1353490981,1353552948,1354789725,1355368495;
                    md5password=8c0d1b48f83669dd65991d55a88c6454; myid=28; email=ajie0521%40gmail.com;
                    name=%E6%96%87%E5%90%9B;webpy_session_id=38431ba096f5138e941cd20820801f88926f8e29;
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355368532', 
    'REQUEST_URI': '/topic/1'
                       
    } 

##windows xp虚拟机-ie6  版本号：6.0.2900.5512.xpsp.080413-2111

    {

    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT': '*/*',
    'HTTP_USER_AGENT': 'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)',
    'HTTP_CONNECTION': 'Keep-Alive',
    'SERVER_PORT': '80',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '192.168.10.110',
    'REMOTE_PORT': '52511',
    'HTTP_ACCEPT_LANGUAGE': 'zh-cn',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'HTTP_HOST': 'me.aoaola.com',
    'PATH_INFO': '/cgi/topic/1',
    'CONTENT_TYPE': '',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_ACCEPT_ENCODING': 'gzip, deflate',
    'HTTP_COOKIE': 'Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1353057347,1354859289,1355368761;
                    md5password=8c0d1b48f83669dd65991d55a88c6454;
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1 355368779;
                    webpy_session_id=d0551d101184f2b112af47df068138183cabe8db; myid=28;
                    email=ajie0521%40gmail.com;
                    md5password=8c0d1b48f83669dd65991d55a88c6454; name=%E6%96%87%E5%90%9B',
    'REQUEST_URI': '/topic/1'

    } 


##windows xp虚拟机-360安全浏览器5.0正式版  版本号：5.0.8.1

    {

    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT': '*/*',
    'HTTP_USER_AGENT': 'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)',
    'HTTP_CONNECTION': 'Keep-Alive',
    'SERVER_PORT': '80',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '192.168.10.110',
    'REMOTE_PORT': '59847',
    'HTTP_ACCEPT_LANGUAGE': 'zh-cn',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'HTTP_HOST': 'me.aoaola.com',
    'PATH_INFO': '/cgi/topic/1',
    'CONTENT_TYPE': '',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_ACCEPT_ENCODING': 'gzip, deflate',
    'HTTP_COOKIE': 'md5password=8c0d1b48f83669dd65991d55a88c6454;
                    Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1354859289,1355368761,1355369458,1355370096;
                    md5password=8c0d1b48f83669dd65991d55a88c6454;
                    webpy_session_id=dc1fbcd5a60345b661ae026fa70c7c442196cc43;
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355370096',
    'REQUEST_URI': '/topic/1'

    } 


##windows xp虚拟机-chrome 版本:23.0.1271.64 m

    {

    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'PATH_INFO': '/cgi/topic/1',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT_CHARSET': 'GBK,utf-8;q=0.7,*;q=0.3',
    'HTTP_USER_AGENT': 'Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.11 (KHTML, like Gecko)
                        Chrome/23.0.1271.64 Safari/537.11',
    'HTTP_CONNECTION': 'keep-alive',
    'HTTP_COOKIE': 'myid=28; email=ajie0521%40gmail.com; md5password=8c0d1b48f83669dd65991d55a88c6454;
                    name=%E6%96%87%E5%90%9B; Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1355368823;
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355370817;
                    webpy_session_id=8a0acddcea171cd46640eebf124049bfaded7ff6',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '192.168.10.110',
    'SERVER_PORT': '80',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_HOST': 'me.aoaola.com',
    'REQUEST_URI': '/topic/1',
    'HTTP_ACCEPT': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'REMOTE_PORT': '33106',
    'HTTP_ACCEPT_LANGUAGE': 'zh-CN,zh;q=0.8',
    'CONTENT_TYPE': '',
    'HTTP_ACCEPT_ENCODING': 'gzip,deflate,sdch'
                       
    }


##windows xp虚拟机-ie7  版本号：7.0.5730.13

    {

    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'PATH_INFO': '/cgi/topic/1',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'CONTENT_LENGTH': '',
    'HTTP_USER_AGENT': 'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)',
    'HTTP_CONNECTION': 'Keep-Alive',
    'HTTP_COOKIE': 'Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1355372694; 
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355372697;
                    webpy_session_id=0f3a1e4d8151ad4cfef6a6c5eb91ab048e1987b4',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '192.168.10.110',
    'SERVER_PORT': '80',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_HOST': 'me.aoaola.com',
    'HTTP_UA_CPU': 'x86',
    'REQUEST_URI': '/topic/1',
    'HTTP_ACCEPT': '*/*',
    'REMOTE_PORT': '37073', 
    'HTTP_ACCEPT_LANGUAGE': 'zh-cn', 
    'CONTENT_TYPE': '',
    'HTTP_ACCEPT_ENCODING': 'gzip, deflate'

    }


##windows xp虚拟机-ie8  版本号：8.0.6001.18702

    {

    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT': '*/*',
    'HTTP_USER_AGENT': 'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0)',
    'HTTP_CONNECTION': 'Keep-Alive',
    'SERVER_PORT': '80',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '192.168.10.110',
    'REMOTE_PORT': '39325',
    'HTTP_ACCEPT_LANGUAGE': 'zh-cn',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'HTTP_HOST': 'me.aoaola.com',
    'PATH_INFO': '/cgi/topic/1',
    'CONTENT_TYPE': '',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_ACCEPT_ENCODING': 'gzip, deflate',
    'HTTP_COOKIE': 'Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1355373707; 
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355373744;
                    webpy_session_id=abc7760bee8855cfd2e445781d0302313d5a1cd5',
    'REQUEST_URI': '/topic/1'

    }


##windows 7虚拟机-ie9  版本号：9.0.8112.16421

    {

    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT': 'text/html,
    application/xhtml+xml, */*',
    'HTTP_USER_AGENT': 'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)',
    'HTTP_CONNECTION': 'Keep-Alive', 
    'SERVER_PORT': '80',
    'SERVER_NAME': 'liu.aoaola.com', 
    'REMOTE_ADDR': '192.168.10.110',
    'REMOTE_PORT': '50354', 
    'HTTP_ACCEPT_LANGUAGE': 'zh-CN', 
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'HTTP_HOST': 'me.aoaola.com',
    'PATH_INFO': '/cgi/topic/1',
    'CONTENT_TYPE': '',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_ACCEPT_ENCODING': 'gzip, deflate',
    'HTTP_COOKIE': 'webpy_session_id=a18280d9ba274a6b73e25d997e262c810ceefa4b;
                    Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1355383562;
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355383723',
    'REQUEST_URI': '/topic/1'

    }

##windows 7虚拟机-chrome  版本号：21.0.1180.89 m

    {

    'HTTP_REFERER': 'http://me.aoaola.com/topic',
    'request_log': {},
    'REQUEST_METHOD': 'GET',
    'PATH_INFO': '/cgi/topic/1',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'QUERY_STRING': '',
    'CONTENT_LENGTH': '',
    'HTTP_ACCEPT_CHARSET': 'GBK,utf-8;q=0.7,*;q=0.3',
    'HTTP_USER_AGENT': 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.89 Safari/537.1',
    'HTTP_CONNECTION': 'keep-alive',
    'HTTP_COOKIE': 'Hm_lvt_df149e7ffb6b3724326fdf3f321e7678=1355384343;
                    Hm_lpvt_df149e7ffb6b3724326fdf3f321e7678=1355384358;
                    webpy_session_id=fc882f5d3316845e76021df6b3e91be001b7ac2f',
    'SERVER_NAME': 'liu.aoaola.com',
    'REMOTE_ADDR': '192.168.10.110',
    'SERVER_PORT': '80', 
    'DOCUMENT_ROOT': '/opt/aoaola/web',
    'HTTP_HOST': 'me.aoaola.com',
    'REQUEST_URI': '/topic/1',
    'HTTP_ACCEPT': 'textml,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'REMOTE_PORT': '52447', 
    'HTTP_ACCEPT_LANGUAGE': 'zh-CN,zh;q=0.8',
    'CONTENT_TYPE': '',
    'HTTP_ACCEPT_ENCODING': 'gzip,deflate,sdch'

    }

