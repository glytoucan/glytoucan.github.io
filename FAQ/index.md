---
title: FAQ
layout: default
---

# Frequently Asked Questions

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->
- [GlyTouCan Tips](#glytoucan-tips)
- [GlyTouCan Site](#glytoucan-site)
- [Registration](#registration)
- [Glycan Image](#glycan-image)
- [GlyTouCan Partner](#glytoucan-partner)
<!-- /TOC -->




## GlyTouCan Tips

---------------

* [How do I find all glycan structures in human that are registered in GlyTouCan?](http://code.glytoucan.org/rdf-ontology/sparql-queries/#find-glycan-structures-iupac-condensed-related-to-homo-sapiens)
* [How do I find all glycan structures in mouse that are registered in GlyTouCan?](http://code.glytoucan.org/rdf-ontology/sparql-queries/#find-glycan-structures-iupac-condensed-related-to-mus-musculus)


## GlyTouCan Site

---------------

**Q.** What is GlyTouCan?<br>
**A.** Please see the [Top page](http://www.glytoucan.org/).<br><br>

**Q.** What is the meaning of GlyTouCan?<br>
**A.** GlyTouCan is the international glycan structure repository.
Thus, it is named by a combination of “Glycan” and “Tou（糖）”, which means sugar in Japanese.<br>
The word “Can” of GlyTouCan also means a can in both English and Japanese, to indicate that it is a container where glycans are accumulated. Therefore, a can is used in the logo to indicate that it is a repository with accumulated glycan data.
In addition, a toucan is used as a character since the word “Toucan” is  used in GlyTouCan.<br>
Moreover, a toucan is sitting on a branch of glycan.<br><br>

**Q.** What can we do at the GlyTouCan home page?<br>
**A.** Please see the [Top page](http://www.glytoucan.org/).<br><br>

**Q.** Who can use the services linked from the GlyTouCan home page?<br>
**A.** Anyone who has access to the Internet can use the services linked from the GlyTouCan home page. However, registration is required to use some services.<br><br>

**Q.** How much does it cost to use the services linked from the GlyTouCan home page?
<br>
**A.** All of the services linked from the GlyTouCan home page are publicly available and free of charge.<br><br>

**Q.** When I use GlyTouCan, will my privacy such as access information be protected?<br>
**A.** GlyTouCan automatically receives certain information such as  IP address, pages viewed etc., from the user’s browser, by the user and user environment, and records onto GlyTouCan’s server. This information is used to detect system attacks by large amounts of access in the early stage and to improve services.<br>
Moreover, GlyTouCan might provide collected access information and analysis results to a third party after statistical proccessing of information.<br>
GlyTouCan will not include information that identifies an individual in the access information and analysis results.<br><br>

**Q.** How can I collaborate with the “GlyTouCan”?<br>
**A.** Please inquire from [Contact Us]( mailto:support@glytoucan.org) and let us know how we may collaborate.<br><br>

**Q.** Can I link to one or several services available from the GlyTouCan home page?<br>
**A.** Yes, links can be used in your web site and does not require any permission.<br>
Please read the Site Policies of the GlyTouCan home page and Creative  
Commons licenses for terms and conditions of use.<br>
When you post links of GlyTouCan, please attribute links to the GlyTouCan URL as a link source by noting “GlyTouCan”.<br>
In addition, please inform us of any link(s) from [Contact Us]( mailto:support@glytoucan.org).<br><br>

**Q.** Can I use data and graphics from the GlyTouCan?<br>
**A.** Yes, data and graphics can be used in your web site and does not require any permission. However, redistribution of some of the resources in part or may be restricted.<br>
Please read and follow the [Site Policies](http://code.glytoucan.org/manual/sitePolicy) of the GlyTouCan home page and [Creative Commons licenses](https://creativecommons.org/licenses/by/4.0/).<br>
When you reprint and/or quote data and graphics from the GlyTouCan, please attribute it to “GlyTouCan”.<br>
In addition, please inform us of link(s) to your work [Contact Us]( mailto:support@glytoucan.org).<br><br>

**Q.** Where should I contact for inquiries for services of the GlyTouCan?<br>
**A.** Please contact us via [Contact Us]( mailto:support@glytoucan.org).<br><br>

**Q.** Do you have a tutorial on how to use this site?<br>
**A.** Please see the [User Guide](http://code.glytoucan.org/manual/).<br><br>

**Q.** What is the "reducing end" column that appears on the motif list and search pages?<br>
**A.** For motifs, this is used to determine if the root monosaccharide must be the same root for the motif-patterned structures.<br><br>

**Q.** What would happen if I don't have linkage position information?<br>
**A.** The position would be considered ambigous.  It would be registered as ambiguous and when searching, the similar structures with any linkage position will be queried.<br><br>

**Q.** How are aglycons supported?<br>
**A.** Currently aglycons and glycoconjugates are not supported.  However we do plan to support them in the near future.<br><br>

**Q.** Is there a way to filter to those glycans that have a calculated mass? <br>
**A.** Yes, currently this is supported on the [Glycan List](https://glytoucan.org/Structures) by clicking on the box to the left.<br><br>


## Registration

---------------

**Q.** When I signin to GlyTouCan and submission with a WURCS sequence for the first time, but I got error code.<br>

> status:500 Internal Server Error "401 null"

**A.** Please check if you have an API key in your Profile page (https://glytoucan.org/Users/profile)
after logging in.  It is possible that this variable is empty.  You first need to click on “Generate API Key” in the Profile page. <br><br>


**Q.** When I register a publication for the GlyTouCan accession that I registered, it was not able to register.   
**A.** Sorry, this is known issue. We are considering measures, currently.  

## Glycan Image

---------------
**Q.** Why does the image for a glycan entry not match the IUPAC string? <br>
**A.** The image generation software is currently not multi-threaded, causing the same image to appear for multiple entries due to a caching problem.  This can be resolved by right-clicking on the image, opening it in a new browser window or tab and refreshing it.  Refreshing may require holding down the control key while refreshing.<br><br>


## GlyTouCan Partner

---------------
**Q.** What is a GlyTouCan partner? <br>

**A.** The GlyTouCan partner is a partner program that enables cross-reference between GlyTouCan entry and glycan related databases id. <br><br>

Details on GlyTouCan partner program can be found on the page
* [GlyTouCan Partner Program](http://code.glytoucan.org/partner/)


**Q.** Want to become a GlyTouCan Partner and link your glycan structure data with us? <br>

**A.** You can register as a GlyTouCan partner using this google form.


  - [GlyTouCan Partner Registration Request](http://code.glytoucan.org/partner/registration/)



 Once you have registered as the GlyTouCan partner, you can register your glycan structure data by the partner account that is registered as the partner by email.


If you register soume glycan structures with your parnte ID (glycan ID in you database),  it is necessary that you use the Command Line Interface (CLI) . The detail is describles in following page.
* [GlyTouCan Command Line Interface to API](http://code.glytoucan.org/system/cli/)
