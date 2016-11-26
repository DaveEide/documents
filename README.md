# VISTA Data Project

The Veterans Information System Technology Architecture (VISTA) is the integrated, comprehensive, longitudinal health information system of the U.S. Department of Veterans Affairs (VA). For the past thirty-five years, 131 VISTA systems have provided all clinical, financial, and administrative functions to support the operations of over 1200 VA hospitals and clinics throughout the United States. See [VISTA Background](https://github.com/vistadataproject/documents/tree/master/Background/vista)


The VISTA Data Project is a new data-centric, model-driven approach to VA VISTA master data management and interfacing.  This is in contrast to the current code-centric approach to interfacing VISTA's data which relies on a byzantine array of thousands hard-coded opaque, brittle remote procedure calls (RPCs) which have accumulated over three decades - none of which are validated, documented, or maintained.  Such a code-centric approach does not provide a coherent, comprehensive, maintainable approach to master data management or interfacing to VISTA's data.

VISTA's master data model - the roadmap to all of VA's institutional, business process, and clinical know-how and data - has evolved organically over the past 35 years, but has not been surfaced and leveraged in computable form.  Now, for the first time, VISTA's data model will be comprehensively exposed, enriched, and operationalized as a single, secure, symmetric read-write, server-side interface to all VISTA data for external interfacing and integration. This data model uniformly bridges  all 131 existing VISTA system data models, allowing secure read-write access to all VA VISTA systems enterprise-wide using a single Master VISTA Data Model (__MVDM__).

### An Evolution in Interfacing
The first set of interfaces to migrate are those of the clinical domain. These are based on the interfaces to the clinical graphical thick client (__CPRS__), and are comprised of over one thousand remote procedure calls (__RPCs__).  Each of these CPRS RPCs will be incrementally audited, emulated, isolated, and secured in the __RPC Locker__, with all semantics reflected in the Master VistA Data Model (__MVDM__). The RPC Locker audits and blocks any code injection, and redirects all database access through the Fileman (database) API.  

Within the MVDM is a configurable set of patient-centric security policies. This is based on the logical isolation of patient, institutional, knowledge, and systems information. This logical isolation of patient data from all other kinds of data is a necessary foundation for patient-centric access control. 

__In Model-driven VISTA, interfacing is through the Master VistA Data Model (MVDM).__ For __existing CPRS clients__, security is enhanced and audited by the RPC Locker; then all reads and writes controlled through MVDM. For __new  clients and interfaces__, reads and writes are through MVDM.  __Authentication__ for all VISTA clients and interfaces is provided (*separately*) through Enterprise mechanisms.
<br>

#### VISTA Interfacing Transition
*The figure summarizes the evolution from __thousands of unique, inconsistent, insecure, unidirectional code-based interfaces__ to that of a __single, standard, secure, server-side, symmetric (bidirectional) data model-driven interface__ - the Master VistA Data Model (MVDM). 

VISTA's new single symmetric read/write interface (blue bidirectional arrow with Linked Data symbol) represents the embedded, in-process, server-side, security-enhanced, transactional Linked Data-driven Master VISTA Data Model (MVDM).*

![vdp-transition](https://github.com/vistadataproject/documents/blob/master/images/vdp-transition-20161119b.png)





Code-driven VISTA <br> (Current) | Model-driven VISTA <br> (VISTA Data Project)
---|---
__Current interfacing  to VISTA is through thousands (over 3300) of unique, opaque, one-way  (either read or write)  legacy (20+ years old)  MUMPS remote procedure calls (RPCs) which are neither documented nor maintained.__ These are hard-coded in MUMPS for specific clients only and not interchangeable to other clients due to business logic within the custom MUMPS and client code. |   __All interfacing to VISTA is through one single, secure, symmetric (bidirectional)  read-write Master VISTA Data Model (MVDM).__  The read data model is identical to the write data model (i.e. symmetric)  providing one single universal structured data read/write mechanism. 
__Access is based on the legacy (1980's) terminal interface and its Menu Actions.__ This is a nonstandard, bespoke, opaque mechanism hardwirted to permutations of permissions to use 9000 menu opttions within 1700 menus within a legacy terminal interface. Clearly this  is not applicable to any new, modern, generic, or graphical interfaces such as web or mobile. The only way to interface to data currently is to write new MUMPS code using the opaque Terminal-specific Actions-centric security, in addition to custom RPC MUMPS security code. | __New, modern, industry-standard, fine-grained access control__ is provided through patient-centric access control (PCAC; new blue layer).  All existing RPC-based clients (left) access the MVDM via the __RPC Locker__, an RPC isolation and audit security layer (new). All new clients and integrations (right) access the MVDM via PCAC security. Authentication (top layer) will leverage enterprise identity and authentication management with time-limited SAML tokens.  
__Many of the MUMPS RPCs bypass the Fileman API and Data Dictionary, writing direct to MUMPS global storage.__ Bypassing the Fileman API means that the security and auditing measures of  the database are bypassed, creating a significant security gap, in addition to currupted data dictionary.  This makes the data inaccessible to any other applications or by any other method other than by writing yet more custom MUMPS global data access code. | __All  interfaces and functionality are Model-driven,  language-agnostic, client-agnostic, and Fileman API compliant, assuring universal auditing of all data access.__ 




__Data Model Features:__
+ All interfacing is through a __single, secure, symmetric read-write Master VISTA Data Model__.
+ All interfaces are Model-driven, language-agnostic, client-agnostic, Fileman API compliant.
+ All interfaces are secured with both existing Kernel authentication, in addition to new modern, industry-standard, patient-centric, attribute-based access control (ABAC).
+ All interfaces are written using modern, web-standard languages and tools (Javascript). 
+ The read data model is identical to the write data model (i.e. symmetric), assuring completeness and correctness of both. 
+ All existing clients or interfaces (such as CPRS) continue to function unchanged on top of MVDM through the a RPC Locker (a new, security isolation locker for all RPCs).
+ All existing clients inherit all MVDM features, including enhanced patient-centric security and attribute-based access control (ABAC).


#### VISTA Interfacing Evolution

Interface |  Code-driven <br>MUMPS RPCs (x3500)  | Data model-driven<br>Master VISTA Data Model (x1)
--- | --- | ---
Method |   :no_entry_sign:  Relies on over 3500 client-specific, non-interchangeable legacy MUMPS routines <br>  :no_entry_sign: Distinct, unique routines for reading vs writing the same data <br>  :no_entry_sign: Requires extensive knowledge and experience with MUMPS and VISTA | :white_check_mark:  Data Model-Driven :new: <br> :white_check_mark: Client-agnostic :new: <br> :white_check_mark: One single, symmetric read-write mechanism for all data :new: <br>:white_check_mark: Requires no knowledge or experience with VISTA internals or MUMPS.
Ease of interfacing to new clients | :no_entry_sign: HARD | :white_check_mark: EASY
Security |  :no_entry_sign: Patchy, Opaque  | :white_check_mark:  Comprehensive, Clear
Authentication |  Kernel Access/Verify | :white_check_mark: SAML token
Access Control |  :no_entry_sign: Dependent on and specific to the legacy terminal interface Menu Options  | :white_check_mark:  Applicable to any and all (new) interfaces  <br>:white_check_mark:  Data-Centric; :new: <br> :white_check_mark:  Patient-Centric :new: <br>:white_check_mark:   Enables Attribute-Based Access Control (ABAC) :new:
Fileman API Compliant|  :no_entry_sign: Unreliable, Incomplete <br> :no_entry_sign: Variable compliance| :white_check_mark:  Reliable, Complete <br> :white_check_mark: 100% Compliant
Audit |   :no_entry_sign: Incomplete <br> :no_entry_sign: Bypassess Fileman auditing | :white_check_mark:  Comprehensive AND <br> :white_check_mark: Patient-Centric :new:  
Unit Tested  |   :no_entry_sign: NO <br>  :no_entry_sign:  0% logic tested  | :white_check_mark: YES <br> :white_check_mark: 100% logic validated
Documentation |  :no_entry_sign: Incomplete, inconsistent, unclear. <br> :no_entry_sign:  Requires understanding MUMPS code | :white_check_mark: Complete, consistent, clear.  <br>:white_check_mark: Core is machine generated  :new: 



##  VISTA Data Model:  Details
_Server-side. Security-enhanced. Symmetric-Read-Write._

![VDP-intro](https://github.com/vistadataproject/documents/blob/master/images/vdp-introA.png)



## VISTA Data Model: Features

*__An operationalized Master VISTA Data Model (MVDM) provides VA with three key transformational capabilities:__*

VISTA<br>Data | Details
---|---
 ![db-search](https://github.com/vistadataproject/documents/blob/master/images/logos/logos-db/50h/db-search.jpg) <br> __Access__ | __A single, universal, industry-standard mechanism for reading and writing *all VISTA data*.__ <br> This mechanism is unified by the read model and the write write model integrated into a single, symmetric-read-write data model (VDM), with all data in industry-standard web formats. *This overcomes the well understood shortcoming with VISTA Data Read and Write, which uses completely unique code, models, and mechanisms for reading data as distinct from writing data. Furthermore, the 20+ year old RPCs - over 3300 MUMPS routines which encapsulate all these idiosyncratic approaches (written *exclusively* and in lock-step with the the Delphi code of CPRS, and none of which are documented or maintained) simply cannot be relied on going forward, particularly for generic, external non-CPRS interfaces and clients.*
![db-integrity](https://github.com/vistadataproject/documents/blob/master/images/logos/logos-db/50h/db-integrityA.jpg) <br> __Integrity__| __Comprehensive, automated, standardized, strict data integrity enforcement for  *all VISTA data*.__ <br> *This is a major improvement over the hodgepodge of legacy, ad-hoc methods that have accumulated over the past 35 years (HL7, RPCs, MUMPS, procedural code), none of which are documented, and all of which are inconsistent, unpredictable, and highly permissive*.  See also: [Master Data Management](https://en.wikipedia.org/wiki/Master_data_management)
![db-security](https://github.com/vistadataproject/documents/blob/master/images/logos/logos-db/50h/db-security.jpg) <br> __Security__ | __Comprehensive, industry-standard, fine-grained, data-centric security for *all VISTA data*.__ <br> Currently VISTA provides security for only a small fraction of its data, and does this through bespoke, complex, opaque, and unmaintainable methods hardwired to a legacy terminal interface and its 9000+ terminal menu options. <br> __Data-centric, attribute-based security__ is the foundation for all other security levels and technologies, because without knowledge of the data and its logical attributes, it will not be possible to provide the appropriate security measures on the data. <br>__Through metadata enrichment of the VISTA Data Model__, VISTA will know *what categories of data it is managing* and thus allow, for the first time, comprehensive, data-centric, attribute-based security "on-the-data" for all VISTA data, permitting the secure exchange of data. See [Data-Centric Security](https://en.wikipedia.org/wiki/Data-centric_security),  [Logical Security](http://www.mdpi.com/1999-5903/4/4/929/htm#fig_body_display_futureinternet-04-00929-f001), [Semantic Security](https://www.google.com/search?q=semantic+data+security&sa=X&biw=1154&bih=1062&tbm=isch&tbo=u&source=univ&ved=0ahUKEwi_14b--JXLAhWKOz4KHWghAVEQsAQIgwE) and [Attribute-Based Access Control (ABAC)](http://csrc.nist.gov/projects/abac)


*Note: As a side-effect of establishing a single comprehensive mechanism for data management for VISTA data, a large portion of VISTA's legacy code (its thousands of data interfacing routines) may be retired.*



## VISTA Data Model: Attributes

Attribute | Details
---|---
__Representative__  | __VDM operationalizes all relevant VA VISTA data to the maximum extent available.__ <br> The VISTA Data Model comprises the current existing data-driven architecture of VISTA, and thus leverages all existing VISTA definitions. There is 100% correspondence and coverage of the internal data definitions of any local VISTA and that of its corresponding  VISTA Data Model (VDM), since these are maintained always in-sync and up-to-date. Any and all enhancements to any VISTA system and its data definitions will automatically be reflected in the VISTA Data Model through automated, triggered updates whenever VISTA's data dictionary is updated. 
__Real-Time__ | __VDM is operationalized using Best-of-Breed real-time server-side runtime technology.__<br> The same runtime technology that drives the largest commericial real-time high-traffic websites such as Walmart, eBay, PayPal, Netflix, Uber, LinkedIn, and the New York Times with millions of concurrent users also drives MVDM. *This maximizes transactional processing performance directly on the transactional database.*
__Noninvasive__ | __VDM provides VISTA with essential new functionality within the current VISTA architecture 'as is', without modification.__ <br>  No existing VISTA code, routines, packages, modules, infrastructure, or functionality will be affected or changed in any way (i.e. this is a 'safe'and 'noninvasive'). This keeps all existing functionality, while offering new, essential functionality for parallel development of all new web-oriented clients. In addition, it makes it easy and 'safe' to install, as this does not affect any current code or functionality.
__Self-Contained__ | __VDM runs server-side, in-process, directly within the existing VISTA/M transaction engine.__ <br> This eliminates all moving parts and maximizes transaction processing performance by running as an embedded process directly within the local M transactional database, fully leveraging the 'as-is' VISTA architecture. *This makes it extremely performant - at native speed of the MUMPS transaction engine. It also makes it easy to deploy and maintain. No moving parts. No external dependencies. No middleware. "On the metal" native M server-side performance*
__Data-Centric__ | __VDM is a completely new, purely data-centric approach to managing VISTA's data__. It does not involve changing a single line of VISTA's existing M procedural code, nor is it 'wrapping' (i.e. secretly using) any legacy code, routines, or RPCs dressed up within a shiny new encapsulation method (i.e. RPC wrapper "APIs"), which only add yet more obfuscation layers on the data. A data-centric approach __*comprehensively exposes all the data, which exposes the fact that VISTA has a data model*__ - which up to this point has not been realized nor taken advantage of. *This is the opposite of a code-centric approach, which obfuscates the data and its data model*.
__Web-Standard__ |  __VDM technologies are 100% web standard__ and all used in production settings by the worlds' largest corporations and organizations.  For further information see [standards and technologies](https://github.com/vistadataproject/documents/tree/master/Background#standards).
__Empiric Evolution__ | __VDM employs a new approach to emprically evolving VISTA's capabilities through rapid, iterative, functional prototypes.__ This allows the focus to remain on exploration of new techniques and approaches, rather than on more superficial end-user requirements, which rarely if ever attempt to tackle the deep conceptual and technological issues of data management. This is *the opposite waterfall development*.  See [spiral model](https://en.wikipedia.org/wiki/Spiral_model)



## Objective and Method of Delivery


__What?__

> The VA Information Systems Technology Architecture (VISTA) is VA's an integrated EHR and resource management system which provides all adminstrative, financial, and clinical information management to efficiently run over 1200 hospitals and clinics throughout the U.S., and thus provide veterans the highest quality of care, everywhere.  

> There are over 131 instances of VISTA deployed nationwide, and each has evolved independently over the past thirty-five years. The result is that each VISTA system has its own distinct database and distinct data model.  There is no single "VA system". There are 131. As a result, VA cannot share any computable data across or between any of the other VISTA systems.

__Why?__

> The mission of the Veterans Health Administration (VHA) is to provide comprehensive lifelong healthcare services to veterans everywhere. To support this, VA must have a seamless, comprehensive, nationally integrated healthcare information system to provide all relevant VISTA data in real-time in computable form at the bedside at all 1200 facilities.  In addition, in order to support the needs of veterans in today's mobile web-oriented world, VA needs to create new web-based clients and services to VISTA data to provide all necessary information to providers and veterans at the point of care using mobile, tablet, and web browser based interfaces to support truly ubiquitous access to healthcare services.

> VA thus needs a single, consistent, web-standard mechanism for real-time read-write transactions to all of the 131 local, unique VISTA systems as one, national master VISTA system.  This reduces the complexity of development, deployment, and maintenance for any new nationwide data service from any of the 131 distinct local VISTA systems to that of only one standardized computable Master VISTA system.

__How?__

> All sources of available metadata and models (internal to VISTA as well as external) will be transformed to a single integrated web-standard machine-processable data model which is then annotated, normalized, and enriched. This enhanced model is in turn is embedded back in VISTA as a server-side, security-enabled, symmetric read/write (i.e. transactional) Master Data Model.   

__Where?__

> All artifacts and deliverables shall be developed, version-controlled, stored, and delivered on an industry-standard public Github repository (“Project Repository”). ... The Project Repository shall contain the one and only authoritative version of all artifacts produced ... The government, all necessary stakeholders, and the public shall have full read and download access of all artifacts on the Project Repository at all times --- See [PWS](https://github.com/vistadataproject/documents/blob/master/Submissions/src/VistAMetadata-2015-12-09-PWS.pdf) Section 1.6.15.1



## Technologies
The VISTA Data Project is based on the following [Web Technologies](https://github.com/vistadataproject/documents/tree/master/Background#technologies) and  [Web Standards](https://github.com/vistadataproject/documents/tree/master/Background#standards):

![](/images/logos/logos-tech/square/60h/node-js.jpg)
![](/images/logos/logos-tech/square/60h/js5.jpg)
![](/images/logos/logos-tech/square/60h/html5.jpg)
![](/images/logos/logos-tech/square/60h/css3.jpg)
![](/images/logos/logos-tech/square/60h/rdf.jpg)
![](/images/logos/logos-tech/square/60h/jsonld.jpg)
![](/images/logos/logos-tech/square/60h/json.jpg)
![](/images/logos/logos-tech/square/60h/markdown.jpg)
![](/images/logos/logos-tech/square/60h/github.jpg)
![](/images/logos/logos-tech/square/60h/git.jpg)
![](/images/logos/logos-tech/square/60h/vagrant.jpg)
![](/images/logos/logos-tech/square/60h/CC.jpg)
![](/images/logos-tech/square/60h/asf.jpg)  

* Detailed review of these and other foundation technologies is contained in the [Background](https://github.com/vistadataproject/documents/tree/master/Background) section.
* The current baseline VISTA and Client functionality and capability is described [here](https://github.com/vistadataproject/documents/blob/master/Background/vista/vista-baseline.md).



## Technical Background

Technical decisions by the VA and in mainstream software industry that framed the approach taken here

1. By virtue of VA's technical review and approval of Node.js in the VA Technical Reference Model ([TRM](http://www.va.gov/TRM/ToolPage.asp?tid=6716)), VA endorses the use of server-side Javascript/Node in the  VA enterprise architecture. See [TRM-Node](http://www.va.gov/TRM/ToolPage.asp?tid=6716).
1. By virtue of VA's Enterprise Health Management Platform being rewritten almost entirely in Javascript and Node.js, the VA has decided that Node.js is essential for the success of enterprise projects.  The backdrop to this decision was the conspicuous failure of numerous mid-tier Java wrappers for VISTA, starting with MyHealtheVet and the others since then. See [reference](http://www.openhealthnews.com/story/2014-07-27/vista-evolution-whats-wrong-picture).
1.  By virtue of VA's large, multi-year [contract](https://www.fbo.gov/index?s=opportunity&mode=form&tab=core&id=2a9bd7f10699f046bd284a2ac28ccf9e&_cview=0) (and [see](https://www.google.com/search?q=%22Control%20Number%2015-038%22&rct=j)) for Node.js, the VA has decided that Node-enabled Javascript on MUMPs is productive and practical.
1. By virtue of inclusion of the Node in all official releases of Cache, Intersystems views in-process Javascript coding on Cache as practical, maintainable, and essential for their commercial customers, particularly VA. See Intersystems documentation on [Cache/Node](http://docs.intersystems.com/ens20141/csp/docbook/DocBook.UI.Page.cls?KEY=BXJS_intro) and [PDF](http://docs.intersystems.com/documentation/cache/20122/pdfs/BXJS.pdf).
1. Node.js adoption continues to grow for mainstream production projects, including Netflix, New York Times, PayPal, LinkedIn, Walmart, Yahoo, and Uber.
1. Javascript is the most popular coding language in the world, as  measured by number of projects, coders, and new code on Github, and by the number of companies developing and deploying enterprise software for consumption on the web.
1. By virtue of VA's technical review and approval in the VA Technical Reference Model of the Resource Description Framework ([RDF](https://www.w3.org/standards/techs/rdf#w3c_all)), VA  endorses the RDF data model for use in the VA enterprise architecture. See [TRM-RDF](http://www.va.gov/TRM/StandardPage.asp?tid=6405).
1. JSON-LD is the most commonly used form of RDF deployed in production settings. It is used by Google, Yahoo, and Microsoft as a common mechanism to structure and index all the data on the web by their search engines, and by the U.S. National Library of Congress and U.S. National Library of Medicine to structure and search all published books and medical journals, respectively.  See [JSON-LD](http://json-ld.org).


## Technical Overview

## Tracks

The Project organizes deliverables in five “tracks” each backed by one or more Gits in the Project Repository.

Track | Name | Description | Git | Technical Deliverables
:---: | :---: | :--- | :--- | :---:
A | Infrastructure | Project infrastructure including Test VISTA (“nodeVISTA”), gits, tooling, website | [nodeVISTA](https://github.com/vistadataproject/nodeVISTA), [Website](https://github.com/vistadataproject/vistadataproject.github.io), [documents](https://github.com/vistadataproject/documents) | 3
B | VDM | VISTA Data Model (VDM) - Comprehensive, native model exposure and package implementation for any specific VISTA | [VDM](https://github.com/vistadataproject/VDM) | 12
C | MVDM | Master VISTA Data Model (MVDM) - Definition and implementation of master data model spanning all VISTA instances | [MVDM](https://github.com/vistadataproject/MVDM) | 10
D | Demo | End to End Demonstration | [VDM](https://github.com/vistadataproject/VDM) | 1
E | Security | Security Model - Definition and implementation of patient-centric security model | [MVDM](https://github.com/vistadataproject/MVDM) | 10
PM | Project Management | Business/Project Management  | [documents](https://github.com/vistadataproject/documents) | &nbsp;


#### Formats and Licenses of Deliverables 

From PWS 8.2 ...

Artifact | Format(s) | License
:---: | :--- | :--- 
Data | CSV if tabular structure; JSON-LD for all other structures. | Creative Commons CC0
Metadata | JSON-LD (RDF) | Creative Commons CC0
Documents | Markdown (git Markdown or Docbook). From this HTML and PDF shall be auto-generated | Creative Commons CC0
Code (Software) | Source code, and all dependent code, with full version control history | Apache 2.0

The forms and licenses are in keeping with the requirement that <q>All artifacts and deliverables shall be developed, version-controlled, stored, and delivered on an industry-standard public Github repository (“Project Repository”)</q>.



## Technical Deliverables

29 Technical Deliverables involve:
  * 7 MetaData Definitions/System Configurations
  * 16 Software 
  * 6 Documents

More artifacts may be identified as work proceeds.

#### Metadata Definitions and System Configurations

\# | Name | Format | Function | Deliverable(s)
:---: | :---: | :---: | :--- | :---:
1. | dd.jsonld 	| JSON-LD | Formal, portable definition of the contents of a VISTA data dictionary | 8
2. | rpc.jsonld | JSON-LD |	Formal definition of the model implicit in RPCs, captured in JSON-LD | E1
3. | vpr.jsonld | JSON-LD | Formal definition of the VPR RPC's patient data model in JSON-LD | Part of 10.1
4. | vdm.jsonld | JSON-LD |	Formal definition of Native VISTA data model based on one or more dd.jsonld's and rpc.jsonld | 7.1, 7.2
5. | mvdm.jsonld | JSON-LD | Formal definition of the MVDM subset of VDM that supports full CRUD | 10.1, 10.2
6. | piks.jsonld | JSON-LD | Formal annotation of vdm.jsonld that distinguishes Patient, Institution, Knowledge and System (PIKS) classes and properties | 18
7. | nodeVISTA Scenarios | GT.M and Cache Databases | VISTA databases for testing and demonstrations | Part of E2.2 Development

#### Software

Note that as development proceeeds, the names and organization of software artifacts may change.

\# | Name | Language | Function | Deliverable(s) 
:---: | :---: | :---: | --- | :---: 
1. | DDJLD Maker | Python/ Javascript |	Caches FileMan Data Dictionary (dd) from a VISTA and creates a _dd.jsonld_ | 8 
2. | RPCJLD Maker | Python/ Javascript | Caches RPC definitions from a VISTA and creates an _rpc.jsonld_ | E1
3. | nodeVISTA | VISTA, node.js | A test VISTA based on OSEHRA's VISTA and a simple node.js front end | E3
4. | nodeVISTA Commands | Javascript (node.js) | invocations of mainly write-back functions in VISTA to prepare for the write-back support of _VDM Package_ | Part 7.2, E2.2
5. | VDM Maker | Python/ Javascript | Creates a VISTA Data Model (VDM), _vdm.jsonld_, from a VISTA's _dd.jsonld_ and _rpc.jsonld_ | 7.1, 7.2
6. | __VDM Package__ | Javascript (node.js module), MUMPS | Implements VDM inside Fileman. The first version will support querying ("Read-only"). The full version will support Create-Read-Update-Delete and transactions. | E2.1, E2.2
7. | MVDM Maker | Python/ Javascript | Creates a Master VISTA Data Model (MVDM), _mvdm.jsonld_, from one or more _vdm.jsonld_'s and _vpr.jsonld_ | 10.1, 10.2
8. | __MVDM Module__ | Javascript (node.js module) | Implements MVDM inside Fileman over the _VDM Package_. The first version will support querying ("Read-only"). The full version will support Create-Read-Update-Delete and transactions. | 11.1, 11.2
9. | MVDM Test Suite | Javascript | A series of tests focused on write-back support of the _MVDM Module_. Tests _VDM_ write-back as MVDM relies on VDM. | 35
10. | PIKS Generator | Python | Generates Patient, Institution, Knowledge and System (PIKS) annotations in _piks.jsonld_ for a _vdm.jsonld_ | 19
11. | Patient Security Prototype | Javascript (node.js) | An illustration of PIKS-enabled Patient level security. This involves an example client and an addition to FQS | 28
12. | FQS	| Javascript (node.js) | Fileman Query Service (FQS) based on embedded VDM model (REST service; read only) | 25
13. | Example Query Clients | Python, Javascript | Example command line clients that show how to use the FQS | 25
14. | FQS Web Client | Javascript, HTML | Browser based client for using the FQS | 33
15. | Metadata Cacher	| Javascript | queries (VISTA Application) metadata using VDM Package | 15
16. | Document Generators	| Various |	Generators of documentation leveraging common packages such as Sphinx and JSDoc and translators from Markdown to PDF and HTML | E4

Note that _VDM Package_ and _MVDM Module_ are the key software artifacts of the Project. Other software either helps in their development or configuration or illustrates their use.

#### Documents/Web Site

Per the PWS, all non PM documentation will be delivered on the Project Gits in the Markdown format.

\# | Name | Deliverable
:---: | :--- | --- | :---:
1. | Website | 13
2. | (Document) Approach to “Live VDM” Maintenance of Current State | 9
3. | [MVDM] Normalization Reports | 12
4. | Report on [MVDM] Exposure of older models | 14
5. | Prototype Patient-centric Data Security [Document] | 28 (Document)
6. | Briefing/Presentation on PIKS-based Security | 41

In addition, programmer documentation will be generated for _VDM Package_, _MVDM Module_ and _FQS_.

#### Model and Metadata Transformations

 Input | Software | Output
:--- | :--- | --- 
Fileman DD 			| DDJLD Maker 	|  dd.jsonld 
RPC models  				| RPCJLD Maker 	| rpc.jsonld
VPR RPC models 			| VPR Maker 		|  vpr.jsonld
dd.jsonld + rpc.jsonld   	| VDM Maker 		| vdm.jsonld  
vdm.jsonld + vpr.jsonld 	| MVDM Maker 		| mvdm.jsonld
vdm.jsonld 			| PIKS Generator 	| piks.jsonld
Markdown 			| Doc Generator	|  PDF, HTML



## Deliverables Schedule

In addition to the deliverables listed in the [Program Management Plan](https://github.com/vistadataproject/documents/blob/master/Submissions/VistAMetadata-PMP-2016-01-08.pdf) submitted to the government (Section 8.2), additional deliverables were identified for planning purposes. Such deliverables have been identified with a prefix of “E”. Deliverables 7, 10, and 11 were divided and designated .1 and .2 for VDM and MVDM, respectively.

Note: the following track breakdown is NOT in the PWS and its schedule. You can see the PWS schedule in [Updated PWS Deliverables](https://github.com/vistadataproject/documents/wiki/Updated-PWS-Deliverables).

### Track A: Infrastructure
Track  | PWS#  | Name |  Git | Content(s) | Format(s) | WBS |  PWS<br>Section
:---: | :---: | :---: | :---: | :---: | :--- | :---: | :---: 
A | 13 | Website  | [Website](https://github.com/vistadataproject/vistadataproject.github.io) | website, infographics to showcase the contents of the VDM and MVDM Subset | HTML, Javascript (d3.js) |  Q1 &#8594; Q4 | 5.3.2 
A | E3 | FileMan TEST VISTA ["nodeVISTA"]  | [nodeVISTA](https://github.com/vistadataproject/nodeVISTA) | a test VISTA ("nodeVISTA") that hosts different test datasets ("nodeVISTA Scenarios") | VISTA System, Vagrant | Q1 &#8594; Q4 | 
A | E4 |  Document Generators  | [documents](https://github.com/vistadataproject/documents) | Programmer documentation will be generated using tools such as Sphinx (http://sphinx-doc.org/) and JSDoc (http://usejsdoc.org/). Important Markdown-formatted documents need to be translated into PDF and HTML | Various  | Q1 &#8594; Q3 |  
&nbsp; ||||||

### Track B: VISTA Data Model (VDM)
Track  | PWS#  | Name |  Git | Content(s) | Format(s) | WBS |  PWS<br>Section
:---: | :---: | :---: | :---: | :---: | :--- | :---: | :---: 
B |  7.1 |  Machine Processable VISTA Data Model (VDM) "Read Only"  | [VDM](https://github.com/vistadataproject/VDM)  | _vdm.jsonld_, the native VISTA data model in JSON-LD based on one or more _dd.jsonld_'s.<br><br>_VDM Maker_, a program that creates _vdm.jsonld_ from _dd.jsonld_'s.<br><br>This version will support query/read ("VDM (read)"). | JSON-LD, Python, Javascript | Q1 | 5.3.1 
 B | 7.2 |  Machine Processable VISTA Data Model (VDM)  | [VDM](https://github.com/vistadataproject/VDM)  | _vdm.jsonld_, enhanced by write-data in _dd.jsonld_s and _rpc.jsonld_.<br><br>_VDM Maker_ must process more information from _dd.jsonld_'s and process _rpc.jsonld_. | JSON-LD, Python, Javascript | Q2 &#8594; Q4 |5.3.1
 B | 8 |   Date-stamped FileMan Data Model Implementations (Definitions) (cross refs, triggers ...)  | [VDM](https://github.com/vistadataproject/VDM) | _dd.jsonld_, a data dictionary captured in JSON-LD<br><br>_DDJLD Maker_, a program that caches and interprets the dictionaries from VISTAs in JSON-LD form. MUMPS code reduction will be needed for write-back support | JSON-LD, Python, Javascript | Q1 &#8594; Q2 | 5.3.1 
 B | E1 |  RPC Model  | [VDM](https://github.com/vistadataproject/VDM), [nodeVISTA](https://github.com/vistadataproject/nodeVISTA) | formal definition of the model implicit in "write-back RPCs", _rpc.jsonld_. Required for write support in _vdm.jsonld_ (#7.2/#E2.2). Created with _RPCJLD Maker_. It may encompass VISTA Options (file 101) too | JSON-LD, Python | Q1 &#8594; Q3 |  
 B | E2.1 |  VDM Package "Read-only"  | [VDM](https://github.com/vistadataproject/VDM) | a package that implements the VDM inside a VISTA. It will allow any FileMan data to be queried according to the VDM. | Javascript (node.js), MUMPS (KIDS) | Q1 | &nbsp; 
B  |   E2.2 | VDM Package | [VDM](https://github.com/vistadataproject/VDM), [nodeVISTA](https://github.com/vistadataproject/nodeVISTA) | Will add support for creating, updating and deleting (full CRUD) VISTA Data enabled by a write-back supporting VDM (#7.2). Initial write-back testing (in Q1) will be directly against nodeVISTA ("nodeVISTA Commands") | Javascript (node.js), MUMPS (KIDS)  | Q1 &#8594; Q4  |  &nbsp; 
B | 9 |  (Document) Approach to “Live VDM” Maintenance of Current State  | [VDM](https://github.com/vistadataproject/VDM) (Wiki) | In a wiki page, describe ways in which _dd.jsonld_ definitions and hence _vdm.jsonld_ could keep pace with changes in VISTAs | Markdown | Q4 | 5.3.1
B | 15 |  Date Stamped (Application) Meta Data for lab, surgery and other applications | [VDM](https://github.com/vistadataproject/VDM) | _Metadata Cacher_ that queries meta-data using _VDM package (Read)_ | Python, JSON-LD |  Q2 |  5.3.3
 B | 18 |  Machine-processable [PIKS] Annotations  | [VDM](https://github.com/vistadataproject/VDM) | Distinguish patient data from other types of VISTA data in a formal definition _piks.jsonld_. A VDM PIKS definition enables MVDM PIKS which in turn enables patient-centric security (#28) |  JSON-LD | Q2 |  5.3.4
B | 19 |  Software code [for PIKS] | [VDM](https://github.com/vistadataproject/VDM) | _PIKS Annotation Generator_. Relies on _VDM Package (Read)_ to create a _piks.jsonld_ | Python | Q2 | 5.3.4
B | 25 |  Prototype query access to VISTA Data against VDM ["FQS"] | [VDM](https://github.com/vistadataproject/VDM) | _Example Query clients_ that query (read-only) nodeVISTA using a REST-based FileMan Query Service (FQS) implemented over _VDM Package (Read)_ | Javascript, Python, JSON-LD |  Q2 | 5.4.1
B | 33 |  Prototype Web-Based Query Interface to FileMan [VDM] Data  | [VDM](https://github.com/vistadataproject/VDM) | _FQS Web Client_ for using _VDM Package (Read)_ | Javascript | Q2 &#8594; Q3 |  5.4.1
&nbsp; ||||||

### Track C: Master VISTA Data Model (MVDM)
Track  | PWS#  | Name |  Git | Content(s) | Format(s) | WBS |  PWS<br>Section
:---: | :---: | :---: | :---: | :---: | :--- | :---: | :---: 
C | 10.1  |  Master VISTA Data Model (MVDM) "Read-only"   | [VDM](https://github.com/vistadataproject/VDM) | _mvdm.jsonld_, a formal “MVDM Subset” definition with much of the scope of the VPR RPC which must be formally captured in _vpr.jsonld_. | JSON-LD | Q1 &#8594; Q2 | 5.3.2
C | 10.2 |  Master VISTA Data Model (MVDM)  | [VDM](https://github.com/vistadataproject/VDM) | full CRUD support rounded out for _mvdm.jsonld_. | JSON-LD | Q2 &#8594; Q4 |  5.3.2
C | 11.1  |  [MVDM over VDM] Heuristic (mapping) code "Read-only" [_MVDM Module_]  | [VDM](https://github.com/vistadataproject/VDM) | mapping tables and rules implemented in a _MVDM module_ that delivers a read-only version of MVDM over the VDM Package "Read-only". | Javascript (node.js), JSON | Q2 |  5.3.2
C | 11.2  |  [MVDM over VDM] Heuristic (mapping) code [_MVDM Module_]  | [VDM](https://github.com/vistadataproject/VDM) | full CRUD support added to _MVDM Module_ (Read). | Javascript (node.js), JSON | Q3 &#8594; Q4 |  5.3.2
C | 12  |  [MVDM] Normalization Reports | [VDM](https://github.com/vistadataproject/VDM) (Wiki) | Documents VDM to MVDM mapping as implemented in Deliverable #11. | Markdown | Q2 &#8594; Q4  | 5.3.2
C | 14  |  Report on [MVDM] Exposure of older models  | [VDM](https://github.com/vistadataproject/VDM) (Wiki) | Describe how older, cruder models could be handled in the MVDM | Markdown | Q4 | 5.3.2
C | 28 |  Prototype Patient-centric Data Security | [VDM](https://github.com/vistadataproject/VDM) | First document and then provide a self- contained prototype ("Patient Security Prototype") that shows how PIKS- enabled annotations enable patient-centric secure queries. The prototype will enhance FQS and have an example client | Javascript, Markdown | Q3 &#8594; Q4  | 5.4.1
C | 35 |  VISTA Application model(s)/Prototype(s) [Tests] | [VDM](https://github.com/vistadataproject/VDM) | MVDM write back tests (tier 1 through 3), enabled by mvdm.js configurations. Test scenarios for Deliverable #11. | Javascript, Python | Q2 &#8594; Q4  | 5.4.2
C | 36 |  Meta-model(s) [VPR] Prototype(s) | [VDM](https://github.com/vistadataproject/VDM) | Test code that shows how well the MVDM supports VPR (Read-only) convenience methods - read-only side of #35 | Javascript, Python | Q2 &#8594; Q3  | 5.4.2
C | 41 | Security Management Summary Report on PIKS Annotation | [documents](https://github.com/vistadataproject/documents) | Documents MVDM-security (authentication/access control/audit) for mixed audience | Markdown | Q4 | 5.4.2 
&nbsp; ||||||

### Track D: End to End Demonstration (Demo)
Track  | PWS#  | Name |  Git | Content(s) | Format(s) | WBS |  PWS<br>Section
:---: | :---: | :---: | :---: | :---: | :--- | :---: | :---: 
D | 23  |  End to end Demonstration of VDM and normalized MVDM redefined | [VDM demo](https://github.com/vistadataproject/VDM/tree/master/demo) | Create a self-contained web-based, graphical demo of VISTA Data Model (VDM) and normalized VDM (VDMN) securely reading and writing a representative subset of VISTA data in the Project's FileMan Test VISTA. This demo will showcase the project and must be installable and runnable on a Mac or Windows portable capable of running FileMan Test VISTA | CPRS and Web, nodeVISTA | Q3 &#8594; Q4 | 5.4.1


## PWS Deliverables Notes

  * Enumerated above are 27 technical deliverables within four tracks ( _VDM_, _MVDM_, _MVDMlink_, and _Infrastructure_).
  * Deliverables E1-4 are required but not explicitly enumerated in the PWS.
  * Deliverable #’s have gaps. The following PWS deliverables were retired as redundant or out of scope per government determination: 6, 16, 17, 20-24, 26, 27, 29-31, 34, 37, 38
  * In June 2016, a mod reintroduced Deliverable #23, added Deliverable #41 and removed deliverables #32, #39 and #40.
  * __There is a substantial difference in complexity between read-only and read-write models and implementations.__ To write anything demands knowledge of rules that go beyond the demands of reading. As a result, both VDM and MVDM models and packages will be delivered in two phases, with read coming first. 
    * __VDM "Read"__ and its package (#7.1 and #E1.1) are due in Q1; Deliverables #8, #15, #18, #19, #25, #33 only require such read-only functionality and are due in Q2
    * __MVDM "Read"__ and its module (#10.1 and #11.1) will be developed in parallel with MVDM write (#10.2, #11.2, #35). Unlike VDM, MVDM is explicitly specified and not automatically derived. 
    * Read-only VDM and by extension MVDM will expand on open source [FMQL](https://github.com/caregraf/FMQL)
    


### Program Management
Track  | PWS#  | Name |  Git | Content(s) | Format(s) | WBS |  PWS<br>Section
:---: | :---: | :---: | :---: | :---: | :--- | :---: | :---: 
PM | 1AA | Artifact Repository |  Project Gits |  ALL |  ALL  |  Q1 | 8.2
PM | 1A  | Non-disclosure/Non-Use Agreement   | &nbsp; | &nbsp; |  &nbsp; | Q1 |  6.1 
PM | 1B |  Quality Control Plan [QCP] |  documents |  an effective quality control program  | &nbsp;  | Q1 | 1.6.1 
PM | 1C  |  Phase-out Migration Plan |  documents | elaborates the artifacts to be transitioned on the Project Repository, and a schedule for transition completion  | &nbsp; | Q4 | 1.6.17 
PM | 2 |   Program Management Plan (PMP)	 | documents | strategy to accomplish the tasks and include the risk, quality and technical management approach, work breakdown structure (WBS), schedule management approach, schedule, cost requirements, and proposed staffing  | &nbsp; | Q1 |  5.2 
PM | 3|   Program Schedule and Monthly Updates  | documents | schedule, updated monthly | &nbsp; | Monthly | 5.2 
PM | 4 |  Monthly Progress Report  | &nbsp; | includes project status and financial management reporting | &nbsp; |  Monthly | 5.2 
PM | 5 |  Quarterly Strategic Communications Message  | documents | project progress and feasibility of transition to production | &nbsp; | Quarterly | 5.2
&nbsp; ||||||



# NOTES

#### Current VISTA Interfacing: MUMPS RPC-Based

- __Current external interfacing  to VISTA is through remote procedure calls (RPCs) written in the MUMPS language.__
- These are hard-coded for very specific clients and are not interchangeable to other clients due to the shared, embedded business logic within the custom MUMPS and client code.
- The RPCs may be "wrapped" with any number of client languages or technologies, which only complicates the maintenance of VISTA business logic as it is now embedded and obfuscated within within multiple different languages depending on the client.  
- This creates a polyglot "babelized"  VISTA, where parts of its logic is written in one language, and other parts of the same transactional logic is written in another, fragmenting and decentralizing the integrity of its business logic. 
- This makes the system extremely brittle and difficult to maintain because any changes to the system would require knowlege not just of MUMPS, but of the 'wrapping' language and technology as well, which changes over time.
- Security for all RPCs is based on the Terminal  (roll-and-scroll) interface and its Menu Actions. These Menus are hard-coded and exclusive to the terminal interface, and is not applicable to any generalized, external, web, or GUI-based interfaces.
- Many of the 3500 RPCs bypass the Fileman API and Data Dictionary, writing direct to MUMPS global storage. Bypassing the FM API means that Fileman security and auditing measures are bypassed, creating a significant security gap. 
- Bypassing the Fileman API also makes the data inaccessible to any other applications or by any other method other than by writing yet more custom MUMPS RPCs (The read and write RPCs are completely distinct from each other).  
- The only means to access or interface to new data is to write new MUMPS RPCs using the Terminal-based Actions-centric security, in addition to custom RPC MUMPS security code. 
