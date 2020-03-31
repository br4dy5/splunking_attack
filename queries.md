### Queries
## Enumerate the number of Techniques you can detect by each data source
```
index=attack type=attack-pattern 
| rename external_references{}.external_id as TID | rename x_mitre_data_sources{} as data_source
| eval technique = TID." ".name 
| stats dc(technique) as techniques values(technique) as technique by data_source
| sort - techniques
```

## Table out each Technique ID, CAPEC ID, Tactic, Technique, Data Source
```
index=attack type=attack-pattern 
| rename kill_chain_phases{}.phase_name as tactic 
| rename external_references{}.external_id as TID 
| rename x_mitre_data_sources{} as data_source 
| eval tech_id= mvindex(TID,0) 
| eval capec_id = mvindex(TID,1) 
| eval technique = tech_id." ".name 
| table tech_id capec_id type tactic technique data_source
```
