# nicorepo_bookmarklet
ニコレポのうち特定のレポートだけを抽出して表示するブックマークレット。

デフォルトでは動画投稿レポートだけを抽出します。

# 動作することを確認
- iOS Safari
- iOS GoogleChrome
- Windows10 GoogleChrome
- Windows10 FireFox

# 使い方
- ブラウザのブックマーク機能で、どこのページでも良いのでブックマークする。
- そのブックマークを編集し、URLの部分に後述のプログラムをコピペして保存してください
- ニコニコのページを（ログインした状態で）開きます
- 開いている状態で、先ほどのブックマークを開きます（スマホでの開き方は後述）
- ニコニコのページが書き換わります

# URLにコピペするプログラム
```
javascript:(function(){"sp.nicovideo.jp"==location.hostname?(document.open(),document.write('<a href="https://www.nicovideo.jp/my/top">PC\u30da\u30fc\u30b8</a>\u3067\u5b9f\u884c\u3057\u3066\u304f\u3060\u3055\u3044')):"www.nicovideo.jp"!=location.hostname?(document.open(),document.write('<a href="https://www.nicovideo.jp/my/top">\u30cb\u30b3\u30cb\u30b3\u52d5\u753b</a>\u3092\u958b\u3044\u305f\u72b6\u614b\u3067\u5b9f\u884c\u3057\u3066\u304f\u3060\u3055\u3044')):(document.open(),document.write('<!DOCTYPE html><html><head><script>\nvar DAYS=0,jsons=[],cursor="",smartphone="sp.nicovideo.jp"==location.hostname,xhr=new XMLHttpRequest;function main(a){DAYS=a;jsons=[];cursor="";document.getElementById("movies").innerHTML="loading:[]<br>";loader()}function loader(){xhr.onreadystatechange=function(){if(4==xhr.readyState&&200==xhr.status){var b=JSON.parse(xhr.responseText);jsons.push(b);cursor="cursor="+b.meta.minId+"&";if(0<b.data.length){var a=b.data[b.data.length-1];b=new Date;var d=new Date;d.setDate(b.getDate()-DAYS);a=new Date(a.createdAt);document.getElementById("movies").innerHTML="loading:["+Math.floor(100*(b.getTime()-a.getTime())/(864E5*DAYS))+"\uff05]<br>";if(a<=d){output();return}}loader()}};var a="https://"+location.hostname+"/api/nicorepo/timeline/my/all?"+cursor+"client_app=pc_myrepo&_="+Date.now();try{xhr.open("GET",a),xhr.send()}catch(b){document.getElementById("movies").innerHTML="\u901a\u4fe1\u306b\u5931\u6557\u3057\u307e\u3057\u305f\u3002\u30d6\u30e9\u30a6\u30b6\u7279\u6709\u306e\u30d0\u30b0\u306e\u3088\u3046\u3067\u3059\u3002<br>\u3082\u3046\u4e00\u5ea6\u30d6\u30c3\u30af\u30de\u30fc\u30af\u30ec\u30c3\u30c8\u3092\u8d77\u52d5\u3059\u308b\u3068\u3046\u307e\u304f\u884c\u304f\u304b\u3082\u3002"}}function output(){for(var a="",b=0;b<jsons.length;b++)for(var e=jsons[b],d=0;d<e.data.length;++d){var c=e.data[d];if("nicovideo.user.video.upload"==c.topic){var f=c.senderNiconicoUser;c=c.video;a+="<div>";a+=\'<a target="_blank" href="https://www.nicovideo.jp/watch/\'+c.id+\'"><img src="\'+c.thumbnailUrl.normal+\'" align="left"></a>\';a+=\'<a target="_blank" href="https://www.nicovideo.jp/user/\'+f.id+\'">\'+f.nickname+"</a><br>";a+=\'<a target="_blank" href="https://www.nicovideo.jp/watch/\'+c.id+\'">\'+c.title+\'</a><br clear="all">\';a+="</div><br>"}}document.getElementById("movies").innerHTML=a};\n\x3c/script></head><body><div><a target="_blank" href="https://com.nicovideo.jp/video/co3982693?page=1&sort=c&order=d"><img src="https://tn.smilevideo.jp/smile?i=32892185" align="left">\u5e83\u544a\uff1a\u9234\u97f3\u306e\u30d4\u30f3\u30af\u306a\u30dc\u30a4\u30ed\u52d5\u753b\u4e00\u89a7\u306f\u3053\u3061\u3089</a></div><br clear="all">\n<hr>\u4f55\u65e5\u524d\u307e\u3067\u306e\u52d5\u753b\u6295\u7a3f\u30ec\u30dd\u30fc\u30c8\u3092\u63a2\u3057\u307e\u3059\u304b\uff1f<hr>\n\uff3b<a href="javascript:main(2);">2\u65e5\u524d</a>\uff3d\uff3b<a href="javascript:main(4);">4\u65e5\u524d</a>\uff3d\uff3b<a href="javascript:main(7);">7\u65e5\u524d</a>\uff3d<hr><div id="movies"></div></body></html>\n'));
document.close()})();
```

# FAQ
## Safari:スマホで「開いている状態で、先ほどのブックマークを開きます」の方法が分からない
URL入力欄の左側にある、本のアイコンからブックマークを選択してください。

## GoogleChrome:スマホで「開いている状態で、先ほどのブックマークを開きます」の方法が分からない
アドレスバーに、登録したブックマークのタイトルを打ち込むと、予測ページとしてブックマークが一覧に出てくるので選んでください。

# 希望
もっと素敵な結果ページを作ってくれる人、プルリクエストお願いします。

# コードを書き換えたい人のための手順
nicorepo.jsは、単なる最初の画面表示機能です。ニコレポ収集のコードはinline.jsにあります。

inline.jsを書き換えた後、https://closure-compiler.appspot.com/home を使用してコード圧縮を行います。

圧縮したコードは「改行を消す」「シングルクォートをエスケープする(\\' に置換)」処理をしてください。

処理したコードを、nicorepo.jsのoutput変数の該当箇所にて置き換えしてください。

