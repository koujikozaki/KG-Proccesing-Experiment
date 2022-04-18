# https://query.wikidata.org/ で実行するクエリ例

## クエリ例１：都道府県の一覧を取得するSPARQLクエリの例．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?item ?itemLabel 
WHERE{
?item wdt:P31 wd:Q50337. 
SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```


## クエリ例２：日本の映画監督を取得するクエリ．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?item ?itemLabel 
WHERE{
?item wdt:P31 wd:Q5.
?item wdt:P27 wd:Q17. 
?item wdt:P106 wd:Q2526255. 
SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```


## クエリ例３：大阪府の都道府県の県庁所在地を取得する．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
	   
SELECT DISTINCT ?o ?oLabel
WHERE { 
wd:Q122723 wdt:P36 ?o.
SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```


## クエリ例４：大阪府の情報を複数まとめて取得する．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

select DISTINCT ?s ?sLabel ?o ?oLabel ?o2 ?o2Label
{ 
  BIND (wd:Q122723 AS ?s)
  wd:Q122723 wdt:P36 ?o.
  wd:Q122723 wdt:P6 ?o2.
  SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```


