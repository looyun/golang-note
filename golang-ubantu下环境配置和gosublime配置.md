
从[http:\/\/www.golangtc.com\/download](http://www.golangtc.com/download) 下载包解压至\/usr\/local或$home

```bash
 nano ~/.bashrc
```

在文本最后添加

```bash
export PATH=$PATH:$HOME/go/bin
export GOROOT=$HOME/go
export GOPATH=$HOME/work
```

下载sublime-text,Ctrl+~,键入以下代码或从
 [https:\/\/packagecontrol.io\/installation\#st3](https://packagecontrol.io/installation#st3) 获取代码安装Package Control.

```bash
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

Ctrl+Shift+P,键入gosublime,Enter安装，如果安装后找不到GOPATH或GOROOT,GoSublime-Setting里找"env"修改如下

```bash
"env": { 
        "GOPATH": "$HOME/work",
        "GOROOT": "$HOME/go"
        },
```

