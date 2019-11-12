---
layout: main
title: nmrCV
nav:
  specs: active
---

You can view the documentation and download current and past releases here:

<table class="table table-hover">
<thead>
<tr><th>nmvCV Version</th><th>Documenation</th><th>Download</th></tr>
</thead>
<tbody>
<tr><td>v1.1.0</td><td><a href="/cv/jOWL/">jOWL Treeview</a></td><td><a href="/cv/v1.1.0/nmrCV.owl">nmrCV.owl</a></td></tr>
</tbody>
</table>



### nmrCV Overview

The nmrCV.owl ontology momentarily contains ~ 600 classes, most of them under nmr namespace but others imported (via MIREOT in Ontofox) from the units, Chebi, ChemO, PSI-MS, OBI and BFO top level ontologies.

We choose the OWL Syntax over the OBO format as exchange syntax for the CV, as the OBO tools are instable, the OBO format is only established in the biology domain (lack of off-the-shelf development tools, OBO expressivity is not as formal as OWL-DL) and there are hence less resources to integrate with.

We maintain a simple term taxonomy but have recently added mild Description Logics (DL) axiomatisations to key classes, i.e. for cases where DL-reasoning can help maintaining multiple parenthood and consistency checking of ontological concepts.

#### Minimal metadata on a CV term

Representational Unit (RU) metadata is captured via standardized owl annotation properties drawn from imported artefacts like DC, SKOS and Information Artefact Ontology (IAO). Not all of our terms currently have natural language definitions as these are time-intensive. None has deeper provenance data explicitly annotated (there is only an implicit indication on from which predecessor CV a term came in the ID ranges). We try to avoid getting stuck in the meta-ether, and have been pragmatic here.

A term batch submission table, i.e. to be gathered by users when requesting a bunch of missing CV terms for inclusion into nmrCV, should have the following mandatory fields:

* term name (rdfs:label)-->skos:prefLabel,ideally adhering to labelling best practice descibed at  http://www.obofoundry.org/wiki/index.php/Naming

* term definition in natural language (IAO_0000115), but also currently uses skos:definition and <rdfs:comment>def:

* superclass (ideally a term from the current nmrCV.owl, or an own suggestion)

Optional fields (good to have) are:

* synonym (oboInOwl:hasExactSynonym), but also currently uses skos:altLabel

* term definition source (obo:IAO_0000119), but also currently uses dc:source

* term editor (obo:IAO_0000117), but also currently uses dc:creator-->dc:author

Here is an example (bold) of the definition of the 'acquisition nucleus' term (NMR:1400083) in the NEW form (using IAO_0000115 to capture metadata and incl a DL axiom):

```xml
     <owl:Class rdf:about="http://nmrML.org/nmrCV#NMR:1400083">
        <rdfs:label rdf:datatype="&xsd;string">acquisition nucleus</rdfs:label>
        <owl:equivalentClass>
            <owl:Restriction>
                <owl:onProperty rdf:resource="&obo;RO_0000087"/>
                <owl:someValuesFrom rdf:resource="http://nmrML.org/nmrCV#acquisition_nucleus_role"/>
            </owl:Restriction>
        </owl:equivalentClass>
        <rdfs:subClassOf rdf:resource="&obo;BFO_0000040"/>
        <obo:IAO_0000119>adapted from wikipedia:
http://en.wikipedia.org/wiki/Nuclear_magnetic_resonance_spectroscopy</obo:IAO_0000119>
        <obo:IAO_0000117>Philippe Rocca-Serra</obo:IAO_0000117>
        <obo:IAO_0000115 xml:lang="en">The nucleus of an element with a non null net sping, whose resonances are being recorded during an NMR spectroscopy experiment. Common NMR requirements include direct 1D and 2D proton-only NMR, direct observation of 13C NMR with 1H decoupling, direct observation of other nuclei such as 19F, 31P, 29Si, 31P, 27Al, and 15N (with or without 1H decoupling), triple resonance NMR (especially inverse triple resonance such as 1H observe, 13C and 15N decouple), and inverse 2D and 3D experiments such as HMQC and HMBC.</obo:IAO_0000115>
    </owl:Class>
```
Here is an example (bold) of the definition of the 'acquisition nucleus' term (NMR:1400083) in the old form (using comments):

```xml
    <owl:Class rdf:about="http://nmrML.org/nmrCV#NMR:1400083">
        <rdfs:label rdf:datatype="&xsd;string">acquisition nucleus</rdfs:label>
        <rdfs:subClassOf rdf:resource="&obo;BFO_0000030"/>
        <rdfs:comment rdf:datatype="&xsd;string">def: The nucleus of an element or isotope that is being studied during an NMR analysis. Common NMR requirements include direct 1D and 2D proton-only NMR, direct observation of 13C NMR with 1H decoupling, direct observation of other nuclei such as 19F, 31P, 29Si, 31P, 27Al, and 15N (with or without 1H decoupling), triple resonance NMR (especially inverse triple resonance such as 1H observe, 13C and 15N decouple), and inverse 2D and 3D experiments such as HMQC and HMBC.</rdfs:comment>
    </owl:Class>
```

#### Top Level Ontology usage

There are a few top and upper level ontologies (TLO) already established. From BFO, OBILight &
 BioTopLight (btl2), we choose BFO as top level ontology to guide our CV upper level development. The reason was that it is abundantly used within existing bioontology frameworks. There has already been a case where the TLO provided modeling restrictions that allowed an automatic DL reasoner to discover CV modelling errors, e.g. https://github.com/nmrML/nmrML/issues/62

Nevertheless, at the moment we avoid frequent usage of object properties from the CV. E.g. for coding the vendor of an NMR instrument, we could have the following axiom in the CV:  ‘NMR Instrument’ hasVendor Vendor. But instead, we say in the mapping file that for an Instrument, the Name and Vendor has to be specified. In an equal way we amend CV information describing Software, e.g. the version info is stored in an XSD attribute.


