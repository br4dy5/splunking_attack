### Queries
## Enumerate the number of Techniques you can detect by each data source
```
index=attack type=attack-pattern 
| rename external_references{}.external_id as TID | rename x_mitre_data_sources{} as data_source
| eval technique = TID." ".name 
| stats dc(technique) as techniques values(technique) as technique by data_source
| sort - techniques
```
