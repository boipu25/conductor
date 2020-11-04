When using the Java client, the following metrics are published:

| Name        | Purpose           | Tags  |
| ------------- |:-------------| -----|
| task_execution_queue_full | Counter to record execution queue has saturated | taskType|
| task_poll_error | Client error when polling for a task queue | taskType, includeRetries, status |
| task_paused | Counter for number of times the task has been polled, when the worker has been paused | taskType |
| task_execute_error | Execution error | taskType|
| task_ack_failed | Task ack failed | taskType |
| task_ack_error | Task ack has encountered an exception | taskType |
| task_update_error | Task status cannot be updated back to server  | taskType |
| task_poll_counter | Incremented each time polling is done  | taskType |
| task_poll_time | Time to poll for a batch of tasks | taskType |
| task_execute_time | Time to execute a task  | taskType |
| task_result_size | Records output payload size of a task | taskType |
| workflow_input_size | Records input payload size of a workflow | workflowType, workflowVersion |
| external_payload_used | Incremented each time external payload storage is used | name, operation, payloadType | 

Metrics on client side supplements the one collected from server in identifying the network as well as client side issues.

[1]: https://github.com/Netflix/spectator

For client side configuration you need to register your metrics registry to the global COMPOSITE registry.

Sample code for prometheus micrometer registry to register these metrices :-

1. Add these to your POM file :-
        <dependency>
            <groupId>com.netflix.spectator</groupId>
            <artifactId>spectator-reg-metrics3</artifactId>
            <version>${spectator.version}</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.spectator</groupId>
            <artifactId>spectator-reg-micrometer</artifactId>
            <version>${spectator.version}</version>
        </dependency>
   Note - Assuming prometheus micrometer POM is already present.
   
   2. Add this to a configuration class :-
   
        MicrometerRegistry micrometerRegistry = new MicrometerRegistry(meterRegistry);
        Spectator.globalRegistry().add(micrometerRegistry);
        
        
        
