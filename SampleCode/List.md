# サンプルプルグラム
## リスト１
[Ex1-1.html](Ex1-1.html)を参照


## リスト２：JSON形式の配列を用いたアノテーション用データの設定例．
```
<script>
window.addEventListener('load', () => {
const anotateButton = document.getElementById('anotate');
const resultArea = document.getElementById('result_div');
var data = 
  [
    {"item":"http://www.wikidata.org/entity/Q120730","itemLabel":"京都府"},
    {"item":"http://www.wikidata.org/entity/Q122723","itemLabel":"大阪府"},
    {"item":"http://www.wikidata.org/entity/Q130290","itemLabel":"兵庫県"}
  ];

anotateButton.addEventListener('click', () => {
var textINPUT = document.getElementById('input_text').value;

for (var i=0 ; i < data.length ; i++){
	textINPUT = textINPUT.replaceAll(data[i].itemLabel,
             '<b><a href="' + data[i].item + '" target="kg">'
              + data[i].itemLabel + '</a></b>');
}	

  resultArea.innerHTML = '<H2>アノテーション結果</H2>'
			+ textINPUT + '<br><H2>リンク先の表示</H2>'
			+'<iframe id="kg" name="kg" width=900" height="500"></iframe>;';	
	}, false);
}, false);
</script>
```
## 「アノテーションに用いるデータ」の外部ファイル化
外部データを読み込むには、プログラムの冒頭（最初の<script>タグの前）に下記の`<script src="data.js"></script>`ようなコードを追加する。   


## リスト３：SPARQLクエリを用いた検索例
[Ex2-1.html](Ex2-1.html)を参照


## リスト４：IDを指定したクエリ生成のための修正箇所１
```
<body>
<h1>SPARQLサンプル</h1>
<hr>
ID:<input type="text" id="qid_input" size="20" value="Q122723"></label><br>
<textarea id="query_area" cols="80" rows="10">
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
   
SELECT DISTINCT ?o ?oLabel
WHERE { 
wd:#INPUT# wdt:P36 ?o.
SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
</textarea>
<br>
<button id="send">クエリ実行</button>
<div id="result_div"></div>
</body>
```
### 追加の訂正箇所用コード  
クエリの文字列を取得する部分のコード`const query = textArea.value;`を下記に置き換える．
```
var qidINPUT = document.getElementById('qid_input').value;
const query = textArea.value.replace( '#INPUT#', qidINPUT );
```

## リスト5：URLのパラメータの処理．
 ```
var param = location.search;
  if(param!=""){
      var qid = param.replace("?id=","");
      document.getElementById('qid_input').value = qid;
  }
```
## 補足：非表示にしたい要素の処理
HTMLで「非表示」にしたい領域は．
```
<div style="display:none" >......</div>
```
で囲むとよい．  
※`style="display:none" `と設定された「タグで囲まれた範囲」は非表示となるので，<div>以外のタグにこの設定を追加することで非表示にすることも可能．

## リスト６：課題１と２の連携
課題１のプログラムのリンク先のURLを出力している`data[i].item`の部分を以下のコードで置き換える．
```
'Ex2-4.html?id=' + data[i].item.replace('http://www.wikidata.org/entity/',"")
```
※`Ex2-4.html`の部分は「検索結果を表示するプログラム」のファイル名とする．  
  
（参考）上記の置き換えを行った後の *アノテーション実行部分* のコードは下記のようになる．
```
for (var i=0 ; i < data.length ; i++){
	textINPUT = textINPUT.replaceAll(data[i].itemLabel,
	 '<b><a href="' 
	 + 'Ex2-4.html?id=' + data[i].item.replace('http://www.wikidata.org/entity/',"")
         + '" target="kg">'
	+ data[i].itemLabel + '</a></b>');
	}	
```
## リスト７：画像表示のサンプル
```
function showResult(resultData){
    const data = resultData.results.bindings;
    var i=0;
    var len = data.length;
    var mesText = "" ;
    while(i < len){
        if(data[i]['oLabel']!=null){
            mesText += '画像:<br><img width="300" src="'+data[i]['oLabel'].value+'"</img><br>';
        }
        i++;				
    }
    const resultArea = document.getElementById('result_div');
    resultArea.innerHTML='<h2>クエリ結果（画像）</h2>'+mesText;
}
```
## リスト８：クエリ例５の処理をするサンプル
```
function showResult(resultData){
    const data = resultData.results.bindings;
    var i=0;
    var len = data.length;
    var mesText = "" ;
    while(i < len){
        if(data[i]['oLabel']!=null){
            if(data[i]['p'].value.endsWith("P6")){
                mesText += "首長＝";
            }else if(data[i]['p'].value.endsWith("P36")){
                mesText += "県庁所在地＝";
            }
            mesText += data[i]['oLabel'].value+'<br>';
        }
        i++;				
    }
    const resultArea = document.getElementById('result_div');
    resultArea.innerHTML='<h2>都道府県のデータ/h2>'+mesText;
}
```
