---
title: Motif System Architecture
layout: default
---

## Overview

The analysis of this system began when a specific motif had an issue and it was found necessary to modify it.  At the time a static list of motifs was defined (given the ``` a glycan:Motif ``` class) which was also mistakingly ``` a glycan:Saccharide ```.  This lead to the Motif having a registered accession number. 

It was apparent that there were support and maintenence issues with this method.  It could not be sustained effectively which would eventually make this functionality insignificant.  Despite the fact that motifs are commonly describe in published literature.  [Here is one example](http://www.ncbi.nlm.nih.gov/pubmed/15535969).

As described above, there were also issues with considering Motifs to be Saccharides themselves, and registering them in the repository.


As of this writing, there does not exist a de-facto standard for naming glycans.  Even within the cheminfo organizations, there only exist proposals of standards.

As a glycan repository, it is important for the system to be able to associate names and naming conventions to specific structures or structural patterns.  This will allow for easier queries and recognizable associations for users and contributors.

This document describes a system architecture designed to be flexible enough to handle contributions of names used by different organizations, as well as an attempt associate the formalizing standards that are or will be used by glycan and other biochemical researchers.

### Vocabulary

To simplify the wording used for the scope of this document, please note the following definitions.

#### What is a Glycan

A glycan is any structure that can be registered into the Glycan Repository.  This is described as any structure that is unique ["at any level of detail or uncertainty"](http://glycob.oxfordjournals.org/content/23/12/1422.long){:target="_blank"}.  This would not include glycan fragments nor aglycon information.

#### What is a Structure

A "structure" is used for any structural sequence clearly definable by the glycan sequence textual format WURCS.

#### What is a Glycan Motif

A Glycan motif is a pattern identified within glycan structures that have been associated with a specific function and thus given a name.  It is important to note that motifs need to be defined as any location of an entire glycan structure, or only connected at the root or reducing end of the tree.

### Detailed Scenarios

Before describing a technical system workflow and data schema to support the naming of glycans, different scenarios will be described to explain the complexity of the problem.  The parts of the architecture that resolve each issue will be explained for each scenario.  

#### Monosaccharides

The base case scenario for the glycan repository would be the registration of a monosaccharide.  [MonosaccharideDB](http://www.monosaccharidedb.org){:target="_blank"} explains this best on the [notation  page](http://www.monosaccharidedb.org/notation.action){:target="_blank"}.  MonosaccharideDB not only incorporates a primary and trivial name schema, it also defines the source of the name or the type with the ```glycan:has_monosaccharide_notation_scheme``` predicate. 

glycan:has_monosaccharide_notation_scheme
              "glycan:monosaccharide_notation_scheme_glycoct"^^<http://www.w3.org/2001/XMLSchema#string> ;
              
                glycan:has_monosaccharide_notation_scheme
              "glycan:monosaccharide_notation_scheme_glycosciences_de"^^<http://www.w3.org/2001/XMLSchema#string> ;
              
                    glycan:has_monosaccharide_notation_scheme
              "glycan:monosaccharide_notation_scheme_carbbank"^^<http://www.w3.org/2001/XMLSchema#string> ;
              
                    glycan:has_monosaccharide_notation_scheme
              "glycan:monosaccharide_notation_scheme_glycosciences_de"^^<http://www.w3.org/2001/XMLSchema#string> ;
              
      glycan:has_monosaccharide_notation_scheme
              "glycan:monosaccharide_notation_scheme_monosaccharidedb"^^<http://www.w3.org/2001/XMLSchema#string> ;

[CFG notation](http://www.functionalglycomics.org/static/consortium/Nomenclature.shtml)
[PGA Nomenclature](http://glycomics.scripps.edu/coreD/PGAnomenclature.pdf)
[Polysaccharide Nomenclature](http://pac.iupac.org/publications/pac/pdf/1982/pdf/5408x1523.pdf)

Sulfates:
[Oligosaccharide chains](http://www.jbc.org/content/257/7/3347.full.pdf)

## Base Case

The base case is where the repository is completely blank.  In this case there is no structure nor image information.  So the entire process should start by the registration of a new structure.

## Registration

[RDF Datasets](https://www.w3.org/TR/rdf-sparql-query/#rdfDataset)

Before registration, the generation of the image is required before submitting the structure.  This is a critical aspect of the registration process, as it confirms the structure to be registered.  This is currently created by hex-encoding the image and then displayed on the browser.

The hex-encoding of the image is generated by the GlycanBuilder Library.  After confirmation, the user can submit the structure, which will then be given an accession number.

Once registered, the GlycanBuilder library is used to generate an image file in png and svg formats.  All of the glycoinformatic formats possible should be created, using CFG, IUPAC, Oxford, or any combination.

The newly registered structure will also store the related data for the image in RDF.

The glycoRDF, FOAF, schema and DC owl will be used.

glycoRDF details can be found [here](http://code.glytoucan.org/rdf-ontology/rdf-doc.html#image).

RDF for image data is covered in detail [here](http://www.kanzaki.com/docs/sw/image-rdf.html).

However there is also image data that is recognized by search engines in the [schema.org ontology](http://schema.org/ImageObject).

The ImageRdf class is used to extract the information required for these ontologies.

Here is an example of a structure ID G00054MO being registered.  Note the dc:creator and URI used specifies the program used to generate the image.

GlycoRDF:

```
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>
<http://rdf.glycoinfo.org/glycan/G00054MO> a glycan:saccharide ;
  glycan:has_image <http://rdf.glycoinfo.org/glycan/glycan/G00054MO/glycanbuilder/image/png/cfg> .

PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>
<http://rdf.glycoinfo.org/glycan/G00054MO/glycanbuilder/png/cfg> a glycan:image ;
 glycan:has_symbol_format	glycan:symbol_format_cfg ;
```

dc:

```
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>

<http://rdf.glycoinfo.org/glycan/G00054MO/glycanbuilder/png/cfg> a glycan:image ;
dc:title "GlyTouCan registered structure ID: G00054MO" ;
dc:creator "GlycanBuilder v1.0" ;
dc:date "1996" ;
dc:description "GlyTouCan registered structure ID: G00054MO." ;
dc:format	"image/png"^^xsd:string ;

```

foaf:

```
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>
<http://rdf.glycoinfo.org/glycan/G00054MO> a glycan:saccharide ;
foaf:thumbnail "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAALoAAABSCAIAAABse1lJAAACX0lEQVR42u3boW7CUBSA4ZtJgiBZsiAQCAQSgUAieIhJRAWiEoEhQZCAQCAQ8AgkCCQCh1mCQCCQPAIC0WSI7WQ1JNtYYVvPKfn/9AGay8ftvdzi3ogi5xgCggvBheBCcCG4EFwYAoILwYXgQnAhuBBcGAKCC8GF4EJwIbgQXBgCggvBheBCcCG4EFwYAoILwYXgQnBJfKfTiXuLlYv7i1QGZbVaFQqF3W5n0MpmsymVSlpi/pvL6+8up2Ll0T16zsvlctbEiJJisTgYDO51dkkYl9DKxE1e3EvP9ayJWSwWwuWOH0ZJ4nJuJbysiWm1Wp1O5z6Xusni8tmKQTH1er3f78NFmct3VqyJ8TxvPB7DRZPLZSumxMjU4vs+XNS4RLFiR4wsdcvlMlx0uES3YkTM8XhMp9NBEMAlbi7XWjEiplqtzudzuMTK5TYrFsQMh0PZH8Hlay7/1IN7aLv2DVbC69k9O71SqRSHALHOLrPZ7Mk9Td30BisyJ2WzWZmftL7ilUpluVzCJda1y21i1K1I3W630WjAJe6d0bViLFiRZNmUz+fhovC7S3QxRqyEZTKZ/X4PF4VfdaOIMWVFqtVqcttw0TkzuizGmhWp2WyqnDXC5QcxBq1Ivu+rvMkAl0tibFp5+3iTYTQawUX5bbpzMWatBEEgO6P1eg0X/Xd1QzFt17ZpRZLHkNa5NP8E+FqM7FRtWjkcDuJ4u93eG5dEJ58K9wYXggvBheBCcCG4EMGF4EJwIbgQXAguRHAhuBBcCC4EF4ILEVwILgQXggvBheBCBBeCC8GF4ELJ7h28Fpg2gJRZSwAAAABJRU5ErkJggg==" .

```

schema.org:

```

<div vocab="http://schema.org/" typeof="ImageObject">
  <h2 property="name">Beach in Mexico</h2>
  <span rel="contentURL"><img src="mexico-beach.jpg" /></span>

  By <span property="author">Jane Doe</span>
  Photographed in
    <span property="contentLocation">Puerto Vallarta, Mexico</span>
  Date uploaded:
    <span property="publishDate" content="2008-01-25">Jan 25, 2008</span>

  <span property="description">I took this picture while on vacation last year.</span>
</div>
```

## Batch Processes

In the case of volume registrations, a batch upload process should also be considered.  This process will reuse the registration procedure to generate the image RDF for a large volume of structures.

Batch process class name:

```
org.glycoinfo.rdf.batch.image
```

## Web Service

The image binary web service will be altered so that when a specific image of an ID is requested, the RDF is checked for the binary and that data will be retrieved.  The web service will handle the parameters to find the specific format requested.

If it is not found, then a not found image will be displayed.


## Retrieval

Once the above image data is stored, it is possible to retrieve the image using sparql.

The current image web service allows for the following to display a binary image:

```
https://glytoucan.org/glycans/G00029MO/image?format=png&notation=cfg&style=extended
```
This is implemented in the following class:

```
org.glytoucan.ws.controller.GlycanController.getGlycanImage(String, String, String, String)
```

It currently retrieves the glycoct and executes the GlycanBuilder generation process to create them.

Instead the hex-encoded data can be retrieved directly using sparql.  This can then be converted into a binary image and displayed.

```
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>
SELECT ?data
WHERE {
  ?glycan glycan:has_primary_key "G00054MO" .
  ?glycan glycan:has_image ?glycanImage .
  ?glycanImage foaf:thumbnail ?data .
  ?glycanImage glycan:has_symbol_format	glycan:symbol_format_cfg ;
  ?glycanImage dc:format "image/png"^^xsd:string ;
}

```

## Conclusion

By storing the hex-encoded data directly into the RDF, the image-generation dependency and point of failure is removed.  It is also possible to alter the registration process to completely regenerate all images using a different generation program.

Once the wurcs image generation program is ready, it can be quickly integrated into this system to have a robust method of displaying structure images.
