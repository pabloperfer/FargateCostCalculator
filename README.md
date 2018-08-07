# FargateCostCalculator
Fargate Cost Calculator, A solution that for each stopped Fargate Task a json object will be generated in S3 with the billing report in a Json object.  

Currently Fargate does not allow cost allocation tagging, this prevents customers from tracking with detail their Fargate resources. Currently there's not any workaround for this more than a manual estimate.

I have created a solution that generates a billing report for each task that fargate executes.

It takes into account the task vCPUs, task Memory and the time spent from the pull action until the task is completely stopped.

A CloudWatch event that is triggered only whenever a Fargate Task is stopped, will invoke a Lambda function that will poll the task , and when the task is completely stopped it will fetch all the relevant information from it in order to make the necessary calculations .

Once the report is done, it will write a file with a json object in a S3 bucket. This will allow as to parse it later with Athena or any similar tool.

You can check the info in CloudWatch logs :

memory : 4096
cpu : 2048
Billing started after pull started at Aug 07 2018 15:49:47
Billing Stopped after task stopped at Aug 07 2018 15:59:39
TotalTime is 0:09:52.590000
Total Seconds : 592.59
CpuUnits : 2.0
Cpu Consumption is 0.0166636308$
MemUnits : 4.0
Memory Consumption is 0.008367370800000001$
total consumption is 0.025031001599999998$

Or parse the Json objects generated:

{"TaskId": "3a2a7366-4171-46b8-9098-056d68926490", "TaskCluster": "teststandalonetask", "Billing_Start_Time": "Aug 07 2018 15:49:47", "Billing_Stop_Time": "Aug 07 2018 15:59:39", "Cpu": 2048, "Memory": 4096, "TotalSeconds": 592.59, "CpuExpenses $": 0.0166636308, "MemExpenses $": 0.008367370800000001, "TotalExpenses $": 0.025031001599999998}



