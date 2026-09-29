# Bulgarian FEVER (BG-FEVER): Bulgarian Fact Verification Dataset

## Bulgarian TRAIN, DEV and Scientific Datasets

This repository contains three Bulgarian-language datasets developed for research on claim verification, evidence retrieval, and the identification of scientifically relevant claims using Bulgarian Wikipedia.

The collection consists of:

1. **TRAIN-bg** — the Bulgarian training dataset;
2. **DEV-bg** — the Bulgarian development dataset;
3. **Scientific-bg** — a scientifically oriented subset derived from the Bulgarian TRAIN and DEV data.

The datasets are accompanied by local collections of Bulgarian Wikipedia articles used as evidence sources.

## Project

The Bulgarian Fact Verification Dataset is developed within the LLMs4EU project, Digital Europe Programme, Grant Agreement No. 101198470.

## 1. Dataset Overview

The datasets are based on a FEVER-style claim verification framework. Each claim is associated with a verification label and, where applicable, Wikipedia evidence.

The three datasets contain a total of:

| Dataset | Claims | With evidence | Without evidence |
|---|---:|---:|---:|
| TRAIN-bg | 107,641 | 43,929 | 63,712 |
| DEV-bg | 7,910 | 4,538 | 3,372 |
| Scientific-bg | 7,047 | 1,541 | 5,506 |

The final collection therefore contains **122,598 claim records** across the three datasets.

The datasets are stored in JSONL format, with one claim per line.

## 2. TRAIN-bg

### Description

`train-bg-final.jsonl` is the Bulgarian training dataset.

Claims labelled `SUPPORTS` or `REFUTES` contain Wikipedia evidence, while `NOT ENOUGH INFO` claims do not have assigned evidence.

### Statistics

| Label | Number | Percentage |
|---|---:|---:|
| SUPPORTS | 30,726 | 28.55% |
| REFUTES | 13,203 | 12.27% |
| NOT ENOUGH INFO | 63,712 | 59.18% |
| **Total** | **107,641** | **100%** |

Evidence distribution:

- **43,929 claims** have Wikipedia evidence;
- **63,712 claims** do not have evidence.

The dataset contains **107,641 unique IDs**.

### Record structure

Each record contains five fields:

```json
{
  "id": 164554,
  "verifiable": "VERIFIABLE",
  "label": "SUPPORTS",
  "claim": "PageRank беше кръстена на американец.",
  "evidence": [[1, "PageRank", 1], [1, "Лари Пейдж", 0]]
}
```

The fields are:

- `id` — unique claim identifier;
- `verifiable` — whether the claim has associated evidence;
- `label` — `SUPPORTS`, `REFUTES`, or `NOT ENOUGH INFO`;
- `claim` — the Bulgarian claim text;
- `evidence` — Wikipedia evidence information, when available.

## 3. DEV-bg

### Description

`dev-bg-final.jsonl` is the Bulgarian development dataset.

It contains **7,910 claims** and follows the same general structure and label scheme as TRAIN-bg.

### Statistics

| Label | Number | Percentage |
|---|---:|---:|
| SUPPORTS | 2,368 | 29.94% |
| REFUTES | 2,170 | 27.43% |
| NOT ENOUGH INFO | 3,372 | 42.63% |
| **Total** | **7,910** | **100%** |

Evidence distribution:

- **4,538 claims** have Wikipedia evidence;
- **3,372 claims** do not have evidence.

The dataset contains **7,910 unique IDs**.

### Record structure

The records use the same five-field structure as TRAIN-bg:

```json
{
  "id": 59621,
  "verifiable": "NOT VERIFIABLE",
  "label": "NOT ENOUGH INFO",
  "claim": "„Аватар“ е отличен с награда „Оскар“ за своите новаторски визуални ефекти.",
  "evidence": []
}
```

## 4. Scientific-bg

### Description

`scientific_bg_final.jsonl` is a scientifically oriented subset developed from the Bulgarian TRAIN and DEV datasets.

The Scientific dataset was created through a multi-stage process.

First, Wikipedia pages associated with claims in the Bulgarian TRAIN and DEV datasets were identified and classified according to their thematic relevance to science, technology, engineering, and academic disciplines.

Scientific Wikipedia pages were then used to identify the claims associated with them.

In a subsequent stage, claims themselves were evaluated according to whether the **content of the claim** contains substantive scientific, technical, engineering, or academic information.

This distinction is important: a claim is not considered scientifically relevant merely because it refers to a scientist, university, scientific institution, technological company, or scientific award. The scientific relevance must be expressed in the content of the claim itself.

The resulting Scientific dataset contains **7,047 claims**.

### Statistics

| Label | Number | Percentage |
|---|---:|---:|
| SUPPORTS | 1,077 | 15.28% |
| REFUTES | 462 | 6.56% |
| NOT ENOUGH INFO | 5,508 | 78.16% |
| **Total** | **7,047** | **100%** |

Evidence distribution:

- **1,541 claims** have Wikipedia evidence;
- **5,506 claims** do not have evidence.

All **7,047 IDs are unique** within the Scientific dataset.

The Scientific dataset contains claims originating from both Bulgarian TRAIN and DEV:

- **6,406 records** correspond to TRAIN claims;
- **641 records** correspond to DEV claims.

## 5. Scientific Claim Selection

The scientific subset was developed in several stages.

### Stage 1: Wikipedia page classification

Wikipedia pages associated with the available evidence were classified according to whether they were scientifically relevant.

A total of **244 scientific Wikipedia pages** were identified during the initial scientific-page classification stage.

### Stage 2: Extraction of associated claims

Claims linked through the evidence information to scientifically classified Wikipedia pages were extracted from TRAIN-bg and DEV-bg.

The initial evidence-based scientific subset contained:

- **1,384 claims from TRAIN-bg**;
- **177 claims from DEV-bg**;
- **1,561 claims in total**.

### Stage 3: NOT ENOUGH INFO processing

The `NOT ENOUGH INFO` claims were processed separately because they do not contain assigned Wikipedia evidence.

Across TRAIN-bg and DEV-bg there were:

- **67,084 NOT ENOUGH INFO records**;
- **63,150 unique claims** after deduplication.

These unique claims were classified according to their scientific relevance.

The resulting classification contained:

- **6,009 SCIENCE_RELATED claims**;
- **57,141 NON_SCIENCE_RELATED claims**.

The final Scientific dataset combines the relevant claims identified through these processing stages.

## 6. Scientific Relevance Criterion

Scientific relevance is defined at the level of the claim itself.

A claim is considered scientifically relevant when it contains substantive content related to areas such as:

- physics;
- chemistry;
- biology;
- medicine;
- astronomy;
- geology;
- ecology;
- climate science;
- mathematics;
- statistics;
- computer science and informatics;
- engineering;
- technology;
- energy;
- technical systems and processes;
- scientific and technical research;
- linguistics and other academic disciplines when the claim itself contains substantive academic content.

The criterion also allows relevant claims from fields such as:

- history;
- archaeology;
- anthropology;
- demography;
- sociology;
- economics;
- political science;

when the claim itself contains substantive academic content from the respective field.

The criterion deliberately excludes claims whose only connection with science is the identity or subject of the entity mentioned.

For example, a biographical statement about a scientist is not automatically classified as scientifically relevant. In contrast, a statement describing a scientific theory, method, process, or finding can be classified as scientifically relevant.

## 7. Wikipedia Evidence

The datasets use Bulgarian Wikipedia as the main evidence source.

Evidence records identify the relevant Wikipedia page and evidence sentence. This information is used to connect claims with the corresponding local Wikipedia articles.

The final collection includes local Bulgarian Wikipedia article collections for:

- TRAIN;
- DEV;
- the Scientific subset.

The article files are stored as JSON files containing the article title and its sentence-level representation.

## 8. Local Wikipedia Article Collections

The final archive contains the following local article collections:

| Collection | Articles | Sentences |
|---|---:|---:|
| Bulgarian TRAIN | 6,185 | 629,200 |
| Bulgarian DEV | 1,545 | 106,606 |
| Scientific | 275 | 27,983 |

The Scientific article collection contains **275 locally prepared Wikipedia articles**.

The article collections preserve the local Train and Dev structure. Articles that occur in more than one source collection are not artificially removed; the original corpus structure is retained.

## 9. File Structure

The repository is organized approximately as follows:

```
Train, Dev, Scientific - fin/
│
├── Bulgarian/
│   │
│   ├── Train/
│   │   ├── train-bg-final.jsonl
│   │   └── articles/
│   │
│   └── Dev/
│       ├── dev-bg-final.jsonl
│       └── articles/
│
└── Scientific/
    ├── scientific_bg_final.jsonl
    └── articles/
```

## 10. JSONL Format

All three claim datasets use JSON Lines format.

Each line represents one claim and contains the following fields:

```
id
verifiable
label
claim
evidence
```

Example:

```json
{
  "id": 164554,
  "verifiable": "VERIFIABLE",
  "label": "SUPPORTS",
  "claim": "PageRank беше кръстена на американец.",
  "evidence": [
    [1, "PageRank", 1],
    [1, "Лари Пейдж", 0]
  ]
}
```

For claims without evidence:

```json
{
  "id": 59621,
  "verifiable": "NOT VERIFIABLE",
  "label": "NOT ENOUGH INFO",
  "claim": "„Аватар“ е отличен с награда „Оскар“ за своите новаторски визуални ефекти.",
  "evidence": []
}
```

## 11. Data Quality and Manual Verification

The datasets were processed using a combination of automated procedures, diagnostic analysis, and manual verification.

The TRAIN-bg and DEV-bg datasets were manually inspected at several stages to verify:

- claim structure;
- labels;
- evidence information;
- evidence-page associations;
- automatically generated classifications;
- selected scientific claims.

Additional manual verification was performed on the Scientific dataset. A final manual review covered approximately **half of the Scientific dataset**.

The purpose of these checks was to identify inconsistencies and improve the reliability of the automatically processed data before subsequent evidence-retrieval experiments.

## 12. Summary Statistics

| Property | TRAIN-bg | DEV-bg | Scientific-bg |
|---|---:|---:|---:|
| Claims | 107,641 | 7,910 | 7,047 |
| SUPPORTS | 30,726 | 2,368 | 1,077 |
| REFUTES | 13,203 | 2,170 | 462 |
| NOT ENOUGH INFO | 63,712 | 3,372 | 5,508 |
| With evidence | 43,929 | 4,538 | 1,541 |
| Without evidence | 63,712 | 3,372 | 5,506 |
| Unique IDs | 107,641 | 7,910 | 7,047 |

### Combined size

Across the three final JSONL datasets, **122,598 claim records** are included.

The collection provides a Bulgarian-language resource for studying the relationship between claims, Wikipedia evidence, and scientific relevance, with particular emphasis on sentence-level evidence retrieval from Bulgarian Wikipedia.

## License

These data annotations incorporate material from Wikipedia, which is licensed pursuant to the [Wikipedia Copyright Policy](https://en.wikipedia.org/wiki/Wikipedia:Copyrights). These annotations are made available under the license terms described on the applicable Wikipedia article pages, or, where Wikipedia license terms are unavailable, under the Creative Commons Attribution-ShareAlike License (version 3.0), available at [http://creativecommons.org/licenses/by-sa/3.0/](http://creativecommons.org/licenses/by-sa/3.0/).

## Cite BG-FEVER

```bibtex
@misc{bgfever2026-github,
  title        = {BG-FEVER: A Bulgarian Fact Verification Dataset},
  author       = {Tsvetana Dimitrova and Mihaela Moskova},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/DCL-IBL/BG-FEVER}},
  note         = {Developed within the LLMs4EU project, Digital Europe Programme, Grant Agreement No. 101198470}
}
```

## Original dataset

```bibtex
@inproceedings{thorne-etal-2018-fever,
  title     = "{FEVER}: a Large-scale Dataset for Fact Extraction and {VER}ification",
  author    = "Thorne, James and Vlachos, Andreas and Christodoulopoulos, Christos and Mittal, Arpit",
  editor    = "Walker, Marilyn and Ji, Heng and Stent, Amanda",
  booktitle = "Proceedings of the 2018 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long Papers)",
  month     = jun,
  year      = "2018",
  address   = "New Orleans, Louisiana",
  publisher = "Association for Computational Linguistics",
  pages     = "809--819",
  doi       = "10.18653/v1/N18-1074",
  url       = "https://aclanthology.org/N18-1074/"
}
```
