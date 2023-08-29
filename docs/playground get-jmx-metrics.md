# playground get-jmx-metrics

🔢 Get JMX metrics from a component  
  
👉 Check documentation https://kafka-docker-playground.io/#/how-to-use?id=%f0%9f%94%a2-jmx-metrics

## Usage

```bash
playground get-jmx-metrics [OPTIONS]
```

## Examples

```bash
playground get-jmx-metrics --component connect
```

```bash
playground get-jmx-metrics --component connect --domain "kafka.connect kafka.consumer kafka.producer"
```

```bash
playground get-jmx-metrics -c broker
```

## Dependencies

#### *java*

visit $(blue_underlined https://openjdk.org/install/) to install

## Options

#### *--component, -c COMPONENT*

Component name

| Attributes      | &nbsp;
|-----------------|-------------
| Default Value:  | connect
| Allowed Values: | zookeeper, broker, connect, schema-registry

#### *--open, -o*

🔖 Save output to a file and open with text editor set with config.ini (default is code)

#### *--domain, -d DOMAIN*

Domain name


