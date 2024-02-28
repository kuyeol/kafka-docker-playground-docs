# playground connector-plugin search-jar

☕ List jars for a connector plugin from confluent hub https://www.confluent.io/hub/ Search for specific class and display method signatures

## Usage

```bash
playground connector-plugin search-jar [OPTIONS]
```

## Dependencies

#### *javap*

visit https://openjdk.org/install/ to install

## Options

#### *--connector-plugin, -c CONNECTOR-PLUGIN*

🔌 Connector plugin name  
  
🎓 Tip: use \<tab\> completion to trigger fzf completion

| Attributes      | &nbsp;
|-----------------|-------------
| Required:       | ✓ Yes

#### *--class CLASS*

☕ Java class name to search for in all jars

## Examples

```bash
playground connector-plugin search-jar --connector-plugin confluentinc/kafka-connect-s3 --class WebIdentityTokenCredentialsProvider

```


