

# **THE OPEN FIBRE DATA STANDARD (OFDS) v0.3.0: COMPLETE REFERENCE AND IMPLEMENTATION GUIDE FOR EXPERTS**

## **Executive Summary: Strategic Overview and Compliance Mandate**

The Open Fibre Data Standard (OFDS) represents a critical initiative designed to establish a universal, open-source framework for accurately describing terrestrial fibre optic networks.1 Developed through collaboration among key international institutions including the World Bank, the International Telecommunications Union (ITU), Mozilla Corporation, and the Internet Society 15, OFDS functions simultaneously as an Open Data initiative and a mandatory Open Standard.2 Its overarching objective is to standardize data publication, thereby enhancing network resilience, increasing sector transparency, and enabling more effective, data-driven investment decisions in digital infrastructure.1

This report serves as the comprehensive technical documentation for the OFDS v0.3.0 release, currently the latest stable version.3 It delineates the normative content—the structure, format, and rules necessary for achieving technical compliance. Compliance is strictly defined by adherence to the core structure outlined in the JSON schema (network-schema.json), the prescribed codelists, and the technical publication formats.13 The documentation covers mandatory data entities (Network, Node, and Span), the required publication formats (JSON, GeoJSON, and CSV), and the use of associated validation tools, such as the Convert, Validate, Explore (CoVE) tool.5 Successful implementation of OFDS allows regulators and operators to accurately assess the true extent of existing infrastructure, identify connectivity gaps, and analyze areas of network fragility.1

---

## **Section 1: Foundational Principles, Scope, and Data Categories**

### **1.1 Strategic Rationale: Transparency, Resilience, and Investment**

The imperative for the Open Fibre Data Standard stems from a fundamental challenge in the telecommunications sector: a pervasive lack of readily available, usable, and consistent data concerning fibre optic network infrastructure.17 This fragmentation historically impedes informed decision-making across regulatory, governmental, and private investment spheres.1 OFDS was developed specifically to address this data gap by prescribing a common language and methodology for describing these essential assets.1

A core strategic benefit of adopting OFDS is the ability it confers upon governments and communication regulators to assess national network infrastructure holistically.1 Standardized data facilitates the sophisticated analysis required to identify not only gaps in coverage but also critical areas of network fragility or single points of failure.1 By providing this common framework, the standard aids in understanding overall network resilience and ensuring regulatory compliance.2

Furthermore, standardized fibre network data is recognized as an invaluable resource for driving infrastructure investment.1 Investors require reliable data to accurately assess market opportunities and guide financing decisions for broadband expansion efforts globally.6 The ability to integrate this standardized data seamlessly with Geographic Information System (GIS) tools and mapping platforms further strengthens its value proposition for planning and feasibility studies.1 The uniformity ensured by OFDS also means that software tools developed to support the standard in one country are easily transferable to others.1

### **1.2 The OFDS Core Definition: Open Standard and Data Principles**

OFDS provides a clear description of *what* data publishers should release regarding fibre optic networks, along with prescriptive instructions on *how* to structure and format that data for publication and use.1 It operates through a consistent set of common concepts and definitions that govern the content and structure of the network information.1

The standard is built upon established Open Data principles, ensuring that compliant data can be freely used, modified, and shared by anyone for any purpose.18

Table 1.2.1: Categories of Open Fibre Data 18

| Data Category | Definition and Scope | Example Attributes |
| :---- | :---- | :---- |
| **Geospatial Data** | Defines the physical location of the fibre network assets (Spans and Nodes). | Physical location (coordinates), route, horizontal accuracy.18 |
| **Technical Data** | Describes specific physical and operational characteristics of the fibre segments. | Number of fibres, fibre type, cable capacity, ITU fibre standards.18 |
| **Administrative Data** | Details the organizational, contractual, and operational entities and relationships. | Organizations that own or operate the infrastructure, status of infrastructure, dark fibre availability, ownership model.18 |

While advocating for broad data openness, the OFDS recognizes that infrastructure data can be sensitive. The standard’s framework allows regulators and operators to determine collectively the appropriate level of transparency for specific data sets, meaning the common language applies equally whether the data is made public or restricted to a limited internal group.2 This inherent flexibility ensures that the standard can be adopted under various national regulatory regimes that may require tiered levels of public disclosure.

### **1.3 Governance and Custodianship**

The Open Fibre Data Standard arose from a multi-stakeholder collaboration established in 2022\.1

* **Key Partners and Collaborators:** The development and promotion are driven by the World Bank, the International Telecommunications Union (ITU), the Internet Society, and Mozilla Corporation.15 Additional founding partners included Liquid Intelligent Technologies, CSquared, and Digital Council Africa.15  
* **Technical Development:** Financial and technical support was secured primarily through the World Bank.15 The technical development, including the creation of the schema, documentation, and tooling, was contracted to the **Open Data Services Cooperative (ODSC)**, a consultancy specializing in open data standard development.15  
* **Implementation Focus:** Since 2023, there has been a concentrated effort on practical adoption. Mozilla staff, supported by the broader consortium, have engaged in building technical capacity among operators and regulators in specific regions, including Ghana, Kenya, and India.15

### **1.4 Normative Hierarchy and Compliance Definition**

In order to be evaluated as compliant with OFDS, a data publisher must adhere strictly to the standard's **normative content**, which is defined as the prescriptive part of the standard from which no deviation is permitted.6 The structure and format of the data are authoritatively defined by three components 13:

1. **JSON Schema Files:** The foundational element is the JSON schema files, particularly network-schema.json, which defines the precise structure and validation rules for the OFDS data model.4  
2. **Codelist CSV Files:** These files provide the canonical reference for the meaning of codes used in various fields (e.g., ownership codes) to limit and standardize possible data values.13  
3. **Reference Documentation:** Technical reference documents covering the schema and publication formats contain the explicit rules that must be followed to publish OFDS data.13

All other documentation, such as the Primer, Guidance sections, examples, analogies, and illustrations, is considered **non-normative** or descriptive, providing context but not dictating compliance.6 The critical implication is that for any conflict between the published documentation text and the JSON schema itself, **the rules encoded within the schema take precedence**.4

### **1.5 Version History and v0.3.0 Changelog**

The Open Fibre Data Standard has undergone extensive development, progressing from Alpha and Beta releases.15 The current authoritative version is **v0.3.0**, released on June 29, 2023\.3

The v0.3.0 release focused heavily on eliminating technical ambiguity and ensuring adherence to external geospatial and data standards, preparing the standard for production-grade use.3

**Key Normative Updates in v0.3.0 (Technical Overview):**

| Component | Change Description | Compliance Impact |
| :---- | :---- | :---- |
| **JSON Schema** | Replacement of deprecated JSON Schema keywords (id with $id, definitions with $defs); replacement of generic publisher field with a structured object.3 | Ensures schema remains compatible with modern JSON Schema validation libraries. |
| **GeoJSON Format** | Disallowed additional properties in Geometry objects.3 | Enforces strict conformance with **RFC 7946** (GeoJSON standard), improving interoperability with professional GIS tools. |
| **CSV Format** | Mandated the use of **Well-Known Text (WKT)** to represent geometries (Node.location and Span.route).3 | Critical change to ensure spatial data accuracy is maintained when serializing to the non-spatial CSV format. |
| **Entity Naming** | Renamed generic links within the data structure to spans.3 | Improved conceptual clarity, specifically identifying the component as the physical fibre cable segments. |
| **Span Attributes** | Clarified the mandatory inclusion of the unit of measure for Span.fibreLength.3 | Enhanced data quality and precision for physical characteristics. |

---

## **Section 2: Normative Data Model Reference (Schema Deep Dive)**

The OFDS data model is built on the **Vector Data Model** concept fundamental to GIS 22, representing the fibre network as a series of connected points and lines. This model defines three primary, interconnected entities: Network (the dataset container), Nodes (point features), and Spans (line features). All data must conform to the structure defined in the canonical JSON schema file, which is linked directly from the network package itself.11

### **2.1 The Network Entity: Context and Georeferencing Integrity**

The Network object serves as the mandatory container for all metadata that defines the dataset's context, ownership, and, critically, its coordinate reference system (CRS) and accuracy.7

**Mandatory Georeferencing and Context Attributes:**

| Required Field | Data Type | Compliance Significance for Intermediates |
| :---- | :---- | :---- |
| **id** | String (UUID RFC 4122\) | Mandatory Universally Unique Identifier for the entire network dataset.7 Enables seamless aggregation across multiple publishers. |
| **collectionDate** | Date/String | The date the location data was collected or the source map digitized.7 Essential for assessing data timeliness and provenance. |
| **crs** | Object | Coordinate Reference System details. **Mandatory** if *any* coordinates exist (in Node.location or Span.route).7 |
| **crs/name** | String | The specific name of the CRS (e.g., WGS 84).7 |
| **crs/uri** | String (URI) | Canonical URI reference for the CRS, ensuring unambiguous definition (e.g., EPSG code URI).7 |
| **accuracy** | Number | Horizontal uncertainty, in meters, of the coordinates.7 A crucial metric for data quality assurance and fitness-for-use assessment. |
| **links** | Array | Must include a link to the canonical JSON schema (network-schema.json) that dictates the data's structure.11 |

The mandated inclusion of CRS details and horizontal uncertainty (accuracy) ensures that data quality and reliability are measurable and transparent. This rigorous metadata requirement is vital for regulatory applications and engineering planning that depend on spatial precision.

### **2.2 The Node Entity: Point Features and Connection Points**

Nodes represent discrete, physical geographical locations within the fibre network where Spans connect. These are critical connectivity markers, which can include Points of Presence (PoPs), Internet Exchange Points (IXPs), cell towers, or cable junction boxes.19

In the GeoJSON publication format, Nodes must be represented by **Point** geometries.9

**Table 2.2.1: Critical Node Attributes (Minimal Requirement)**

| Required Field | Data Type | Definition and Compliance Role |
| :---- | :---- | :---- |
| **id** | String | Unique identifier for the Node instance.7 Used to link Spans to their endpoints. |
| **location** | GeoJSON Point Object | Defines the geographic coordinates (longitude, latitude).7 This is the spatial anchor for the Node. |
| **type** | Codelist String | Prescribed classification (e.g., pop, ixp, tower).8 Limits ambiguity in asset identification. |
| **ownership** | Codelist String | Classification detailing the organizational or contractual model governing the asset.8 Must use codes from the normative Ownership Codelist. |

The Node.location object must strictly conform to the GeoJSON specification, ensuring coordinates are provided in WGS 84 (EPSG:4326) decimal degrees.9

### **2.3 The Span Entity: Line Features and Cable Characteristics**

Spans define the physical segments of fibre cable running between two Nodes. They are the backbone of the network and capture the technical and topological characteristics of the cable itself.

In the GeoJSON publication format, Spans must be represented by **LineString** geometries.9

**Table 2.3.1: Critical Span Attributes (Minimal Requirement)**

| Required Field | Data Type | Definition and Compliance Role |
| :---- | :---- | :---- |
| **id** | String | Unique identifier for the Span instance. |
| **route** | GeoJSON LineString Object | Defines the sequential coordinates tracing the physical path of the cable.9 |
| **links/start** | String | **Required ID** of the Node entity where the Span originates.3 |
| **links/end** | String | **Required ID** of the Node entity where the Span terminates.3 |
| **fibreLength** | Number | The physical length of the fibre segment (with required unit).3 Essential for engineering cost models and maintenance planning. |
| **fibreCount** | Integer | The total count of individual optical fibres contained within the cable segment. |
| **fibreTypeDetails/fibreSubtype** | Codelist String | Detailed technical classification of the specific fibre technology (e.g., Single-mode, Multi-mode).3 |

The explicit linkage of Spans to Nodes (links/start, links/end) is the foundation of the OFDS graph model. This rigorous topological structure allows analysts to trace physical pathways, assess connectivity redundancy, and identify potential single points of failure, directly supporting network resilience studies.1

### **2.4 Codelist Reference and Semantic Consistency**

Codelists are fundamental to compliance, ensuring that semantic consistency is maintained across all published datasets.13 They restrict the possible values for specific fields, which is vital for automated validation and cross-dataset analysis.

* **Ownership Codelist:** Standardizes the definition of control and management models for network assets.8  
  * public: Public sector owns and operates the network.8  
  * private: Private sector owns and operates the network.8  
  * ppp: Public Private Partnership model, characterized by shared risk and management responsibility.8  
* **Language Codelist:** Standardizes the representation of the default language, recommending adherence to BCP47 tags.7

Adherence to the codes defined in the Codelist CSV files is mandatory. Deviations result in immediate non-conformance.

---

## **Section 3: Technical Publication Formats and Interoperability**

The OFDS mandates publication in formats compatible with both Geospatial Information Systems (GIS) and conventional enterprise data processing. Publishers must use at least one of the three specified formats: JSON, GeoJSON, or CSV.25 All formats must support both small, API-driven responses and large-scale, streamable bulk downloads.11

### **3.1 GeoJSON Publication Format (Geospatial Integration)**

GeoJSON is the primary format for spatial representation and is essential for GIS integration.9 OFDS mandates a specific GeoJSON structure to maintain topological clarity.

#### **3.1.1 Feature Segregation and Geometry Types**

To comply with OFDS, GeoJSON data must be published in two separate, distinct Feature Collections 5:

1. **Nodes Feature Collection:** Must exclusively contain features with **Point** geometry.9  
2. **Spans Feature Collection:** Must exclusively contain features with **LineString** geometry.9

This separation reflects the fundamental GIS Vector Data Model, ensuring seamless integration with standard GIS layering concepts.22

#### **3.1.2 GeoJSON Conformance and Streaming**

* **RFC 7946 Compliance:** The GeoJSON implementation is highly strict, especially in v0.3.0, which explicitly disallows additional, non-standard properties in Geometry objects to enforce strict conformance with RFC 7946, a key requirement for professional GIS systems.3  
* **Bulk Data:** For large networks, GeoJSON files must be formatted for streaming as **Newline-delimited GeoJSON files (GeoJSON Lines)**.12 This allows data processing tools to handle large files sequentially without loading the entire dataset into memory.

### **3.2 CSV Publication Format (Database Portability)**

The CSV format supports flat data requirements, offering portability for use in conventional databases and spreadsheets.6 However, special handling is required to preserve spatial integrity.

#### **3.2.1 Relational Structure**

OFDS data in CSV format is normalized into a set of linked relational tables, typically: networks.csv, nodes.csv, and spans.csv.7 The unique IDs (id, nodes/0/id) are used as primary keys and foreign keys to maintain the necessary topological and contextual relationships across tables.7

#### **3.2.2 Mandatory Geometry Serialization (Well-Known Text \- WKT)**

Since CSV is non-spatial, a mechanism is required to embed the precise geometry of the network routes. The v0.3.0 release mandates the use of **Well-Known Text (WKT)** to represent geometries within the CSV fields.3

* **Node Geometry:** The Node.location coordinates must be serialized into a **WKT POINT** string (e.g., POINT (23.549 35.2908)).  
* **Span Geometry:** The Span.route coordinates must be serialized into a **WKT LINESTRING** string.

**Compliance Rationale:** WKT is the standard exchange format for vector geometry in most relational and spatial databases (e.g., PostGIS, Oracle Spatial). Mandating WKT ensures that spatial data published in CSV can be programmatically converted back into accurate GIS features (Points and LineStrings) without loss of spatial integrity, effectively bridging non-spatial and spatial environments.3

### **3.3 JSON Publication Format (API and Embedded Data)**

The JSON format is versatile, primarily used for embedding Network data in API responses or small, complete data packages.11

* **Small Files/API Responses:** Data must be contained within a JSON object that includes the mandatory .networks array, conforming to the network-package-schema.json.11  
* **Large Network Handling:** For networks too large for a single API response or memory load, the Network.links array must include canonical URLs pointing to:  
  * **Paginated Endpoints:** For API consumers to retrieve Nodes and Spans incrementally.11  
  * **Streamable Bulk Files:** For large-scale downloads, typically JSON Lines or Newline-delimited GeoJSON.11  
* **Streaming:** For bulk downloads, the data must be published as a **JSON Lines** file, where each line represents a complete Network object.12

**Table 3.3.1: OFDS Publication Formats and Streaming Options**

| Format | Small/API Response | Bulk/Streaming Method | Geometry Serialization | Standard Adherence |
| :---- | :---- | :---- | :---- | :---- |
| **JSON** | Network object with embedded .networks array | JSON Lines (Network object per line) 12 | Embedded GeoJSON (Optional) | network-package-schema.json 11 |
| **GeoJSON** | Separate Feature Collections (Nodes/Spans) 5 | Newline-Delimited GeoJSON (GeoJSON Lines) 12 | Nodes: Point; Spans: LineString 9 | RFC 7946 3 |
| **CSV** | Multiple linked relational tables (networks, nodes, spans) 7 | Standard CSV files 5 | **Mandatory Well-Known Text (WKT)** for geometry fields 3 | Relational linking via IDs 7 |

---

## **Section 4: Compliance and Validation Ecosystem**

Effective adoption of the Open Fibre Data Standard is inherently linked to stringent quality assurance and the official validation ecosystem. The primary tool provided by the custodians is the **Convert, Validate, Explore (CoVE)** tool.

### **4.1 The CoVE Tool: Conformance and Quality Assurance**

The CoVE tool (Convert, Validate, Explore) is the authoritative, open-source utility provided by the technical custodians (ODSC) to support publishers.5 Its use is highly recommended as conformance with its validation output represents the functional definition of compliance with the normative standard.2

#### **4.1.1 Validation Capabilities (Conformance Check)**

CoVE’s primary function is to perform a rigorous **conformance check**.5 This validation verifies that the submitted data strictly adheres to the technical rules defined in the OFDS JSON schema (network-schema.json) and the associated codelists.4

The conformance check includes, but is not limited to:

* Structural integrity (presence of all mandatory fields).  
* Data type validation (e.g., accuracy is a number, id is a valid UUID).  
* Codelist validation (values match canonical codelist entries, e.g., ownership codes).13  
* Topological validation (Spans correctly reference existing Node IDs via links/start and links/end).3

#### **4.1.2 Data Submission Protocols**

CoVE is designed for maximum accessibility, accepting data submission via direct upload or URL, and supporting multiple encodings.5

| Format | Accepted Submission/Encoding | GeoJSON-Specific Requirement |
| :---- | :---- | :---- |
| **JSON** | UTF-8 encoded JSON file (or pasted data) 5 | N/A |
| **GeoJSON** | UTF-8 encoded GeoJSON files 5 | Requires separate submission of **Nodes file** (Points) and **Spans file** (LineStrings).5 |
| **CSV** | UTF-8 encoded CSV files 5 | N/A |
| **Spreadsheet** | UTF-8, Windows-1252, or ISO-8859-1 encoded .xlsx or .ods files 5 | Sheets must conform to the relational CSV publication format.5 |

#### **4.1.3 Exploration and Quality Feedback**

Upon submission, the CoVE tool offers crucial post-processing features that aid quality assurance 5:

* **Visualization:** Immediate mapping of the data, allowing publishers to visually verify the geographical accuracy of the network routes and component placement.  
* **Quality Feedback:** Detailed feedback on non-conformance issues and potential data quality warnings.  
* **Format Conversion:** Ability to download the data in alternative supported formats, facilitating internal testing and integration.  
* **Sharing:** Submitted data is temporarily stored (90 days) at a randomly generated URL to facilitate transparent discussion and collaborative quality improvement among stakeholders.5

### **4.2 Data Collection and Quality Best Practices**

Data quality determines the utility of OFDS for regulatory and investment analysis. Intermediate experts must adhere to detailed quality practices:

* **Geospatial Provenance:** If data is derived from digitizing map images, publishers must accurately document the collectionDate and, critically, the horizontal accuracy (in meters) to establish the confidence level of the coordinates.7 This is required metadata, not simply an optional field.  
* **Topological Integrity:** Continuous and accurate asset tracking is essential.26 Publishers must ensure the consistent management of unique IDs (UUIDs) for Networks, Nodes, and Spans.7 The topological links defined by Span.links/start and Span.links/end must be rigorously maintained to ensure the network graph is sound for analysis.24  
* **Consistent Encoding:** All publication formats must use UTF-8 encoding (except where other encodings are explicitly allowed for spreadsheets) to prevent character corruption during data exchange.5

---

## **Section 5: Advanced Implementation and Network Analysis**

This section explores how the structured OFDS data model facilitates advanced network analysis, asset management, and complex system integration, targeting the intermediate-level expert.

### **5.1 Network Topology and Resilience Assessment**

The graph structure enforced by the OFDS data model is a prerequisite for formal topological analysis. The explicit relationship between Nodes (vertices) and Spans (edges) enables sophisticated network modeling.

**Conceptual Diagram 5.1.1: The OFDS Graph Model**

Code snippet

graph TD  
    subgraph Network  
        A \--\>|Span ID: CABLE-001| B  
        B \--\>|Span ID: CABLE-002| C  
        B \--\>|Span ID: CABLE-003| D  
        C \--\>|Span ID: CABLE-004| D  
    end  
    style Network fill:\#f0f8ff,stroke:\#003366,stroke-width:2px,stroke-dasharray: 5 5  
    A \-- Data: Node.location, Node.type, Node.ownership \--\> A\_D(Node Attributes)  
    A\_D \-- Span.links/start \--\> A  
    B \-- Data: Span.route, Span.fibreCount, Span.fibreLength \--\> B\_D(Span Attributes)  
    B\_D \-- Span.links/end \--\> B

#### **5.1.1 Vulnerability and Fragility Analysis**

The primary strategic benefit of OFDS is its ability to support the identification of **network fragility**.1 Analysts can leverage the standardized data to run algorithms that pinpoint vulnerabilities:

* **Single Points of Failure:** Identifying Nodes (e.g., PoPs) that, if disabled, would disconnect large segments of the network (high betweenness centrality).  
* **Redundancy Gaps:** Analyzing the ratio of multiple routes (paths) between critical nodes versus the overall network size.  
* **Shared Infrastructure:** Using the Span.route LineString geometry to identify where multiple different Spans (owned by different operators or serving different circuits) physically run together in a shared trench or conduit, creating a common failure risk from a single physical event (e.g., excavation cut).

Maintaining complete and accurate documentation of the network topology is vital for effective incident response and recovery, allowing network managers to rapidly identify and address vulnerabilities when an outage occurs.24

### **5.2 Integration with Asset Management Systems**

OFDS is not intended to replace existing enterprise Asset Management (AM) or Enterprise Resource Planning (ERP) systems but rather to serve as an open exchange standard for external publication and regulatory review.

#### **5.2.1 Asset Tracking and Data Flow**

The use of UUIDs for all assets (Networks, Nodes, Spans) is crucial for integrating OFDS data with internal asset tracking systems.7 The OFDS data can be viewed as the *publicly verifiable* snapshot of the physical network assets, managed internally by the operator's AM system.26

**Conceptual Diagram 5.2.1: Data Flow from Internal Systems to OFDS Publication**

| Internal System Layer | Function | OFDS Output Mapping |
| :---- | :---- | :---- |
| **ERP/Finance** | Ownership, contract data, acquisition date, maintenance records. | Network.ownership, Network.collectionDate, Node.ownership |
| **GIS/CAD (Engineering)** | Precise coordinates, trenching data, survey logs, cable splicing plans. | Node.location (Point), Span.route (LineString), Network.accuracy, Network.crs |
| **Fiber Inventory/EMS** | Fibre count, fibre type, active components (e.g., wavelength division multiplexing). | Span.fibreCount, Span.fibreTypeDetails/fibreSubtype, Span.capacityDetails (if published) |
| **Data Publishing Engine** | **Conformance Check (via CoVE)**, Format Serialization (WKT, GeoJSON). | **Final OFDS v0.3.0 Package** (JSON, GeoJSON, CSV) |

By ensuring that the OFDS id fields are cross-referenced with internal asset tags, organizations can maintain control and visibility over the asset landscape, mitigating risks associated with misplacement or loss of control.26

### **5.3 Advanced Geodetic Considerations**

For intermediate experts, understanding the technical implications of the mandatory georeferencing attributes is key to compliance and data quality.

#### **5.3.1 Datum and Coordinate Systems (CRS)**

While GeoJSON mandates the use of WGS 84 (decimal degrees) for the coordinates themselves 9, the Network object requires explicit documentation of the crs used for data collection.7

* **Projection Management:** When integrating OFDS data with local or national infrastructure maps, the Network.crs/uri and Network.crs/name allow systems to correctly re-project the data. For high-precision engineering work, local projections (e.g., specific UTM zones) are often used for input, which must be transformed to WGS 84 for OFDS publication. The metadata ensures the *original* context is preserved.7

#### **5.3.2 Horizontal Uncertainty (accuracy)**

The Network.accuracy field, defining horizontal uncertainty in meters, directly informs the consumer about the reliability of the spatial data.7

* **Compliance Interpretation:** A low accuracy value (e.g., \< 1 meter) suggests high-precision collection methods (e.g., high-grade GPS, post-processed survey data, or detailed CAD plans). A high accuracy value (e.g., \> 10 meters) indicates less precise methods (e.g., consumer GPS or digitization from older, undimensioned maps). This allows regulators to prioritize funding and planning decisions based on the most reliable infrastructure data.

---

## **Section 6: Standard Development, Future Outlook, and Support**

### **6.1 Standard Evolution and Community Engagement**

The OFDS operates as an open standard, meaning its development is iterative and driven by community feedback, use cases, and technical necessity.6

* **Feedback Mechanisms:** The custodians actively solicit feedback through established channels.3  
  * **General Feedback:** Existing discussions or new discussion threads on GitHub.11  
  * **Specific Issues/Bugs:** Issue tracker on GitHub for specific elements of the data model or documentation.11  
* **The Development Cycle:** Iterative improvements (which do not change normative content) are made outside the formal release cycle, while major normative changes are bundled into specific releases (like v0.3.0).3

### **6.2 Pilot Implementations and Capacity Building**

The development process has relied heavily on real-world use cases identified through pilot testing and regional workshops.6

* **Initial Pilots:** The standard was successfully piloted in **Kenya and Ghana** in 2023, including workshops with local network operators and communications regulators.15 Mozilla staff also engaged in capacity building in India.15  
* **Strategic Outcome:** The pilots serve to refine the schema based on practical challenges faced by operators and regulators in diverse markets, ensuring the standard is fit for global purpose.21 The World Bank has set aside funding for subsequent pilot implementations in East Africa.21

### **6.3 Accessing Support and Canonical Sources**

For compliance and implementation, it is essential to reference the official, canonical sources.19

**Table 6.3.1: Canonical Sources and Support**

| Resource | Purpose | Reference |
| :---- | :---- | :---- |
| **JSON Schema** | Authoritative definition of data structure and rules | The network-schema.json file (hosted on GitHub) 4 |
| **Reference Documentation** | Detailed rules for schema, codelists, and publication formats | The OFDS documentation site reference sections 19 |
| **Validation Tool** | Authoritative compliance check and quality assessment | The CoVE (Convert, Validate, Explore) web tool 5 |
| **Getting Help** | Access to support and community discussion | The OFDS Support Team (info@opentelecomdata.net) and the GitHub Community 17 |

---

## **Final Conclusions: OFDS as an Expert Tool**

The Open Fibre Data Standard (OFDS) v0.3.0 is a mature and technically rigorous standard. It moves beyond a simple framework by mandating precise geodetic metadata, requiring strict GeoJSON RFC 7946 compliance, and enforcing the use of Well-Known Text (WKT) for data integrity in database exchanges (CSV).3 For an intermediate-level expert, the standard’s value is realized in its capacity to enable automated validation via the CoVE tool and to support complex, graph-based topological analyses necessary for assessing network fragility and guiding national broadband investment.1 Adherence to the normative content, driven by the technical authority of the JSON schema, is the uncompromising requirement for full compliance.

#### **Works cited**

1. The Open Fibre Data Standard \- Internet Society, accessed November 17, 2025, [https://www.internetsociety.org/blog/2025/04/the-open-fibre-data-standard/](https://www.internetsociety.org/blog/2025/04/the-open-fibre-data-standard/)  
2. The Open Fibre Data Standard \- IETF Datatracker, accessed November 17, 2025, [https://datatracker.ietf.org/meeting/124/materials/slides-124-gaia-the-open-fibre-data-standard-shedding-light-on-fibre-networks-00](https://datatracker.ietf.org/meeting/124/materials/slides-124-gaia-the-open-fibre-data-standard-shedding-light-on-fibre-networks-00)  
3. Changelog — Open Fibre Data Standard 0.3.0 documentation, accessed November 17, 2025, [https://open-fibre-data-standard.readthedocs.io/en/latest/history/changelog.html](https://open-fibre-data-standard.readthedocs.io/en/latest/history/changelog.html)  
4. Schema reference — Open Fibre Data Standard 0.3.0 documentation, accessed November 17, 2025, [https://standard.ofds.info/en/latest/reference/schema.html](https://standard.ofds.info/en/latest/reference/schema.html)  
5. OFDS CoVE, accessed November 17, 2025, [https://ofds.cove.opendataservices.coop/](https://ofds.cove.opendataservices.coop/)  
6. Open Terrestrial Fiber Data Standard and Map ASA (P176146) \- World Bank Document, accessed November 17, 2025, [https://documents1.worldbank.org/curated/en/099063023160023332/pdf/P1761460fac12e0b09cb90f26880158a4f.pdf](https://documents1.worldbank.org/curated/en/099063023160023332/pdf/P1761460fac12e0b09cb90f26880158a4f.pdf)  
7. CSV — Open Fibre Data Standard 0.2.0 documentation, accessed November 17, 2025, [https://standard.ofds.info/en/0.2/reference/publication\_formats/csv.html](https://standard.ofds.info/en/0.2/reference/publication_formats/csv.html)  
8. Codelists reference — Open Fibre Data Standard 0.3.0 documentation, accessed November 17, 2025, [https://open-fibre-data-standard.readthedocs.io/en/latest/reference/codelists.html](https://open-fibre-data-standard.readthedocs.io/en/latest/reference/codelists.html)  
9. GeoJSON—ArcGIS Online Help | Documentation, accessed November 17, 2025, [https://doc.arcgis.com/en/arcgis-online/reference/geojson.htm](https://doc.arcgis.com/en/arcgis-online/reference/geojson.htm)  
10. About GeoJson Data \- Oracle Help Center, accessed November 17, 2025, [https://docs.oracle.com/en/database/other-databases/nosql-database/23.3/sqlreferencefornosql/geojson-data.html](https://docs.oracle.com/en/database/other-databases/nosql-database/23.3/sqlreferencefornosql/geojson-data.html)  
11. JSON — Open Fibre Data Standard 0.3.0 documentation, accessed November 17, 2025, [https://open-fibre-data-standard.readthedocs.io/en/latest/reference/publication\_formats/json.html](https://open-fibre-data-standard.readthedocs.io/en/latest/reference/publication_formats/json.html)  
12. Data modelling and schema development \- Standards Lab Handbook, accessed November 17, 2025, [https://os4d.opendataservices.coop/development/schema/](https://os4d.opendataservices.coop/development/schema/)  
13. Governance — Open Fibre Data Standard 0.3.0 documentation, accessed November 17, 2025, [https://open-fibre-data-standard.readthedocs.io/en/latest/governance/index.html](https://open-fibre-data-standard.readthedocs.io/en/latest/governance/index.html)  
14. Reference — Open Fibre Data Standard 0.3.0 documentation \- Read the Docs, accessed November 17, 2025, [https://open-fibre-data-standard.readthedocs.io/en/latest/reference/](https://open-fibre-data-standard.readthedocs.io/en/latest/reference/)  
15. Open Fibre Data Standard: Understanding the True Extent of the Internet \- The Mozilla Blog, accessed November 17, 2025, [https://blog.mozilla.org/netpolicy/2023/03/28/open-fibre-data-standard-understanding-the-true-extent-of-the-internet/](https://blog.mozilla.org/netpolicy/2023/03/28/open-fibre-data-standard-understanding-the-true-extent-of-the-internet/)  
16. Advancing Global Connectivity Through Open Fibre Data Standard \- Unified Magazine, accessed November 17, 2025, [https://unifiedmagazine.com/advancing-global-connectivity-through-open-fibre-data-standard/](https://unifiedmagazine.com/advancing-global-connectivity-through-open-fibre-data-standard/)  
17. Open Fibre Data Standard \- Read the Docs, accessed November 17, 2025, [http://open-fibre-data-standard.readthedocs.io/](http://open-fibre-data-standard.readthedocs.io/)  
18. Open Fibre Data Standard 0.3.0 documentation, accessed November 17, 2025, [https://open-fibre-data-standard.readthedocs.io/en/latest/primer/openfibredata.html](https://open-fibre-data-standard.readthedocs.io/en/latest/primer/openfibredata.html)  
19. Open Fibre Data Standard \- PIMF \- Internet Society Pulse, accessed November 17, 2025, [https://pulse.internetsociety.org/documents/11/Open\_Fibre\_Data\_Standard\_-\_PIMF.pdf](https://pulse.internetsociety.org/documents/11/Open_Fibre_Data_Standard_-_PIMF.pdf)  
20. Open Data Services, accessed November 17, 2025, [https://opendataservices.coop/](https://opendataservices.coop/)  
21. Mapping high speed internet networks in Sub Saharan Africa \- Open Data Services, accessed November 17, 2025, [https://opendataservices.coop/case-studies/mapping-high-speed-internet/](https://opendataservices.coop/case-studies/mapping-high-speed-internet/)  
22. Data model (GIS) \- Wikipedia, accessed November 17, 2025, [https://en.wikipedia.org/wiki/Data\_model\_(GIS)](https://en.wikipedia.org/wiki/Data_model_\(GIS\))  
23. GeoJSON \- Wikipedia, accessed November 17, 2025, [https://en.wikipedia.org/wiki/GeoJSON](https://en.wikipedia.org/wiki/GeoJSON)  
24. EPA Guidance on Improving Cybersecurity at Drinking Water and Wastewater Systems, Factsheet 2.P, accessed November 17, 2025, [https://www.epa.gov/system/files/documents/2024-07/maintain-updated-documentation.pdf](https://www.epa.gov/system/files/documents/2024-07/maintain-updated-documentation.pdf)  
25. Publication formats reference \- Open Fibre Data Standard \- Read the Docs, accessed November 17, 2025, [https://open-fibre-data-standard.readthedocs.io/en/latest/reference/publication\_formats/](https://open-fibre-data-standard.readthedocs.io/en/latest/reference/publication_formats/)  
26. What is Asset Tracking? Best Practices, Benefits, and Examples \- EZO, accessed November 17, 2025, [https://ezo.io/ezofficeinventory/blog/asset-tracking/](https://ezo.io/ezofficeinventory/blog/asset-tracking/)