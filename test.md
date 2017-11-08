# WebPortal
WebPortal is the entrance for users to see the cluster, view job status and operate jobs. You very easily use it to submit, delete and see details of jobs.
## Installation
You only need to config several parameters in clusterconfig.yaml, and then it will automatically install WebPortal.
* REST_SERVER_ADDR: String, the address of the rest server, for example, http://10.0.0.3:9273
* K8S_DASHBOARD_ADDR: String, the address of the kubernetes dashboard, for example, http://10.0.0.4:9090
* WEBPORTAL_PORT: Int, the port to use when launching WebPortal, for example, 6969
## Usage
### View cluster status
When you click the tab "Cluster Status", it will direct to the dashboard of kubernetes. Then you can see all components running on the cluster.
### View job status
When you click the tab "View Jobs", it will display the list of jobs. But you can only see some simple information in this page.
* If you want to see more, you can click the name of the job, then it will direct to a hadoop website. You can see the detail information here.
* If you want to delete a job, you can just click the "DELETE" button of the job. It will refresh the page after deleting successfully.
### Submit job
When you click the tab "Submit Job", you can see a button asking you to select a file to submit. We give an example and the requirements here.
(Remeber to delete the comments, or it will lead to an error.)
```json
{
    "jobName": "tf-example",                                        // the name of the job, must be unique.
    "image": "phillyonap.azurecr.io/hadoopk8su1604/cuda-hdfs-tf",   // the image to launch dockers
    
    "dataDir": "hdfs://10.0.3.9:9000/data/cifar10",                 // the path to the data 
    "trainDir": "hdfs://10.0.3.9:9000/output/train.model.1",        // the path to put training result
    "evalDir": "",                                                  // the path to put evaluating result
    "codeDir": "hdfs://10.0.3.9:9000/scripts/tf_cnn_benchmarks",    // the path of the codes
    
    "taskRoles": [                                                  // the list of taskroles
        {
            "name": "m1",                                           // the name the taskrole, must be different with the name of other taskroles in this job
            "taskNumber": 2,                                        // the number of the taskrole to launch, must be positive
            "cpuNumber": 1,                                         // the number of cpu to use
            "memoryMB": 1024,                                       // the number of memory to use 
            "gpuNumber": 0,                                         // the number of gpu to use
                                                                    // the command to run
            "command": "pip install scipy && python tf_cnn_benchmarks.py --local_parameter_device=cpu --num_gpus=4 --batch_size=16 --model=resnet20 --variable_update=distributed_replicated --data_dir=$AII_DATA_DIR --data_name=cifar10 --train_dir=$AII_TRAIN_DIR --ps_hosts=$AII_TASK_ROLE_0_HOST_LIST --worker_hosts=$AII_TASK_ROLE_1_HOST_LIST --job_name=ps --task_index=$AII_TASK_ROLE_INDEX"
        },
        {
            "name": "m2",
            "taskNumber": 2,
            "cpuNumber": 1,
            "memoryMB": 1024,
            "gpuNumber": 4,
            "command": "python tf_cnn_benchmarks.py --local_parameter_device=cpu --num_gpus=4 --batch_size=16 --model=resnet20 --variable_update=distributed_replicated --data_dir=$AII_DATA_DIR --data_name=cifar10 --train_dir=$AII_TRAIN_DIR --ps_hosts=$AII_TASK_ROLE_0_HOST_LIST --worker_hosts=$AII_TASK_ROLE_1_HOST_LIST --job_name=worker --task_index=$AII_TASK_ROLE_INDEX"
        }
    ],
    "killAllOnCompletedTaskNumber": 0,                              // the number of tasks to remain after completed
    "retryCount": 0                                                 // retry count if failed
}
```
