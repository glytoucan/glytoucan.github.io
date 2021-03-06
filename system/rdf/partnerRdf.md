---
title: Partner RDF
authors:
- Daisuke Shinmachi
date: 2016-04-20
layout: default
---

# <a name="menu"> GlyTouCan Partner RDF
* [from BCSDB](#from BCSDB)
* [from GlycomeDB](#from GlycomeDB)  
* [from GlycoEpitope](#from GlycoEpitope)
* [from GlycoNAVI](#from GlycoNAVI)
	* [PubChem](#pubchem)
	* [wwPDB](#wwpdb) 
	* [wwPDB CC](#wwpdb cc) 
* [from UniCarbKB](#from UniCarbKB)
* [from UniCarb-DB](#from UniCarb-DB)
* [SPARQL Query](#SPARQL Query)


##  About IRI

**Using Domain** 

- rdf.glycoinfo.org  
- identifier.org



**Instance IRI**

```
Source instance  
http://rdf.glycoinfo.org/source/{taxnomomy_id}


Taxonomy instance    
http://identifiers.org/taxonomy/{taxonomy_id}
=> http://identifiers.org/taxonomy/1010795

# In the case of a taxon has not taxonomy id.
http://rdf.glycoinfo.org/source/{encode_taxon_name}
=> http://rdf.glycoinfo.org/source/Helicobacter%20pylori%20O3


Article instance   
http://rdf.glycoinfo.org/references/{pubmed_id}


PubMed URL  
http://identifiers.org/pubmed/{pubmed_id}


Resource entry instance IRI  
http://rdf.glycoinfo.org/{database_label or entry_id_type}/{each_entry_id}

	# BCSDB
	http://rdf.glycoinfo.org/bcsdb/6922

	# GlycomeDB
	http://rdf.glycoinfo.org/glycome-db/48

	# JCGGDB
	http://rdf.glycoinfo.org/jcggdb/JCGG-STR010506

	# GlycoEpitope
	http://rdf.glycoinfo.org/glycoepitope/EP0011

	# PubChem SID
	http://rdf.glycoinfo.org/pubchem/SID252275760

	# PubChem CID
	http://rdf.glycoinfo.org/pubchem/CID91844939

	# wwPDB
		# PDBj 
		http://rdf.glycoinfo.org/pdbj/03F
		# PDBe CC
		http://rdf.glycoinfo.org/pdbe/03F
		# RCSB PDB CC
		http://rdf.glycoinfo.org/rcsb_pdb/03F

	# wwPDB CC
		# PDBj CC
		http://rdf.glycoinfo.org/pdbj-cc/03F
		# PDBe CC
		http://rdf.glycoinfo.org/pdbe-cc/03F
		# RCSB PDB CC
		http://rdf.glycoinfo.org/rcsb_pdb-cc/03F

	# UniCarbKB
	http://rdf.glycoinfo.org/unicarbkb/1
```





**Entry page URL**  

```
BCSDB  
http://csdb.glycoscience.ru/bacterial/core/search_id.php?mode=record&id_list={bcsdb_id}
=> http://csdb.glycoscience.ru/bacterial/core/search_id.php?mode=record&id_list=6922 

GlycomeDB  
http://www.glycome-db.org/database/showStructure.action?glycomeId={glycome-db_id}
=> http://www.glycome-db.org/database/showStructure.action?glycomeId=48

JCGGDB
http://jcggdb.jp/idb/jcggdb/{jcggdb_id}
=> http://jcggdb.jp/idb/jcggdb/JCGG-STR010506

GlycoEpitope  
http://identifiers.org/glycoepitope/{epitope_id}
=> http://identifiers.org/glycoepitope/EP0011

PubChem substance  
http://pubchem.ncbi.nlm.nih.gov/substance/{sid}
=> http://pubchem.ncbi.nlm.nih.gov/substance/SID252275760

PubChem compound  
http://pubchem.ncbi.nlm.nih.gov/compound/{cid}
=> http://pubchem.ncbi.nlm.nih.gov/compound/CID91844939

PDBj CC
http://pdbj.org/chemie/summary/{pdb_chem-comp_id}
=> http://pdbj.org/chemie/summary/03F

PDBe CC
http://www.ebi.ac.uk/pdbe-srv/pdbechem/chemicalCompound/show/{pdb_chem-comp_id}
=> http://www.ebi.ac.uk/pdbe-srv/pdbechem/chemicalCompound/show/03F

RCSB PDB CC
http://ligand-expo.rcsb.org/pyapps/ldHandler.py?formid=cc-index-search&operation=ccid&target={pdb_chem-comp_id}
=> http://ligand-expo.rcsb.org/pyapps/ldHandler.py?formid=cc-index-search&operation=ccid&target=03F

UniCarbKB
http://www.unicarbkb.org/structure/{structure_id}
=> http://www.unicarbkb.org/structure/1
```




### <a name="from BCSDB">from BCSDB 
[return to menu](#menu)


GRAPH : `<http://rdf.glytoucan.org/partner/bcsdb>`  

**Dataset**

``` 
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix bibo: <http://purl.org/ontology/bibo/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .


# Species
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
	glycan:is_from_source <http://rdf.glycoinfo.org/source/{taxonomy_id}> ;
	glycan:is_from_source <http://rdf.glycoinfo.org/source/{encode_taxon_name}> .

# exist taxonomy id
<http://rdf.glycoinfo.org/source/{taxonomy_id}>
	a glycan:Source ;
	rdfs:label "taxon name" ;
	dcterms:identifier "{taxonomy_id}" ;
	rdfs:seeAlso <http://identifiers.org/taxonomy/{taxonomy_id}> .

# dose not taxonomy id
<http://rdf.glycoinfo.org/source/{encode_taxon_name}>
	a glycan:Source ;
	rdfs:label "taxon name" .


# Literature
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
	dcterms:references <http://rdf.glycoinfo.org/references/{pubmed_id}> ; #Article

<http://rdf.glycoinfo.org/references/{pubmed_id}>
	a bibo:Article ;
	dcterms:identifier "{pubmed_id}" ;
	rdfs:seeAlso <http://identifiers.org/pubmed/{pubmed_id}> . #PubMed URL


# External ID
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
	glycan:has_resource_entry <http://rdf.glycoinfo.org/bcsdb/{bcsdb_id}>.

<http://rdf.glycoinfo.org/bcsdb/{bcsdb_id}>
	a glycan:Resource_entry ;
	rdfs:label	"BCSDB" ;
	glycan:in_glycan_database glycan:Database_bcsdb ;
	dcterms:identifier  "{bcsdb_id}" ;
	rdfs:seeAlso	<http://csdb.glycoscience.ru/bacterial/core/search_id.php?mode=record&id_list={bcsdb_id}> .

glycan:Database_bcsdb 
	rdfs:label "BCSDB".
```



### <a name="from GlycomeDB">from  GlycomeDB
[return to menu](#menu)

GRAPH : `<http://rdf.glytoucan.org/partner/glycome-db>`  

**Dataset**

```
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .

# Species
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
	glycan:is_from_source <http://rdf.glycoinfo.org/source/{taxonomy_id}> .

<http://rdf.glycoinfo.org/source/{taxonomy_id}>
	a glycan:Source ;
	dcterms:identifier "{taxonomy_id}" ;
	rdfs:seeAlso <http://identifiers.org/taxonomy/{taxonomy_id}> .


# External ID
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
	glycan:has_resource_entry <http://rdf.glycoinfo.org/glycome-db/{glycome-db_id>.

<http://rdf.glycoinfo.org/glycome-db/{glycome-db_id}>
	a glycan:Resource_entry ;
	rdfs:label	"GlycomeDB" ;
	glycan:in_glycan_database glycan:Database_glycomedb ;
	dcterms:identifier  "{glycome-db_id}" ;
	rdfs:seeAlso	<http://www.glycome-db.org/database/showStructure.action?glycomeId={glycome-db id}> .

glycan:Database_glycomedb 
	rdfs:label "GlycomeDB".



# JCGGDB entry (External ID) from GlycomeDB 
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
	glycan:has_resource_entry <http://rdf.glycoinfo.org/jcggdb/{jcggdb_id}>.

<http://rdf.glycoinfo.org/jcggdb/{jcggdb_id}>
	a glycan:Resource_entry ;
	rdfs:label	"JCGGDB" ;
	glycan:in_glycan_database glycan:Database_jcggdb ;
	dcterms:identifier  "jcggdb_id" ;
	rdfs:seeAlso	<http://jcggdb.jp/idb/jcggdb/{jcggdb_id}> .

glycan:Database_jcggdb	
	rdfs:label	"JCGGDB" .
```




### <a name="from GlycoEpitope">from GlycoEpitope 
[return to menu](#menu)

GRAPH : `<http://rdf.glytoucan.org/partner/glycoepitope>`  

**Dataset**

```
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix bibo: <http://purl.org/ontology/bibo/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .

 # Species
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
 glycan:is_from_source <http://rdf.glycoinfo.org/source/{taxonomy_id}> ;

<http://rdf.glycoinfo.org/source/{taxonomy_id}>
 a glycan:Source ;
 dcterms:identifier "{taxonomy_id}" ;
 rdfs:seeAlso <http://identifiers.org/taxonomy/{taxonomy_id}> .


# Literature
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
 dcterms:references <http://rdf.glycoinfo.org/references/{pubmed_id}> ; #Article

<http://rdf.glycoinfo.org/references/{pubmed_id}>
 a bibo:Article ;
 dcterms:identifier "{pubmed_id}" ;
 rdfs:seeAlso <http://identifiers.org/pubmed/{pubmed_id}> . #PubMed URL


# External ID
<http://rdf.glycoinfo.org/glycan/{accession_number}> #Saccharide 
	glycan:has_resource_entry <http://rdf.glycoinfo.org/glycoepitope/{epitope_id}> .

<http://rdf.glycoinfo.org/glycoepitope/{epitope_id}>
	a glycan:Resource_entry ;
	rdfs:label	"GlycoEpitope" ;
	glycan:in_glycan_database glycan:Database_glycoepitope ;
	dcterms:identifier	"{epitope_id}" ;
	rdfs:seeAlso	<http://identifiers.org/glycoepitope/{epitope_id}> .

glycan:Database_glycoepitope 
	rdfs:label "GlycoEpitope".
```




### <a name="from GlycoNAVI">from GlycoNAVI 
[return to menu](#menu)

GRAPH : `<http://rdf.glytoucan.org/partner/glyconavi>`  

**Dataset**

<a name="pubchem"> PubChem  
[return to menu](#menu)


```
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .


<http://rdf.glycoinfo.org/glycan/{accession_number}>
	glycan:has_resource_entry 
 			<http://rdf.glycoinfo.org/pubchem/{sid}> ,
			<http://rdf.glycoinfo.org/pubchem/{cid}> .

<http://rdf.glycoinfo.org/pubchem/{sid}>
	a glycan:Resource_entry ;
	rdfs:label "PubChem SID" ;
	glycan:in_glycan_database　glycan:Database_pubchem ;
	rdfs:seeAlso <http://pubchem.ncbi.nlm.nih.gov/substance/{sid}> ;
	skos:exactMatch <http://pubchem.ncbi.nlm.nih.gov/substance/{sid}> ;
	dcterms:identifier "{sid}" .

<http://rdf.glycoinfo.org/pubchem/{cid}>
	a glycan:Resource_entry ;
	rdfs:label "PubChem CID" ;
	glycan:in_glycan_databse	glytoucan:Database_pubchem ;
	rdfs:seeAlso <http://pubchem.ncbi.nlm.nih.gov/compound/{cid}> ;
	skos:closeMatch	<http://pubchem.ncbi.nlm.nih.gov/compound/{cid}> ;
	dcterms:identifier "{cid}" .

glycan:Database_pubchem 
	rdfs:label "PubChem".
```


<a name="wwpdb"> wwPDB members  
[return to menu](#menu)

* PDBj 
* PDBe 
* RCSB 

```
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .

<http://rdf.glycoinfo.org/glycan/{accession_number}>
	a glycan:Saccharide ;
	glycan:has_resource_entry <http://rdf.glycoinfo.org/pdbj/{pdb_id}> ;
	glycan:has_resource_entry <http://rdf.glycoinfo.org/pdbe/{pdb_id}> ;
	glycan:has_resource_entry <http://rdf.glycoinfo.org/rcsb_pdb/{pdb_id}> .

<http://rdf.glycoinfo.org/pdbj/{pdb_id}>
	a glycan:Resource_entry ;
	rdfs:label	"PDBj" ;
	dcterms:identifier	"{pdb_id}";
	glycan:in_glycan_database  glycan:Database_pdbj ;
	rdfs:seeAlso	<http://pdbj.org/mine/summary/{pdb_id}> ;
	skos:exactMatch	<http://rdf.wwpdb.org/pdb/{pdb_id}> .


<http://rdf.glycoinfo.org/pdbe/{pdb_id}>
	a glycan:Resource_entry ;
	dcterms:identifier	"{pdb_id}";
	rdfs:label	"PDBe" ;
	glycan:in_glycan_database  glycan:Database_pdbe ;
	rdfs:seeAlso	<http://www.ebi.ac.uk/pdbe/entry/pdb/{pdb_id}> ;
	skos:exactMatch	<http://rdf.wwpdb.org/pdb/{pdb_id}> .

<http://rdf.glycoinfo.org/rcsb_pdb/{pdb_id}>
	a glycan:Resource_entry ;
	dcterms:identifier	"{pdb_id}";
	rdfs:label	"RCSB PDB" ;
	glycan:in_glycan_database  glycan:Database_rcsb_pdb ;
	rdfs:seeAlso	<http://www.rcsb.org/pdb/explore.do?structureId={pdb_id}> ;
	skos:exactMatch	<http://rdf.wwpdb.org/pdb/{pdb_id}> .

<http://rdf.wwpdb.org/pdb/{pdb_id}>
	dcterms:identifier	"{pdb_id}".
```

OK http://pdbj.org/mine/summary/3s0k  
OK http://www.ebi.ac.uk/pdbe/entry/pdb/3s0k  
OK http://www.rcsb.org/pdb/explore/explore.do?pdbId=3s0k  
OK http://www.rcsb.org/pdb/explore.do?structureId=3s0k   

OK http://rdf.wwpdb.org/pdb/3s0k  

<br/>
<br/>

<a name="wwpdb cc"> **In the case of CC**   
[return to menu](#menu)

* PDBj CC
* PDBe CC
* RCSB PDB CC

cc : chemical component

```
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .

<http://rdf.glycoinfo.org/glycan/{accession_number}>
	a glycan:Saccharide ;
	glycan:has_resource_entry <http://rdf.glycoinfo.org/pdbj-cc/{pdb_chem-comp_id}> ;
	glycan:has_resource_entry <http://rdf.glycoinfo.org/pdbe-cc/{pdb_chem-comp_id}> ;
	glycan:has_resource_entry <http://rdf.glycoinfo.org/rcsb_pdb-cc/{pdb_chem-comp_id}> .

<http://rdf.glycoinfo.org/pdbj-cc/{pdb_chem-comp_id}>
	a glycan:Resource_entry ;
	rdfs:label	"PDBj CC" ;
	dcterms:identifier	"{pdb_chem-comp_id}";
	glycan:in_glycan_database  glycan:Database_pdbj ;
	rdfs:seeAlso	<http://pdbj.org/chemie/summary/{pdb_chem-comp_id}> ;
	skos:exactMatch	<http://rdf.wwpdb.org/cc/{pdb_chem-comp_id}> .

<http://rdf.glycoinfo.org/pdbe-cc/{pdb_chem-comp_id}>
	a glycan:Resource_entry ;
	dcterms:identifier	"{pdb_chem-comp_id}";
	rdfs:label	"PDBe CC" ;
	glycan:in_glycan_database  glycan:Database_pdbe ;
	rdfs:seeAlso	<http://www.ebi.ac.uk/pdbe-srv/pdbechem/chemicalCompound/show/{pdb_chem-comp_id}> ;
	skos:exactMatch	<http://rdf.wwpdb.org/cc/{pdb_chem-comp_id}> .

<http://rdf.glycoinfo.org/rcsb_pdb-cc/{pdb_chem-comp_id}>
	a glycan:Resource_entry ;
	dcterms:identifier	"{pdb_chem-comp_id}";
	rdfs:label	"RCSB PDB CC" ;
	glycan:in_glycan_database  glycan:Database_rcsb_pdb ;
	rdfs:seeAlso	<http://ligand-expo.rcsb.org/pyapps/ldHandler.py?formid=cc-index-search&operation=ccid&target={pdb_chem-comp_id}> ;
	skos:exactMatch	<http://rdf.wwpdb.org/cc/{pdb_chem-comp_id}> .

<http://rdf.wwpdb.org/cc/{pdb_chem-comp_id}>
	dcterms:identifier	"{pdb_chem-comp_id}".

glycan:Database_pdbj
	rdfs:label	"PDBj" .

glycan:Database_pdbe
	rdfs:label	"PDBe" .

glycan:Database_rcsb_pdb
	rdfs:label	"RCSB PDB" .
```


### <a name="from UniCarbKB"> from UniCarbKB
[return to menu](#menu)

GRAPH : `<http://rdf.glytoucan.org/partner/unicarbkb>`  

**Dataset**

```
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .

<http://rdf.glycoinfo.org/glycan/{accession_number}>
	a glycan:Saccharide ;
	glycan:has_resource_entry <http://rdf.glycoinfo.org/unicarbkb/{structure_id}> .

<http://rdf.glycoinfo.org/unicarbkb/{structure_id}>
	a glycan:Resource_entry ;
	glycan:in_glycan_database  glycan:Database_unicarbkb ;
	dcterms:identifier	"{structure_id}";
	rdfs:label	"UniCarbKB" ;
	rdfs:seeAlso	<http://www.unicarbkb.org/structure/{structure_id}> .

glycan:Database_unicarbkb
	rdfs:label	"UniCarbKB" .
```

### <a name="from UniCarb-DB"> from UniCarb-DB
[return to menu](#menu)

GRAPH : `<http://rdf.glytoucan.org/partner/unicarb-db>`  

**Dataset**

```
@prefix rdfs:	<http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix glycan:	<http://purl.jp/bio/12/glyco/glycan#> .
@prefix glytoucan:	<http://www.glytoucan.org/glyco/owl/glytoucan#> .


<http://rdf.glycoinfo.org/glycan/{accession_number}>
	glycan:has_resource_entry <http://rdf.glycoinfo.org/glycan/resource-entry/unicarb-db/{structure id}> .
	
<http://rdf.glycoinfo.org/glycan/resource-entry/unicarb-db/{structure id}> 
	a glycan:Resource_entry ;
	glycan:in_glycan_database <http://rdf.glytoucan.org/partner/unicarb-db> ;
	rdfs:label "UniCarb-DB" ;
	dcterms:identifier "structure id" ;
	rdfs:seeAlso <http://unicarb-db.biomedicine.gu.se/msData/{structure id}> ;
	glytoucan:contributor <http://rdf.glycoinfo.org/glytoucan/contributor/userId/{user id}> ;
	glytoucan:date_registered "time stanp"^^xsd:dateTimeStamp ;
```


## <a name="SPARQL Query">SPARQL query
[return to menu](#menu)


Insert   
[from BCSDB](/system/sparql/insertSparqlPartnerBCSDB)  
[from GlycoEpitope](/system/sparql/insertSparqlPartnerGlycoEpitope)  
[from GlycomeDB](/system/sparql/insertSparqlPartnerGlycomeDB)  
[from GlycoNAVI](/system/sparql/insertSparqlPartnerGlycoNAVI)  
