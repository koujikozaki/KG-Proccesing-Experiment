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
## クエリ例１の補足：「分類階層」を含めたSPARQLクエリの例（博物館の一覧）．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?item ?itemLabel 
WHERE{
?item wdt:P31/wdt:P279* wd:Q33506. 
SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```
「wdt:P31」の代わりに「wdt:P31/wdt:P279*」を使うと分類階層を含めた一覧が取得できる．

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
## クエリ例２＋：「分類の種類（例：果物）」の一覧を取得するクエリ．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?item ?itemLabel 
WHERE{
?item wdt:P279* wd:Q3314483. 
SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```
## クエリ例１の参考：「都道府県」の一覧を「別名」を含めて取得するクエリ．
```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?item ?itemLabel 
WHERE{
?item wdt:P31 wd:Q50337. 
?item rdfs:Label|skos:altLabel ?itemLabel.
FILTER(lang(?itemLabel)="ja")
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
## クエリ例５：大阪府の情報を複数まとめて取得する【別クエリ１】．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

select DISTINCT ?p ?o ?oLabel 
{ wd:Q122723 ?p ?o.
 FILTER(?p=wdt:P36
       || ?p=wdt:P6 )
  SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```
## クエリ例６：大阪府の情報を複数まとめて取得する【別クエリ２】．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wikibase: <http://wikiba.se/ontology#>

select DISTINCT ?o ?oLabel ?prop ?propLabel
{ wd:Q122723 ?p ?o.
 FILTER(?p=wdt:P36
       || ?p=wdt:P6 )
 ?prop wikibase:directClaim ?p.
  SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en". }
}
```

