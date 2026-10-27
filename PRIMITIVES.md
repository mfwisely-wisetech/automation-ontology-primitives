# ISA primitives: the shared building blocks

Purpose: break every concept used across the ISA standards disciplines into a small set of primitives, so that two terms from different standards can be compared by what they are made of, not by their names. Two terms whose signatures match can be linked. Two terms whose signatures differ show exactly where a translation would go wrong.

Rules for everything in this folder
- Definitions are written fresh, in our own words. No ISA or IEC text is copied. Cite the standard (and part or clause when known) so a reader can check against their own copy.
- Every primitive is aligned to Basic Formal Ontology (BFO, ISO/IEC 21838-2) or the Information Artifact Ontology (IAO) where one exists, so the result can later be serialised as OWL.
- A signature uses only the primitives and relations below. If a concept cannot be expressed, it is recorded as a gap, not forced.

## 1. Kinds (16)

| Primitive | Meaning | Examples | BFO / IAO alignment |
|---|---|---|---|
| OBJECT | A physical thing that persists through time and keeps its identity | reactor, transmitter, valve, cable, controller hardware, building | BFO object |
| MATERIAL | A portion or amount of stuff, identified by what it is and how much | raw material, intermediate, product, sample, instrument air, fuel | BFO material entity (portion) |
| SITE | A place defined by location or boundary | plant area, room, hazardous area, sampling point, control room | BFO site / spatial region |
| PROCESS | Something that happens over time and has participants | reaction, batch run, measurement, transmission, maintenance task | BFO process |
| EVENT | An instantaneous happening; the start, end or change of something | alarm activation, operator command, state transition, trip | BFO process boundary |
| TIME | An instant or interval | timestamp, shift, batch window | BFO temporal region |
| QUALITY | A property that a thing, material or process has and that can be measured or observed | temperature, flow, pH, valve position, response time | BFO quality (and process profile) |
| STATE | A condition held over an interval, drawn from a defined set | Running, Held, In Alarm, Bypassed, Out of Service | (no BFO primitive; a quality or process profile over time) |
| ROLE | What something is in a particular context, which it can gain or lose without changing physically | sample, feed, critical parameter, operator, safety-related | BFO role |
| FUNCTION | What something is designed or intended to do | a valve regulates flow; a safety function brings the process to a safe state | BFO function |
| CAPABILITY | What something is able to do, or liable to do, under conditions (includes capacities and hazards) | equipment capability, valve capacity, flammability, potential for overpressure | BFO disposition |
| QUANTITY | A value with a unit | 4,000 mm, 6.8 pH, 250 ms, 3 bar(g) | (QUDT quantity value) |
| CATEGORY | A value from a defined classification or ordinal scale | SIL 2, alarm priority High, security level 3, ISA-95 Level 2, Zone 1 | (value specification) |
| INFORMATION | Content that is about something; independent of the medium carrying it | recipe, specification, P&ID, record, message, tag name | IAO information content entity |
| AGENT | A person, organisation, or system that initiates actions or can be held responsible | operator, engineer, vendor, regulator, an autonomous control system | (BFO object bearing an agent role) |
| COLLECTION | A group treated as one for a purpose, whose members keep their own identity | equipment list, alarm group, zone membership, fleet of devices | BFO object aggregate |

## 2. Relations (20)

| Relation | Reads as | Example |
|---|---|---|
| kind_of | A is a subtype of B | Unit kind_of OBJECT[equipment] |
| instance_of | a is one particular B | Reactor R-101 instance_of Unit |
| part_of | A is a part of B (has_part inverse) | Phase part_of Operation |
| member_of | A belongs to collection B | Alarm member_of Alarm group |
| connected_to | A is physically or logically connected to B (pipe, wire, network) | Transmitter connected_to Controller |
| located_in | thing A is in site B | Unit located_in Process cell |
| occurs_in | process or event A happens in site or time B | Batch run occurs_in Unit, during TIME |
| input_of / output_of | A goes into / comes out of process B | Raw material input_of Batch run |
| performed_by | process A is carried out by agent or object B | Phase run performed_by Equipment module |
| bears | A has quality, state, role, function or capability B | Valve bears FUNCTION[regulate flow] |
| realizes | process A carries out function, role, capability or plan B | Phase run realizes Equipment capability |
| prescribes | information A says how B should be or happen (plan, spec, design, requirement) | Recipe prescribes Batch run |
| records | information A says what B was or did (record, log, history) | Batch record records Batch run |
| about | information A refers to B without prescribing or recording it | Alarm message about QUALITY[level] |
| identifies | information A uniquely names B | Tag "LT-101" identifies Transmitter |
| derived_from | A was made or copied from B | Control recipe derived_from Master recipe; Sample derived_from Batch material |
| has_value | quality A has quantity or category B (at a time) | Level has_value 3,960 mm |
| constrains | information or quantity A sets a bound on B | Alarm limit constrains Level; Acceptance criterion constrains Assay |
| targets | information or quantity A sets the desired value of B | Setpoint targets Temperature |
| triggers | event or state A causes process, event or state B | High-high level triggers Trip |
| transitions_to | state A changes to state B (on event) | Running transitions_to Held on Hold |
| precedes | A comes before B in an order | Charge precedes Heat |
| mitigates | function or process A reduces capability (hazard) or risk B | SIF mitigates Overpressure |
| responsible_for | agent A is accountable for B | Operator responsible_for Alarm response |
| approves | agent A has authorised information B | Regulator approves Process description |

## 3. Qualifiers (5 axes)

Every mapped term also carries these, because they are where most mistranslations happen.

| Axis | Values | Why it matters |
|---|---|---|
| abstraction | type, class, particular | "Unit" (a type), "reactor units" (a class of equipment), "R-101" (a particular). ISA-88 recipe levels move along this axis. |
| modality | prescribed, actual, recorded | A design or recipe says what should be; the live plant is what is; a record says what was. The same quantity means different things in each. |
| viewpoint | design, operations, control, maintenance, safety, security, quality/lab, regulatory, enterprise | The discipline lens. The same word carries a different "flavour" in each. |
| lifecycle | specify, design, build, commission, operate, maintain, modify, decommission | When in the asset's life the concept applies. |
| scope | enterprise, site, area, cell, unit, equipment, device, signal | ISA-95 and ISA-88 hierarchies, 62443 zones and 18.2 alarm scope all sit on this axis. |

## 4. Signature notation

`KIND[note] & relation(KIND[note]) & relation(...)`

Example, Sample (lab viewpoint):
`MATERIAL & derived_from(MATERIAL[batch material]) & bears(ROLE[sample for testing]) & located_in(SITE[sampling point]) & occurs_in(TIME[drawn at])`
qualifiers: abstraction=particular, modality=actual, viewpoint=quality/lab

## 5. Mapping file format (YAML, one file per cluster)

```yaml
- id: ISA88.unit                 # standard + short slug, unique
  standard: ISA-88               # committee or standard
  part: "Part 1, physical model" # where in the standard, if known; else ""
  term: Unit
  definition: >-                 # fresh wording, max ~35 words, no copied text
    Equipment that carries out one or more major processing activities on one batch at a time.
  primary: OBJECT                # one kind from section 1
  signature: >-
    OBJECT[equipment] & part_of(OBJECT[process cell]) & has_part(OBJECT[equipment module]) &
    bears(CAPABILITY[major processing activity]) & constrains-self(one batch at a time)
  qualifiers: {abstraction: type, modality: actual, viewpoint: control, lifecycle: design, scope: unit}
  also_called: []                # synonyms in this or other standards
  same_word_elsewhere: [ISA95.equipment_hierarchy.unit]   # same word, different signature
  confidence: high               # how sure of the concept itself: high / medium / low
  gap: ""                        # anything the primitives could not express
```
