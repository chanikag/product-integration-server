Running Samples
---------------

Sample Integration Flow configurations are available at samples/SequenceDiagramDSLSamples directory.

Configuration can be deployed to server by dropping the file to <CARBON_HOME>/deployment/integration-flows/ directory.


####Sample Configuration

```sh
@startuml

#Defining my integration flow
myIntegrationFlow : IntegrationFlow

//This is a sample inbound endpoint
participant sampleHTTPinbound : InboundEndpoint(protocol("http"),port("8290"),context("/sample/request"))

participant samplePipeline : Pipeline("message_flow_1")

participant sampleOutbound1 : OutboundEndpoint(protocol("http"),host("http://localhost:9000/services/SimpleStockQuoteService"))

participant sampleOutbound2 : OutboundEndpoint(protocol("http"),host("http://localhost:9001/services/SimpleStockQuoteService"))

sampleHTTPinbound -> samplePipeline : "client request"

samplePipeline::log("before filter statement")

alt with condition(source("$header.routeId"),pattern("r1"))
    samplePipeline::log("filter condition is true")
    samplePipeline -> sampleOutbound1 : "Validate policy with service 1"
    sampleOutbound1 -> samplePipeline : "Validate response from service 1"

else
    samplePipeline::log("filter condition is false")
    samplePipeline -> sampleOutbound2 : "Validate policy with service 2"
    sampleOutbound2 -> samplePipeline : "Validate response from service 2"
end

samplePipeline::log("after filter statement")

samplePipeline -> sampleHTTPinbound : "Final Response"


@enduml
```
