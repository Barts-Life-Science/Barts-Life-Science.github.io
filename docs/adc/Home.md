# The Analysis Data Core

This page contains the user documentation around the Analysis Data Core (ADC).

## Introduction

The ADC refers to the data warehouse environment where we transfrom the raw patient data collected from sources across the hospital into a format that is ready for further analysis. 
There are no user facing elements other than the data that will be provided into your project workspace within the Secure Data Environment (SDE), however our transformation pipeline is used to build the [Research Data Extract](https://github.com/Barts-Life-Science/Research-Data-Extract) and the resulting meta-data is also [documented](https://healthdatagateway.org/en/dataset/648) on the [HDR UK Gateway](https://healthdatagateway.org/en).

We maintain three baseliine data sets from which we generate project specific cohorts as requested and apply the [national data opt-out](https://digital.nhs.uk/services/national-data-opt-out) at this stage . These data sets are summarised below and the Research Data Extract can be browsed in overview through the [HDR Cohort Discovery tool](https://healthdatagateway.org/en/about/cohort-discovery).
| Dataset | Overview | Meta data | Synthetic data |
|---------|----------|-----------|----------------|
| Oracle Health Millennium | The raw data drawn dierctly from our EPR | [HDR UK Metadata](https://healthdatagateway.org/dataset/992) | [Download](https://bhdppublicassets.blob.core.windows.net/$web/millenium_synth.zip) |
| Ressearch Data Extract | Transformed EPR data prepared to be research ready | [HDR UK Metadata](https://healthdatagateway.org/en/dataset/648) | [Download](https://bhdppublicassets.blob.core.windows.net/$web/research_data_extract_synth.zip) |
| OMOP Encoded Reesearch Data Extract | The research data extract mapped into the OMOP CDM | [HDR UK Metadata](https://healthdatagateway.org/dataset/1098) | [Download](https://bhdppublicassets.blob.core.windows.net/$web/omop_synth.zip) |

Each synthetic data table is at most 10,000 rows and generated using a Gaussian Copula synthesizer for high statistic similarity between the source and synthetic data.
