# HBase Connectors - TDP

## Build

```
mvn clean install \
  -DskipTests \
  -Dmaven.javdoc.skip=true \
  -Dcheckstyle.skip=true \
  -Dfindbugs.skip=true \
  -Dspotbugs.skip=true \
  --batch-mode \
  -fae
```
## Build with specific dependencies

```
mvn clean install \
  -Dspark.version=2.3.5-TDP-0.1.0-SNAPSHOT \
  -Dscala.version=2.11.8 \
  -Dscala.binary.version=2.11 \
  -Dhadoop-three.version=3.1.1-TDP-0.1.0-SNAPSHOT \
  -Dhbase.version=2.1.10-TDP-0.1.0-SNAPSHOT \
  -DskipTests \
  -Dmaven.javdoc.skip=true \
  -Dcheckstyle.skip=true \
  -Dfindbugs.skip=true \
  -Dspotbugs.skip=true \
  --batch-mode \
  -fae
```

## Test

```
mvn clean test \
  --batch-mode \
  --fail-never
```
