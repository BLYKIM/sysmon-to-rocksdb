Elasticsearch data to .csv file
===

First, you need to collect [SYSMON](https://learn.microsoft.com/ko-kr/sysinternals/downloads/sysmon) data with [WINLOGBEAT](https://www.elastic.co/kr/beats/winlogbeat) and stored with [ELASTICSEARCH](https://www.elastic.co/kr/elasticsearch)   
Second, this code will extract data to CSV files with delimiter "\t"

it's parsing "message" field with "agent.name", "agent.id" field

may require to modify maximum size of search query, default is 10000
```
// replace with your Index name
PUT /.ds-winlogbeat-8.8.2-2023.08.06-000001/_settings
{
    "max_result_window": 1000000
}
```

Please refer to the comments in the code for detailed explanation

## Quickstart
1. You need to create "elastic.rs" files, located "/src/envs"
- /src/envs/elasric.rs
```
pub const ES_URL_SECRET: &str = "YOUR ELASTICSEARCH URL";
pub const ID_SECRET: &str = "YOUR ELASTICSEARCH USERNAME (default is elaseic)";
pub const PW_SECRET: &str = "YOUR ELASTICSEARCH PASSWORD";
```
2. You need set your index name, the name may start with ".ds-winlogbeat" if you setup winlogbeat to elasticsearch automatically
and if index is multiple, set numbers and write index names within array
- /src/envs/env.rs
```
pub const INDICES: [&str; 1] = ["YOUR INDEX NAME"];

// if you have three indexes
// When the CSV is saved, if the file does not exist, a title line is added as the file is created, and if the file exists, the parsed data rows are added without the title line.
// To explain further, if you specify multiple indexes, the file will be created from the first index and the data will be added to the file created from the second index.
pub const INDICES: [&str; 3] = ["YOUR INDEX NAME 1", "YOUR INDEX NAME 2", "YOUR INDEX NAME 3"];
```
3. Set timestamp, query size, save location
- /src/envs/env.rs
```
pub const TIMESTAMP_START: &str = "START TIMESTAMP";
pub const TIMESTAMP_END: &str = "END TIMESTAMP";

pub const SIZE: usize = QUERY SIZE;

// between SAVELOCATION, CSVNAME event code will automatically generated
pub const SAVELOCATION: &str = "SAVE LOCATION";
pub const CSVNAME: &str = "FILENAME WITH FILE EXTENSTION (extenstion is .csv)";
```
4. Excute code
```
cargo build
cargo run --bin main
```

* Tip : Checking field types when selecting a wildcard type
```
// replace with your Index name
// When checking the message field type
GET /.ds-winlogbeat-8.8.2-2023.08.06-000001/_mapping/field/message
```
</br>

#### Copyright 2023. ClumL Inc. all rights reserved 