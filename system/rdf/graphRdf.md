---
title: GRAPH RDF
layout: default
---


## GRAPH RDF

**Triple**

```
<http://rdf.glytoucan.org/partner/bcsdb>
    rdfs:label "BCSDB".

<http://rdf.glytoucan.org/partner/glycome-db>
    rdfs:label "GlycomeDB".

<http://rdf.glytoucan.org/partner/glycoepitope>
    rdfs:label "GlycoEpitope".

<http://rdf.glytoucan.org/partner/glyconavi>
    rdfs:label "GlycoNAVI".
```


**Insert query**

```
log_enable(2,1);
sparql
INSERT DATA{
        GRAPH <http://rdf.glytoucan.org/graph> {
                
        <http://rdf.glytoucan.org/partner/bcsdb>
                rdfs:label "BCSDB".

        <http://rdf.glytoucan.org/partner/glycome-db>
                rdfs:label "GlycomeDB".

        <http://rdf.glytoucan.org/partner/glycoepitope>
                rdfs:label "GlycoEpitope".

        <http://rdf.glytoucan.org/partner/glyconavi>
                rdfs:label "GlycoNAVI".

        }

};
checkpoint;
commit WORK;
```


**Clear graph**

```
log_enable(2,1);
sparql clear graph <http://rdf.glytoucan.org/graph>;
```