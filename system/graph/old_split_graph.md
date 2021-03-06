---
title: Split GlyTouCan RDF GRAPH
layout: default
---
# rdf.glytoucan.org(from gs2virt)をバラす



## Virtuoso note
VirtuosoのSPARQLの結果件数の設定  
ResultSetMaxRows = 100万(テスト環境&本番環境)  
テスト環境では、CONSTRCTで全て取れた  
本番環境では、CONSTRCTで全て取れなかった  

GRAPHの削除

```
log_enable(2,1);
sparql clear graph <graph name>;
```


## Target GRAPH
**From**   
`http://rdf.glytoucan.org` (made by gs2virt)

**To**  
`http://rdf.glytoucan.org/core`  
including following classes  

* Saccharide class  
* Resource entry class  


`http://rdf.glytoucan.org/sequence/glycoct`  
including following classes  

* Glycosequence class  


`http://rdf.glytoucan.org/image`  
including following classes  

* Image class


`http://rdf.glytoucan.org/motif`  
including following classes  

* Motif class




## Saccharide Class  
**From**`http://rdf.glytoucan.org`  
**To**`http://rdf.glytoucan.org/core`  

* glycan:has_componentは今後利用しない予定だが、'</core>'に今のところ必要
    * glycan:has_componentが無ければ、Glycan ListやMonosaccharide compositionが表示されなくなるため
* Accession numberは、has_primary_idを利用しない予定だが、`</core>`に今のところ必要
* glycan:has_imageは、`</image>`に含まれる
* glycan:has_motifは、`</motif>`に含まれる
* glycan:has_glycosequenceは、`</sequence/glycoct>`と`</motif>`に含まれる


| Instance URI | Proerty   | Class |  Instance URI Literal | Literal | Individual |
|--------------|-----------|-------|-----------------------|---------|------------|
| Saccharide instance | a | glycan:saccharide | 
|                     | ~~glycan:has_motif~~ |  | ~~Glycan motif instance~~ |
|                     | ~~glycan:has_image~~|  | ~~Image instance~~ |
|                     | glycan:has_resource_entry |  | Glycosequence instance |
|                     | glycan:has_component |  | component instance |
|                     | ~~glytoucan:has_primary_id~~ |  |  | ~~"Accession number"~~ |



### INSERT query

```
log_enable(2,1);
sparql
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>
INSERT
{ GRAPH <http://rdf.glytoucan.org/core> {
    ?Saccharide a glycan:saccharide .
    ?Saccharide glytoucan:has_primary_id ?AccessionNumber .
    ?Saccharide glycan:has_resource_entry ?ResourceEntry .
  }
}
USING <http://rdf.glytoucan.org>
WHERE {
    # Saccharide
    ?Saccharide a glycan:saccharide .
    ?Saccharide glytoucan:has_primary_id ?AccessionNumber .
    ?Saccharide glycan:has_resource_entry ?Entry .
    BIND(STR(?Entry) AS ?strEntry)
    BIND(STRAFTER(?strEntry, "http://glytoucan.org/Structures/Glycans/") AS ?AccNum)
    BIND(IRI(CONCAT("http://rdf.glycoinfo.org/resource-entry/", ?AccNum)) AS ?ResourceEntry)
};
checkpoint;
commit WORK;
```


### confirm query
```
# Saccharide 
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>

SELECT ?ResourceEntry
#SELECT count(?ResourceEntry)
#SELECT count(?Saccharide)
#FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/core>
WHERE {
    # Saccharide
    #?Saccharide a glycan:saccharide .
    #?Saccharide glytoucan:has_primary_id ?AccessionNumber .
    #?Saccharide glycan:has_resource_entry ?ResourceEntry .
    #?Saccharide glycan:has_resource_entry ?ResourceEntry .
}
limit 200
```




## Resource entry Class
**From**`http://rdf.glytoucan.org`  
**To**`http://rdf.glytoucan.org/core`  

* 目的語のPerson instaceは、`http://rdf.glytoucan.org/users`のGRAPHにもある
* gs2virt版には、`dcterms:identifier`と`rdfs:seeAlso`が含まれていなかったので追加する。
  * `<glycan:resource_entry> dcterms:identifier “Accession number”`とする
  * `<glycan:resource_entry> rdfs:seeAlso “GlyTouCan URL”`とする

| Instance URI | Proerty   | Class |  Instance URI Literal | Literal | Individual |
|--------------|-----------|-------|-----------------------|---------|------------|
| Resource entry instance | a | glycan:resource_entry | 
|                     | glytoucan:contributor |  | Person instance |
|                     | glytoucan:date_registered |  |  | dateTimestamp |
|                     | dcterms:identifier |  |  | Accession number |
|                     | rdfs:seeAlso |  |  | GlyTouCan URL | 
|                     | glycan:in_glycan_database |  |  |  | glytoucan:database_glytoucan |


### INSERT query
```
log_enable(2,1);
sparql
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>
PREFIX dcterms: <http://purl.org/dc/terms/>

INSERT
{ GRAPH <http://rdf.glytoucan.org/core> {
    ?ResourceEntry a glycan:resource_entry .
    ?ResourceEntry glytoucan:contributor ?Contributor .
    ?ResourceEntry glytoucan:date_registered ?Date .
    ?ResourceEntry glycan:in_glycan_database ?DB .
    ?ResourceEntry dcterms:identifier ?AccessionNumber .
    ?ResourceEntry rdfs:seeAlso ?Entry .
  }
}
USING <http://rdf.glytoucan.org>
WHERE {
    # Resource entry
    ?Saccharide glytoucan:has_primary_id ?AccessionNumber .
    ?Saccharide glycan:has_resource_entry ?Entry .
    ?Entry a glycan:resource_entry .
    ?Entry glytoucan:contributor ?Contributor .
    ?Entry glytoucan:date_registered ?Date .
    ?Entry glycan:in_glycan_database ?DB .
    BIND(STR(?Entry) AS ?strEntry)
    BIND(STRAFTER(?strEntry, "http://glytoucan.org/Structures/Glycans/") AS ?AccNum)
    BIND(IRI(CONCAT("http://rdf.glycoinfo.org/resource-entry/", ?AccNum)) AS ?ResourceEntry)
};
checkpoint;
commit WORK;
```


### Confirm query

```
# Resource entry 
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT *
#SELECT COUNT(?AccessionNumber)
#SELECT COUNT(?DB)
#SELECT COUNT(?Date)
#SELECT COUNT(?Contributor)
#SELECT COUNT(?ResourceEntry)
#FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/core>
WHERE {
    # New Resource entry
    #?ResourceEntry a glycan:resource_entry .
    #?ResourceEntry glytoucan:contributor ?Contributor .
    #?ResourceEntry glytoucan:date_registered ?Date .
    #?ResourceEntry glycan:in_glycan_database ?DB .
    #?ResourceEntry dcterms:identifier ?AccessionNumber .
    #?ResourceEntry rdfs:seeAlso ?Entry .
}
limit 200
```







## Component Class
**From**`http://rdf.glytoucan.org`  
**To**`http://rdf.glytoucan.org/core`  

* Componentは、WURCS版が開発中のため、`</core>`を利用する


| Instance URI            | property                  | Class                 | Instance URI            | Literal     | Individual |
|-------------------------|---------------------------|-----------------------|-------------------------|-------------|------------|
| saccharide instance     | glycan:has_component      |                       | component instance      |             |            |
| component instance      | a                         | glycan:component      |                         |             |            |
|                         | glycan:has_cardinality    |                       |                         | xsd:integer |            |
|                         | glycan:has_monosaccharide |                       | monosaccharide instance |             |            |
| monosaccharide instance | a                         | glycan:monosaccharide |                         |             |            |


### INSERT query

```
log_enable(2,1);
sparql
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>
INSERT
{ GRAPH <http://rdf.glytoucan.org/core> {
  ?Saccharide glycan:has_component ?Component .
  ?Component a glycan:component .
  ?Component glycan:has_cardinality ?Cardinality .
  ?Component glycan:has_monosaccharide ?Monosaccharide .
  ?Monosaccharide a glycan:monosaccharide .
  }
}
USING <http://rdf.glytoucan.org>
WHERE {
    # Component 
    ?Saccharide a glycan:saccharide .
  ?Saccharide glycan:has_component ?Component .
  ?Component a glycan:component .
  ?Component glycan:has_cardinality ?Cardinality .
  ?Component glycan:has_monosaccharide ?Monosaccharide .
  ?Monosaccharide a glycan:monosaccharide .
};
checkpoint;
commit WORK;
```


### Confirm query

```
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>

SELECT *
#FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/core>
WHERE {
  # Saccharide
  ?Saccharide a glycan:saccharide .
  ?Saccharide glycan:has_component ?Component .
  ?Component a glycan:component .
  ?Component glycan:has_cardinality ?Cardinality .
  ?Component glycan:has_monosaccharide ?Monosaccharide.
  ?Monosaccharide a glycan:monosaccharide .
}
limit 200

```






## Glycosequence Class
**From**`http://rdf.glytoucan.org`  
**To**`http://rdf.glytoucan.org/sequence/glycoct`  

* `http://rdf.glytoucan.org/sequence/glycoct`のGRAPHについて
	* 登録時のGlycoCTを記述している
	* Property
		* rdfs:label, glycan:has_glycosequence, glycan:has_sequence, glycan:in_carbohydrate_format
		* glycan:glycosequenceのタイプ付けは無し
			* glycan:glycosequenceのタイプ付けをした方が良い
		* rdfs:labelとglycan:has_sequenceが同じデータタイプ
			* rdfs:labeはどこかで使われているか？
      * Stanzaでは使われていなかった

| Instance URI             | property                      | Class                | Instance URI            | Literal        | Individual                         |
|--------------------------|-------------------------------|----------------------|-------------------------|----------------|------------------------------------|
| saccharide instance     | glycan:has_glycosequence      |                      | glycosequence instance |                |                                    |
| glycosequence instance| a                             | glycosequence instance |                         |                |                                    |
|                          | glycan:has_sequence           |                      |                         | GlycoCT string |                                    |
|                          | glycan:in_carbohydrate_format |                      |                         |                | glycan:carbohydrate_format_glycoct |
|                          | rdfs:label                    |                      |                         | GlycoCT string |                                    |

### INSERT query
```
log_enable(2,1);
sparql
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
INSERT
{ GRAPH <http://rdf.glytoucan.org/sequence/glycoct> {
    ?Saccharide glycan:has_glycosequence ?GSequence .
    ?GSequence glycan:has_sequence ?Sequence .
    ?GSequence glycan:in_carbohydrate_format ?Format .
    ?GSequence rdfs:label ?Sequence .
  }
}
USING <http://rdf.glytoucan.org>
WHERE {
    # Glycosequence
    ?Saccharide glycan:has_glycosequence ?GSequence .
    ?GSequence glycan:has_sequence ?Sequence .
    ?GSequence glycan:in_carbohydrate_format ?Format .
};
checkpoint;
commit WORK;
```


### Confirm query

```
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 

SELECT *
#SELECT COUNT(?label)
#SELECT COUNT(?Format)
#SELECT COUNT(?Sequence)
#SELECT COUNT(?GSequence)
#FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/sequence/glycoct> 
WHERE {
    # Glycosequence
    ?Saccharide glycan:has_glycosequence ?GSequence .
    ?GSequence glycan:has_sequence ?Sequence .
    ?GSequence glycan:in_carbohydrate_format ?Format .
    ?GSequence rdfs:label ?label .
}
limit 100
```





## Image Class
**From**`http://rdf.glytoucan.org`  
**To**`http://rdf.glytoucan.org/image`  

```
* GlyTouCanでは、RDFで記述されているImageのURIは利用していない
* JavascriptでイメージのURLを作っている
* とくに必要な場面は今のところ無いため、GRAPHは作らない
* 念のためにやり方を残す
```

* 新しいGRAPH `http://rdf.glytoucan.org/image`を用意する
* gs2virtにあるImageをインサートする


| Instance URI        | property                 | Class          | Instance URI | Literal              | Individual                      |
|---------------------|--------------------------|----------------|--------------|----------------------|---------------------------------|
| saccharide instance | glycan:has_image         | image instance |              |                      |                                 |
| image instance    | a                        | glycan:image   |              |                      |                                 |
|                     | dc:format                |                |              | image/pingxsd:string |                                 |
|                     | glycan:has_symbol_format |                |              |                      | glycan:symbol_format_cfg        |
|                     |                          |                |              |                      | glycan:symbol_format_uoxf       |
|                     |                          |                |              |                      | glycan:symbol_format_text       |
|                     |                          |                |              |                      | glycan:symbol_format_uoxf_color |
|                     |                          |                |              |                      | glycan:symbol_format_cfg_uoxf   |
|                     |                          |                |              |                      | glycan:symbol_format_cfg_bw     |



### INSERT query
```
log_enable(2,1);
sparql
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>
INSERT
{ GRAPH <http://rdf.glytoucan.org/image> {
  ?Saccharide glycan:has_image ?Image .
    ?Image a glycan:image .
    ?Image dc:format ?Format .
    ?Image glycan:has_symbol_format ?Symbol .
  }
}
FROM <http://rdf.glytoucan.org>
WHERE {
    # Image 
  ?Saccharide glycan:has_image ?Image .
    ?Image a glycan:image .
    ?Image dc:format ?Format .
    ?Image glycan:has_symbol_format ?Symbol .
}
```


### Confirm query

```
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>

SELECT *
#SELECT COUNT(?Symbol)
#SELECT COUNT(?Format)
#SELECT COUNT(?Image)
#FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/image/test> 
WHERE {
    # Image 
    #?Sacc glycan:has_image ?Image .
    #?Image a glycan:image .
    #?Image dc:format ?Format .
    #?Image glycan:has_symbol_format ?Symbol .

}
limit 100
```





## Glycan motif Class
**From**`http://rdf.glytoucan.org`  
**To**`http://rdf.glytoucan.org/motif`  


 `</motif>`

* glycan:has_motif (only)


追加

* rdf:type
* glycan:has_glycosequence
* rdfs:label
* glytoucan:is_reducing_end


| Instance URI            | property                  | Class                 | Instance URI             | Literal         | Individual |
|-------------------------|---------------------------|-----------------------|--------------------------|-----------------|------------|
| saccharide instance     | glycan:has_motif          | glycan motif instance |                          |                 |            |
| glycan motif instance | a                         | glycan:glycan_moti    |                          |                 |            |
|                         | glycan:has_glycosequence  |                       | glycosequence instance |                 |            |
|                         | rdfs:label                |                       |                          | “motif name”@en |            |
|                         | glytoucan:is_reducing_end |                       |                          | boolean         |            |



### INSERT query
```
log_enable(2,1);
sparql
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>

INSERT
{ GRAPH <http://rdf.glytoucan.org/motif> {
   # Motif
    ?Saccharide glycan:has_motif ?Motif .
    ?Motif a glycan:glycan_motif .
    ?Motif glycan:has_glycosequence ?GSequence .
    ?Motif rdfs:label ?MotifName .
    ?Motif glytoucan:is_reducing_end ?ReducingEnd .
  }
}
USING <http://rdf.glytoucan.org>
WHERE {
    # Motif
    ?Saccharide glycan:has_motif ?Motif .
    ?Motif a glycan:glycan_motif .
    ?Motif glycan:has_glycosequence ?GSequence .
    ?Motif rdfs:label ?MotifName .
    ?Motif glytoucan:is_reducing_end ?ReducingEnd .
};
checkpoint;
commit WORK;
```


### Confirm query

```
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glytoucan:  <http://www.glytoucan.org/glyco/owl/glytoucan#>

SELECT *
#SELECT COUNT(?MotifName)
#SELECT COUNT(?GlycanMotifAlias)
#SELECT COUNT(?ReducingEnd)
#SELECT COUNT(?GSequence)
#SELECT COUNT(?GlycanMotif)
#FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/motif/test> 
WHERE {
    # Motif
    #?Saccharide glycan:has_motif ?GlycanMotif.
    #?GlycanMotif a glycan:glycan_motif .
    #?GlycanMotif glycan:has_glycosequence ?GSequence .
    #?GlycanMotif glytoucan:is_reducing_end ?ReducingEnd .
    #?GlycanMotif rdfs:label ?MotifName .
}
limit 100
```




### Note
* http://test.ts.glytoucan.org/sparql
	* このテスト環境のエンドポイントでコンストラクトすると、 glytoucan:is_reducing_endのbooleanが数字の０と１になる
* https://ts.glytoucan.org/sparql
	* 本番環境のエンドポイントでコンストラクトをすると、glytoucan:is_reducing_endのbooleanが"true"^^xsd:booleanと"false"^^xsd:boolean になる
* テスト環境にTTLをアップロードしても、Virtuosoの状態により、0と1になってしまう。
* テスト環境、本番環境、共にINSERTクエリを利用する

