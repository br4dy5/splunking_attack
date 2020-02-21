### Queries
## Enumerate the number of Techniques you can detect by each data source
```
index=attack type=attack-pattern 
| rename external_references{}.external_id as TID 
| eval technique = TID." ".name 
| stats dc(technique) as techniques values(technique) by x_mitre_data_sources{} 
| sort - techniques
```
