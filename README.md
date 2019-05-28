# sysmon-winlogbeat-config

Elasticsearch config examples (template and ingest/pipeline) for Sysmon + Winlogbeat.


### Elasticsearch template for Sysmon + Winlogbeat

* Mapping for Sysmon 9.01 + Winlogbeat 6.6.0 (sysmon-9.01_winlogbeat-6.6.0/template.json)

| Field | Updated datatype | Default datatype |
| ---------- | ------- | ------- |
| host.name | text | keyword |
| host.os.name | text | keyword |
| event_data.UtcTime | date | keyword |
| event_data.CreationUtcTime | date | keyword |
| event_data.PreviousCreationUtcTime | date | keyword |
| event_data.Image | text | keyword |
| event_data.Company | text | keyword |
| event_data.Product | text | keyword |
| event_data.Description | text | keyword |
| event_data.CommandLine | text | keyword |
| event_data.User | text | keyword |
| event_data.ParentImage | text | keyword |
| event_data.ParentCommandLine | text | keyword |
| event_data.Protocol | text | keyword |
| event_data.SourceHostname | text | keyword |
| event_data.DestinationHostname | text | keyword |
| event_data.SourceIp | ip | keyword |
| event_data.DestinationIp | ip | keyword |
| event_data.SourcePort | integer | keyword |
| event_data.DestinationPort | integer | keyword |
| event_data.SourcePortName | text | keyword |
| event_data.DestinationPortName | text | keyword |
| event_data.TargetFilename | text | keyword |
| event_data.Details | text | keyword |
| event_data.SourceImage | text | keyword |
| event_data.TargetImage | text | keyword |
| event_data.ImageLoaded | text | keyword |
| event_data.Signature | text | keyword |
| event_data.StartModule | text | keyword |
| event_data.StartFunction | text | keyword |
| event_data.Device | text | keyword |
| event_data.CallTrace | text | keyword |
| event_data.PipeName | text | keyword |
| event_data.TargetObject | text | keyword |
| event_data.EventType | text | keyword |
| event_data.Operation | text | keyword |
| event_data.Type | text | keyword |
| event_data.NewName | text | keyword |
| event_data.Name | text | keyword |
| event_data.Query | text | keyword |
| event_data.Destination | text | keyword |
| event_data.Consumer | text | keyword |
| event_data.Filter | text | keyword |
| event_data.Operation | text | keyword |
| task | text | keyword |
| user.identifier | text | keyword |
| user.name | text | keyword |
| user.domain | text | keyword |

#### Usage example

```text: winlogbeat-6.6.0
cd sysmon-9.01_winlogbeat-6.6.0/
curl -XPUT http://<elasticsearch-address>:9200/_template/winlogbeat-6.6.0 -H 'Content-Type: application/json' -d @template.json
```



### Elasticsearch pipeline (ingest node) for Sysmon + Winlogbeat

* Processors for Sysmon 9.01 + Winlogbeat 6.6.0  (sysmon-9.01_winlogbeat-6.6.0/pipeline.json)

| Field | processor | description |
| ---------- | ------- | ------- |
| event_data.UtcTime | Date processor | Parse the timestamp value and update @timestamp field. |
| event_data.Hashes | Split processor | Split multiple hash values by algorithm. |
| event_data.Hash | Split processor | Split multiple hash values by algorithm. |
| event_data.CreationUtcTime | Date processor | Parse the timestamp value and update this field in ISO8601 format. |
| event_data.PreviousCreationUtcTime | Date processor | Parse the timestamp value and update this field in ISO8601 format. |
| event_data.SourcePort | Convert processor | Convert the socket port-number value to integer. |
| event_data.DestinationPort | Convert processor | Convert the socket port-number value to integer. |
| message | Remove processor | Remove the original log message. |

#### Usage example

```text: winlogbeat-6.6.0
cd sysmon-9.01_winlogbeat-6.6.0/
curl -XPUT http://<elasticsearch-address>:9200/_ingest/pipeline/my-sysmon-9.01-winlogbeat-6.6.0 -H 'Content-Type: application/json' -d @pipeline.json
```



### Winlogbeat config for Sysmon

* **winlogbeat.yml** for Sysmon 9.01 + Winlogbeat 6.6.0  (sysmon-9.01_winlogbeat-6.6.0/winlogbeat-6.6.0/winlogbeat.yml)

See fields added to the original winlogbeat.yml (winlogbeat.yml.org).

```text: winlogbeat-6.6.0
cd sysmon-9.01_winlogbeat-6.6.0/winlogbeat-6.6.0/
diff winlogbeat.yml.org winlogbeat.yml
24a25
>   - name: "Microsoft-Windows-Sysmon/Operational"
98c99,100
<   hosts: ["localhost:9200"]
---
>   hosts: ["<elasticsearch-address>:9200"]
>   pipeline: my-sysmon-9.01-winlogbeat-6.6.0

```



