---
title: Insert SPARQL for Partner 
author:
- Daisuke Shinmachi
date: 2016-05-31
layout: default
---

# BCSDB

**Clear GRAPH**

```
log_enable(2,1);
sparql clear graph <http://rdf.glytoucan.org/partner/bcsdb>;
```

**Source, Article & Resource entry**


```

log_enable(2,1);
sparql
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX bibo: <http://purl.org/ontology/bibo/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glycodb: <http://purl.jp/bio/12/database/>
PREFIX glytoucan: <http://www.glytoucan.org/glyco/owl/glytoucan#>

INSERT{
        GRAPH <http://rdf.glytoucan.org/partner/bcsdb>{
        ?saccharide glycan:is_from_source ?taxon_iri .
        ?taxon_iri a glycan:Source .
        ?taxon_iri rdfs:label ?taxon_name .
        }
}
FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/core>
FROM NAMED <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/bcsdb>
FROM <http://rdf.glycoinfo.org/bcsdb>
WHERE{
        # Glytoucan
        #VALUES ?AccessionNumber {"G00051MO"}
        ?saccharide glytoucan:has_primary_id ?AccessionNumber.

        # From glytoucan to glycome-db 
        GRAPH <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>{
        ?saccharide skos:exactMatch ?glycomedb .
        }

        # GlycomeDB  bcsdb id
        GRAPH <http://rdf.glycoinfo.org/glycome-db>{
        ?glycomedb glycan:has_resource_entry ?res .
        ?res glycan:in_glycan_database glycan:database_bcsdb .
        ?res dcterms:identifier ?b_id.
        BIND(str(?b_id) AS ?bcsdb_id)
        }

        # BCSDB
        GRAPH <http://rdf.glycoinfo.org/bcsdb>{

        # BCSDB URL
        ?url dcterms:identifier ?bcsdb_id .
        ?url glycan:in_glycan_database glycodb:bcsdb .
        ?bcsdb owl:sameAs ?url .
        ?ref glycan:has_glycan ?bcsdb .

        # Taxon
        ?ref glycan:is_from_source ?source .
        ?source a glycan:source_natural .
        ?source glycan:has_taxon  ?taxon .
        ?taxon <http://purl.uniprot.org/core/scientificName> ?taxon_name .

        BIND(STR(?taxon) AS ?strTaxon)
        FILTER CONTAINS(?strTaxon,"taxon_")
        BIND(MD5(?taxon_name) AS ?md5Taxon)
        BIND(IRI(CONCAT("http://rdf.glycoinfo.org/source/", ?md5Taxon)) AS ?taxon_iri)
        #BIND(ENCODE_FOR_URI(?taxon_name) AS ?encTaxon)
        #BIND(IRI(CONCAT("http://rdf.glycoinfo.org/source/", ?encTaxon)) AS ?taxon_iri)

        # Article
        ?ref glycan:published_in ?article .
        }
};
checkpoint;
commit WORK;


log_enable(2,1);
sparql
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX bibo: <http://purl.org/ontology/bibo/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glycodb: <http://purl.jp/bio/12/database/>
PREFIX glytoucan: <http://www.glytoucan.org/glyco/owl/glytoucan#>

INSERT{
        GRAPH <http://rdf.glytoucan.org/partner/bcsdb>{
        ?saccharide glycan:is_from_source ?taxon_iri .
        ?taxon_iri a glycan:Source .
        ?taxon_iri rdfs:label ?taxon_name .
        ?taxon_iri dcterms:identifier ?taxon_id .
        ?taxon_iri rdfs:seeAlso ?taxon_url .
        }
}
FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/core>
FROM NAMED <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/bcsdb>
FROM <http://rdf.glycoinfo.org/bcsdb>
WHERE{
        # Glytoucan
        #VALUES ?AccessionNumber {"G00051MO"}
        ?saccharide glytoucan:has_primary_id ?AccessionNumber.

        # From glytoucan to glycome-db 
        GRAPH <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>{
        ?saccharide skos:exactMatch ?glycomedb .
        }

        # GlycomeDB  bcsdb id
        GRAPH <http://rdf.glycoinfo.org/glycome-db>{
        ?glycomedb glycan:has_resource_entry ?res .
        ?res glycan:in_glycan_database glycan:database_bcsdb .
        ?res dcterms:identifier ?b_id.
        BIND(str(?b_id) AS ?bcsdb_id)
        }

        # BCSDB
        GRAPH <http://rdf.glycoinfo.org/bcsdb>{

        # BCSDB URL
        ?url dcterms:identifier ?bcsdb_id .
        ?url glycan:in_glycan_database glycodb:bcsdb .
        ?bcsdb owl:sameAs ?url .
        ?ref glycan:has_glycan ?bcsdb .

        # Taxon
        ?ref glycan:is_from_source ?source .
        ?source a glycan:source_natural .
        ?source glycan:has_taxon  ?taxon .
        ?taxon <http://purl.uniprot.org/core/scientificName> ?taxon_name .

        BIND(STR(?taxon) AS ?strTaxon)
        FILTER CONTAINS(?strTaxon, "uniprot")
        BIND(STRAFTER(?strTaxon, "http://purl.uniprot.org/taxonomy/") AS ?taxon_id)
        BIND(IRI(CONCAT("http://rdf.glycoinfo.org/source/", ?taxon_id)) AS ?taxon_iri)
        BIND(IRI(CONCAT("http://identifiers.org/taxonomy/", ?taxon_id)) AS ?taxon_url)

        # Article
        ?ref glycan:published_in ?article .

        }
};
checkpoint;
commit WORK;


log_enable(2,1);
sparql
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX bibo: <http://purl.org/ontology/bibo/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glycodb: <http://purl.jp/bio/12/database/>
PREFIX glytoucan: <http://www.glytoucan.org/glyco/owl/glytoucan#>

INSERT{
        GRAPH <http://rdf.glytoucan.org/partner/bcsdb>{
        ?saccharide dcterms:references ?article_iri.
        ?article_iri a bibo:Article .
        ?article_iri dcterms:identifier ?pubmed_id .
        ?article_iri rdfs:seeAlso ?pubmed_url .
        }
}
FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/core>
FROM NAMED <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/bcsdb>
FROM <http://rdf.glycoinfo.org/bcsdb>
WHERE{
        # Glytoucan
        #VALUES ?AccessionNumber {"G00051MO"}
        ?saccharide glytoucan:has_primary_id ?AccessionNumber.

        # From glytoucan to glycome-db 
        GRAPH <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>{
        ?saccharide skos:exactMatch ?glycomedb .
        }

        # GlycomeDB  bcsdb id
        GRAPH <http://rdf.glycoinfo.org/glycome-db>{
        ?glycomedb glycan:has_resource_entry ?res .
        ?res glycan:in_glycan_database glycan:database_bcsdb .
        ?res dcterms:identifier ?b_id.
        BIND(str(?b_id) AS ?bcsdb_id)
        }

        # BCSDB
        GRAPH <http://rdf.glycoinfo.org/bcsdb>{
        # BCSDB URL
        ?url dcterms:identifier ?bcsdb_id .
        ?url glycan:in_glycan_database glycodb:bcsdb .
        ?bcsdb owl:sameAs ?url .

        ?ref glycan:has_glycan ?bcsdb .
        ?ref glycan:is_from_source ?source .
        ?ref glycan:published_in ?article .

        # Article
        ?article a bibo:Article .
        ?article glycan:has_pmid ?pubmed_id .
        ?article owl:sameAs ?pubmed .
        BIND(STR(?pubmed) AS ?strPubmed)
        FILTER CONTAINS(?strPubmed, "pubmed")
        BIND(IRI(CONCAT("http://rdf.glycoinfo.org/references/", ?pubmed_id)) AS ?article_iri)
        BIND(IRI(CONCAT("http://identifiers.org/pubmed/", ?pubmed_id)) AS ?pubmed_url)

        # Taxon
        ?source a glycan:source_natural .
        }
};
checkpoint;
commit WORK;


log_enable(2,1);
sparql
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX bibo: <http://purl.org/ontology/bibo/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#> 
PREFIX glycodb: <http://purl.jp/bio/12/database/>
PREFIX glytoucan: <http://www.glytoucan.org/glyco/owl/glytoucan#>

INSERT{
        GRAPH <http://rdf.glytoucan.org/partner/bcsdb>{
        ?saccharide glycan:has_resource_entry ?rEntry_iri.
        ?rEntry_iri a glycan:Resource_entry.
        ?rEntry_iri rdfs:label "BCSDB" .
        ?rEntry_iri glycan:in_glycan_database glycan:Database_bcsdb.
        ?rEntry_iri dcterms:identifier ?bcsdb_id.
        ?rEntry_iri rdfs:seeAlso ?bcsdb_url.
        glycan:Database_bcsdb rdfs:label "BCSDB".
        }
}
FROM <http://rdf.glytoucan.org>
FROM <http://rdf.glytoucan.org/core>
FROM NAMED <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/glycome-db>
FROM NAMED <http://rdf.glycoinfo.org/bcsdb>
FROM <http://rdf.glycoinfo.org/bcsdb>
WHERE{
        # Glytoucan
        #VALUES ?AccessionNumber {"G00051MO"}
        ?saccharide glytoucan:has_primary_id ?AccessionNumber.

        # From glytoucan to glycome-db 
        GRAPH <http://rdf.glycoinfo.org/mapping/glytoucan/glycome-db>{
        ?saccharide skos:exactMatch ?glycomedb .
        }

        # GlycomeDB  bcsdb id
        GRAPH <http://rdf.glycoinfo.org/glycome-db>{
        ?glycomedb glycan:has_resource_entry ?res .
        ?res glycan:in_glycan_database glycan:database_bcsdb .
        ?res dcterms:identifier ?b_id.
        BIND(str(?b_id) AS ?bcsdb_id)
        }

        # BCSDB
        GRAPH <http://rdf.glycoinfo.org/bcsdb>{

        # BCSDB URL
        ?bcsdb_url dcterms:identifier ?bcsdb_id .
        BIND(IRI(CONCAT("http://rdf.glycoinfo.org/bcsdb/", ?bcsdb_id)) AS ?rEntry_iri)

        ?bcsdb_url glycan:in_glycan_database glycodb:bcsdb .
        ?bcsdb owl:sameAs ?bcsdb_url .
        ?ref glycan:has_glycan ?bcsdb .

        # Taxon
        ?ref glycan:is_from_source ?source .
        ?source a glycan:source_natural .

        # Article
        ?ref glycan:published_in ?article .

        }
};
checkpoint;
commit WORK;
```
