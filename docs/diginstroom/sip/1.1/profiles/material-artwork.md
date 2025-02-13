---
layout:       default
title:        Material artwork
parent:       Profiles
grand_parent:  1.1
nav_order:    3
nav_exclude:  false
---
Release Candidate
{: .label .label-blue }
# Profile: Material artwork 

The _Material artwork_ profile supports digital reproductions of artworks that are moveable material objects, often, but not always, displayed in or maintained by museums. 
This includes photographic registration of 2D artworks, such as paintings or drawings, in high and very high (gigapixel) resolution and high-polygon scans of 3D artworks, such as statues or sculptures.

This content profile specifies how to package different media files (e.g., TIFF, JPEG, OBJ or MTL), their metadata and the relationships between them in a meemoo SIP package.
It also allows extensions to the descriptive metadata using [Schema.org](https://schema.org).

**Permalink:** <https://data.hetarchief.be/id/sip/1.1/material-artwork>

## Example Directory structure

### 2D photoregistration

```plaintext
root_directory
│── manifest-md5.txt
│── bagit.txt
│
└── data
    │── mets.xml
    │── metadata
    |   |── descriptive
    |   |   └── dc+schema.xml
    |   └── preservation
    |       └── premis.xml
    │
    └── representations
        └──representation_1       # overview with frame
           │── mets.xml
           └──data
           │  └── PID_overzichtsopname_metlijst_tiff.tiff
           │
           └──metadata
              |── descriptive     (optional)
              |   └── dc+schema.xml
              └──preservation
                 └── premis.xml
        └──representation_2       # overview without frame
           │── mets.xml
           └──data
           │  └── PID_overzichtsopname_zonderlijst_tiff.tif
           │
           └──metadata
              |── descriptive     (optional)
              |   └── dc+schema.xml    
              └──preservation
                 └── premis.xml
        └──representation_3       # composed stitch 
           │── mets.xml
           └──data
           │  └── PID_stitch_tiff.tif
           │
           └──metadata
              |── descriptive     (optional)    
              |   └── dc+schema.xml    
              └──preservation
                 └── premis.xml
        └──representation_4       # stitch 
           │── mets.xml
           └──data
           |  |── PID_deelopname1_tiff.tif
           |  |── PID_deelopname2_tiff.tif
           │  └── ...
           │
           └──metadata
              |── descriptive     (optional)  
              |   └── dc+schema.xml    
              └──preservation
                 └── premis.xml
```

### 3D scan

```plaintext
root_directory
│── manifest-md5.txt
│── bagit.txt
│
└── data
    │── mets.xml
    │── metadata
    |   |── descriptive
    |   |   └── dc+schema.xml
    |   └── preservation
    |       └── premis.xml
    │
    └── representations
        └──representation_1       # high-poly capture for print
           │── mets.xml
           └──data  
           │  └── PID_ARCH_STL.STL              
           │
           └──metadata
              |── descriptive     (optional)
              |   └── dc+schema.xml
              └──preservation
                 └── premis.xml
        └──representation_2       # high-poly capture
           │── mets.xml
           └──data
           |  |── PID_ARCH_OBJ.OBJ       # polygon file    
           |  |── PID_ARCH_TIFF_COLOR.TIFF      # texture image       
           │  └── PID_ARCH_MTL.MTL              # texture mapping file
           │
           └──metadata
              |── descriptive     (optional)
              |   └── dc+schema.xml
              └──preservation
                 └── premis.xml
        └──representation_3       # low-poly capture
           │── mets.xml
           └──data
           |  |── PID_VER_OBJ.OBJ       # polygon file    
           |  |── PID_VER_COLOR_BMP.BMP             # texture image       
           │  └── PID_VER_MTL.MTL                   # texture mapping file
           │
           └──metadata
              |── descriptive     (optional)
              |   └── dc+schema.xml   
              └──preservation
                 └── premis.xml
        └──representation_4       # quality assessment reference
           │── mets.xml
           └──data
           |  |── PID_REF_OBJ.OBJ               # polygon file    
           |  |── PID_REF_BMP.BMP               # texture image      
           |  |── PID_REF_IJK_BMP.BMP           # reference texture image     
           │  └── PID_REF_MTL.MTL               # texture mapping file
           │
           └──metadata
              |── descriptive     (optional)
              |   └── dc+schema.xml    
              └──preservation
                 └── premis.xml
```

## Requirements

### General

- There MUST be exactly one IE at the root. There MAY be other sub-IE's that are part of the root IE or other sub-IE's (e.g. to describe panels of a triptych individually).
- There MUST be at least one representation, but there MAY be multiple: either containing different captures or resolutions of the same IE or representing a different IE.
- Each representation MUST contain at least one file. 
- Preservation metadata MUST be limited to the PREMIS metadata schema.
- There MUST be preservation metadata at the package level in the `preservation/premis.xml` file.
- There MUST be preservation metadata at the representation level in the `preservation/premis.xml` file.

### Package METS

- The `csip:CONTENTINFORMATIONTYPE` attribute MUST be set to `https://data.hetarchief.be/id/sip/1.1/material-artwork`.
- The `TYPE` attribute in the `mets.xml` file MUST be set to `Physical object`.
- The `mets/dmdSec/mdRef/@MDTYPE` attribute MUST be set to `OTHER`.
  
<!-- 
The E-ARK (C)SIP spec verplicht blijkbaar OTHERMDTYPE niet als MDTYPE de waarde "OTHER" bevat:
https://earkcsip.dilcis.eu/. Later nog toevoegen indien nodig?
-->

### Descriptive Metadata

- A descriptive metadata file `descriptive/dc+schema.xml` describing the IE MUST be present at the package level.
- A descriptive metadata file `descriptive/dc+schema.xml` describing the representation MAY be present at the representation level (eg. to indicate diverting licenses). 
- Descriptive metadata in `descriptive/dc+schema.xml` MUST apply the DCTERMS metadata schema and MAY also apply the [SCHEMA](http://schema.org) metadata schema (see below).
- The [DCTERMS](https://www.dublincore.org/schemas/xmls/qdc/dcterms.xsd) metadata MUST follow the [basic profile requirements]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/profiles/basic.md %}#dc-requirements) regarding the use of elements and attributes.
- The [SCHEMA](http://schema.org) metadata in `descriptive/dc+schema.xml` MUST be limited to the elements and attributes outlined below.

| Element | `schema:creator` |
|-----------------------|-----------|
| Name | Creator artwork |
| Description | The creator/author of the digitally reproduced artwork.  |
| Cardinality | 0..* |
| Obligation | MAY |

| Attribute | `schema:creator/@roleName` |
|-----------------------|-----------|
| Name | Role creator |
| Description | The role with which the creator/author was involved in creating the digitally reproduced artwork.  |
| Cardinality | 0..1 |
| Obligation | SHOULD |

| Element | `schema:creator/schema:name` |
|-----------------------|-----------|
| Name | Name creator |
| Description | The name of the  creator.  |
| Cardinality | 1..1 |
| Obligation | MUST |

| Element | `schema:creator/schema:birthDate` |
|-----------------------|-----------|
| Name | Birth date creator |
| Description | The creator's date of birth.  |
| Datatype | [EDTF]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/2_terminology.md %}#edtf) |
| Cardinality | 0..1 |
| Obligation | MAY |

| Element | `schema:creator/schema:deathDate` |
|-----------------------|-----------|
| Name | Death date creator |
| Description | The creator's date of death.  |
| Datatype | [EDTF]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/2_terminology.md %}#edtf) |
| Cardinality | 0..1 |
| Obligation | MAY |

| Element | `schema:height` |
|-----------------------|-----------|
| Name | Height artwork |
| Description | The measured height of the physical artwork.  |
| Cardinality | 0..1 |
| Obligation | MAY |

| Element | `schema:width` |
|-----------------------|-----------|
| Name | Width artwork |
| Description | The measure width of the physical artwork. |
| Cardinality | 0..1 |
| Obligation | SHOULD |

| Element | `schema:depth` |
|-----------------------|-----------|
| Name | Depth artwork |
| Description | The measured depth of the physical artwork. |
| Cardinality | 0..1 |
| Obligation | SHOULD |

| Element | `(schema:height|schema:width|schema:depth)/schema:value` |
|-----------------------|-----------|
| Name | Value |
| Description | The height, width or depth measurement value. |
| Cardinality | 1..1 |
| Datatype | [Integer]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/2_terminology.md %}#integer) |
| Obligation | MUST |

| Element | `(schema:height|schema:width|schema:depth)/schema:unitCode` |
|-----------------------|-----------|
| Name | height |
| Description | The unit of length measurement given using the [UN/CEFACT Common Code (3 characters)](http://wiki.goodrelations-vocabulary.org/Documentation/UN/CEFACT_Common_Codes). |
| Vocabulary | `MMT`, `CMT`, `MTR` |
| Datatype | [String]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/2_terminology.md %}#string) |
| Cardinality | 0..1 |
| Obligation | SHOULD |

| Element | `(schema:height|schema:width|schema:depth)/schema:unitText` |
|-----------------------|-----------|
| Name | height |
| Description | A string or text indicating the unit of the height or width measurement value. Useful if you cannot provide a standard unit code for `schema:unitCode`.  |
| Vocabulary | `mm`, `cm`, `m` |
| Datatype | [String]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/2_terminology.md %}#string) |
| Cardinality | 0..1 |
| Obligation | MUST |

| Element | `schema:artMedium` |
|-----------------------|-----------|
| Name | Art medium |
| Description | The material used to create the physical artwork, e.g. Oil, Watercolour, Acrylic, Linoprint, Marble, Cyanotype, Digital, Lithograph, DryPoint, Intaglio, Pastel, Woodcut, Pencil, Mixed Media, etc. |
| Datatype | [String]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/2_terminology.md %}#string) |
| Cardinality | 0..1 |
| Obligation | MAY |

| Element | `schema:artform` |
|-----------------------|-----------|
| Name | Artform |
| Description | The type of artform, e.g. Painting, Drawing, Sculpture, Print, Photograph, Assemblage, Collage, etc. |
| Datatype | [String]({{ site.baseurl }}{% link docs/diginstroom/sip/1.1/2_terminology.md %}#string) |
| Cardinality | 0..1 |
| Obligation | MAY |

### Validation

The XML files that are required by this profile can be validated using the following XML schema definitions:

| File | Format | XML Schema |
| `mets.xml` | METS v1.12.1 | [mets.xsd](https://www.loc.gov/standards/mets/mets.xsd) |
| `premis.xml` | PREMIS v3.0 | [premis-v3-0.xsd](https://www.loc.gov/standards/premis/v3/premis-v3-0.xsd) |
| `dc+schema.xml` | Dublin Core with Schema.org | dc+schema.xsd (not yet available) |

## Use Cases

Some use cases that implement this profile are:

{% include _usecases.liquid  %}
