.PHONY: clean deps new

clean:
	mvn clean

deps:
	mvn dependency:copy-dependencies -DoutputDirectory=../inst/java

new:
	mvn archetype:generate -DgroupId=is.rud.phcss -DartifactId=phcss -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
