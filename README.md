# automation-ontology-primitives

Industrial automation standards use the same words for different things. "Unit", "level", "zone", "state", "function", "procedure" and "alarm" each mean something different depending on which standard, and which discipline, you are standing in. People cope by knowing the context. Software and AI agents do not, and they swap meanings quietly.

This repo breaks the concepts of the ISA standards disciplines into a small, shared set of **primitives**, and maps every concept to the primitives it is made of. Two terms can then be compared by what they are made of instead of what they are called:

- the same signature: the terms can be linked
- a different signature: the difference shows exactly where a translation would go wrong

The goal is interoperability by design between standards, not a single merged model. Each standard keeps its own meaning; this repo says how the meanings relate.

## What is here

| Path | Contents |
|---|---|
| [`PRIMITIVES.md`](PRIMITIVES.md) | The primitive kinds, relations and qualifier axes, the signature notation and the mapping file format |
| `mappings/` | One YAML file per discipline cluster: every term, a fresh definition, its primitive signature and qualifiers |
| `notes/` | Per-cluster notes: concepts the primitives could not yet express, and words that collide across standards |

## Coverage

All 21 ISA standards committees, grouped into five clusters:

| Cluster | Committees |
|---|---|
| Procedural and workflow | ISA-88 (with PackML, ISA-TR88.00.02), ISA-106, ISA-113 |
| Enterprise and operations | ISA-95, ISA-108, ISA-111, ISA-112 |
| Instrumentation and field hardware | ISA-5, ISA-7, ISA-20, ISA-75, ISA-96, ISA-100, ISA-105 |
| Safety, alarms and operator interface | ISA-18, ISA-60, ISA-84, ISA-101 |
| Security and power | ISA-99 (ISA/IEC 62443), ISA-67, ISA-77 |

Adjacent models included where they meet ISA: the Allotrope Foundation Ontology (analytical lab) and the draft ISO 26060 (medicinal product manufacturing process and controls data).

## About the definitions

- Every definition here is written fresh. No text, figures or tables from ISA, IEC or ISO standards are reproduced. Each entry names the standard (and part, where known) so a reader can check it against their own licensed copy.
- Each entry carries a `confidence` rating. Entries are a first pass and need review by people who work with each standard. Corrections are the most useful contribution.
- This project is not affiliated with or endorsed by ISA, IEC, ISO or the Allotrope Foundation. Standard names are used only to identify the standards.

## Alignment

The primitives align to Basic Formal Ontology (BFO, ISO/IEC 21838-2) and the Information Artifact Ontology, so the mappings can be serialised to OWL and connected to ontologies that already use BFO, such as the Allotrope Foundation Ontology and the Industrial Ontologies Foundry core.

## Status

First draft, October 2026. Started in the context of the ISA-113 discussion on interoperability between ISA-88/ISA-113, ISO 26060 and Allotrope/Pistoia digital analytical methods.

## Licence

- Mapping data, definitions and documentation: [CC BY 4.0](LICENSE-DATA)
- Any code: [Apache 2.0](LICENSE)

## Contributing

Open an issue or a pull request. For a correction, say which entry, what is wrong, and which part of the standard supports the change (cite it; please do not paste the standard's text).
