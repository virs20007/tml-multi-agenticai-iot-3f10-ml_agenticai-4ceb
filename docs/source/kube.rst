Scaling [tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb] With Kubernetes
===========================

Generated On: 2026-03-30 21:38:21 UTC

You can scale your solution with Kubernetes.  To do so, will will need to apply the following YAML files to your Kubernetes cluster.

.. tip::
   Refer to TML documentation for more information on `scaling with Kubernetes <https://tml.readthedocs.io/en/latest/kube.html>`_.

   Watch the YouTube Video: `here <https://www.youtube.com/watch?v=MEbmTXIQpVo>`_.

   You can also run the YAML files locally in a 1 node kubernetes cluster called minikube.  Refer to `Installing minikube <https://tml.readthedocs.io/en/latest/kube.html#installing-minikube>`_

.. important:: 
   Below assumes you have a Kubernetes cluster and **kubectl** installed in your Linux environment.

.. attention::

   Make sure to STOP the TSS Container and other containers before running Kubernetes/Minikube.

   If you get the following WARNING from Kubernetes:

    Warning  FailedScheduling  default-scheduler  0/1 nodes are available: 1 Insufficient nvidia.com/gpu. preemption: 0/1 nodes are available: 1 No preemption victims found for 
    incoming pod.  **Make sure no other application is using the GPU.**  You can check by executing in terminal the command: **nvidia-smi**

    Oherwise, Issue the commands below:

.. code-block::

   sudo apt update && sudo apt install -y nvidia-docker2

   sudo nvidia-ctk runtime configure --runtime=docker 

   sudo systemctl restart docker


Based on your TML solution [tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb] - if you want to scale your application with Kubernetes - you will need to apply the following YAML files.

.. list-table::

   * - **YML File**
     - **Description**
   * - :ref:`tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml`
     - This is your main solution YAML file.  
 
       It MUST be applied to your Kubernetes cluster.
   * - :ref:`secrets.yml`
     - You MUST store your passwords in base64 format 

       in this file.  For instructions on how to convert

       plain text passwords to base64 refer to `instructions <https://tml.readthedocs.io/en/latest/kube.html#how-to-store-secure-passwords-in-kubernetes>`_
   * - :ref:`mysql-storage.yml`
     - This is storage allocation for MySQL DB.
 
       It MUST be applied to your Kubernetes cluster.
   * - :ref:`mysql-db-deployment.yml`
     - This is the MySQL deployment YAML.
 
       It MUST be applied to your Kubernetes cluster.
   * - :ref:`kafka.yml`
     - This is the Kafka deployment YAML.
 
       This is MANDATORY if using kafka locally or on-premise.
   * - :ref:`privategpt.yml`
     - This is the privateGPT deployment YAML.
 
       This is OPTIONAL.  However, it must be 
 
       applied if using Step 9 DAG.
   * - :ref:`qdrant.yml`
     - This is the Qdrant deployment YAML.
 
       This is OPTIONAL.  However, it must be 
 
       applied if using Step 9 DAG.
   * - :ref:`nginx-ingress-tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml`
     - If you are scaling your TML solution you must

       apply the nginx-ingresstml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml; this yaml is 

       auto-generated for every TML solution.

       For more details see section 

       :ref:`Scaling with NGINX Ingress and Ingress Controller`

kubectl Apply command
-----------------

.. important::
   To apply the YAML files below to your Kubernetes cluster simply run this command:

.. code-block:: YAML

   kubectl apply -f kafka.yml -f secrets.yml -f mysql-storage.yml -f mysql-db-deployment.yml -f tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml -f ollama.yml

tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml
------------------------

.. important::
   Copy and Paste this YAML file: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml - and save it locally.

   These YML files can also be found on GitHub at: https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/ymls/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb

.. attention::

   MAKE SURE to update any tokens and passwords in the **secrets.yml** file:

          1. GITPASSWORD (MANDATORY)
             
          2. READTHEDOCS (MANDATORY)
             
          3. KAFKACLOUDPASSWORD (OPTIONAL)
             
          4. MQTTPASSWORD (OPTIONAL)

   For instructions on how to do this, refer to `instructions <https://tml.readthedocs.io/en/latest/kube.html#how-to-store-secure-passwords-in-kubernetes>`_

.. code-block:: YAML

   ################# tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml
   
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb
     spec:
       selector:
         matchLabels:
           app: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb
       replicas: 3 # tells deployment to run 1 pods matching the template
       template:
         metadata:
           labels:
             app: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb
         spec:
           containers:
           - name: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb
             image: virs20007/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-amd64:latest
             volumeMounts:
             - name: dockerpath
               mountPath: /var/run/docker.sock
             - name: rawdata
               mountPath: /rawdata   # container folder where the local folder will be mounted                           
             ports:
             - containerPort: 5050
             - containerPort: 4040
             - containerPort: 6060
             env:
             - name: TSS
               value: '0'
             - name: PROJECTNAME
               value: 'tml-multi-agenticai-iot-3f10'                              
             - name: SOLUTIONNAME
               value: 'tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb'
             - name: SOLUTIONDAG
               value: 'solution_preprocessing_ml_agenticai_dag-tml-multi-agenticai-iot-3f10'
             - name: GITUSERNAME
               value: 'virs20007'
             - name: GITREPOURL
               value: 'https://github.com/virs20007/rasptssV2.git'
             - name: SOLUTIONEXTERNALPORT
               value: '5050'
             - name: CHIP
               value: 'amd64'
             - name: SOLUTIONAIRFLOWPORT
               value: '4040'
             - name: SOLUTIONVIPERVIZPORT
               value: '6060'
             - name: DOCKERUSERNAME
               value: 'virs20007'
             - name: CLIENTPORT
               value: '0'
             - name: EXTERNALPORT
               value: '42275'
             - name: KAFKACLOUDUSERNAME
               value: ''
             - name: VIPERVIZPORT
               value: '9005'
             - name: MQTTUSERNAME
               value: ''
             - name: AIRFLOWPORT
               value: '9000'
             - name: GITPASSWORD
               valueFrom:
                 secretKeyRef:
                  name: tmlsecrets 
                  key: githubtoken                       
             - name: KAFKACLOUDPASSWORD
               valueFrom:
                 secretKeyRef:
                  name: tmlsecrets 
                  key: kafkacloudpassword                      
             - name: MQTTPASSWORD
               valueFrom: 
                 secretKeyRef:
                   name: tmlsecrets 
                   key: mqttpass                        
             - name: READTHEDOCS
               valueFrom:
                 secretKeyRef:
                   name: tmlsecrets 
                   key: readthedocs          
             - name: qip 
               value: 'privategpt-service' # This is private GPT service in kubernetes
             - name: KUBE
               value: '1'
             - name: step3localfileinputfile # STEP 3 localfile inputfile field can be adjusted here.
               value: '/rawdatademo/IoTData.txt'
             - name: step3localfiledocfolder # STEP 3 # STEP 3 docfolder inputfile field can be adjusted here.
               value: ''               
             - name: step4maxrows # STEP 4 maxrows field can be adjusted here.  Higher the number more data to process, BUT more memory needed.
               value: '-1'
             - name: step4bmaxrows # STEP 4b maxrows field can be adjusted here.  Higher the number more data to process, BUT more memory needed.
               value: '-1'               
             - name: step4cmaxrows # STEP 4c maxrows field can be adjusted here.  Higher the number more data to process, BUT more memory needed.
               value: '-1'               
             - name: step4crawdatatopic # STEP 4c
               value: ''               
             - name: step4csearchterms # STEP 4c 
               value: ''               
             - name: step4crememberpastwindows # STEP 4c 
               value: ''               
             - name: step4cpatternwindowthreshold # STEP 4c 
               value: ''               
             - name: step4crtmsscorethreshold # STEP 4c 
               value: '' 
             - name: step4cattackscorethreshold # STEP 4c 
               value: '' 
             - name: step4cpatternscorethreshold # STEP 4c 
               value: ''                
             - name: step4crtmsstream # STEP 4c 
               value: ''                              
             - name: step4clocalsearchtermfolder # STEP 4c 
               value: ''                              
             - name: step4clocalsearchtermfolderinterval # STEP 4c 
               value: ''                                                            
             - name: step4crtmsfoldername # STEP 4c 
               value: ''                                                                           
             - name: step4crtmsmaxwindows # STEP 4c adjust RTMSMAXWINDOWS for Step 4c
               value: ''                                       
             - name: step2raw_data_topic # STEP 2 
               value: 'iot-raw-data,agent-responses,team-lead-responses,supervisor-responses,all-agents-responses'                           
             - name: step2preprocess_data_topic # STEP 2 
               value: 'iot-preprocess,iot-preprocess2'                           
             - name: step4raw_data_topic # STEP 4
               value: 'iot-raw-data'                           
             - name: step4preprocess_data_topic # STEP 4
               value: 'iot-preprocess'                                                         
             - name: step4preprocesstypes # STEP 4
               value: 'anomprob,trend,avg'                                                         
             - name: step4jsoncriteria # STEP 4
               value: 'uid=metadata.dsn,filter:allrecords~subtopics=metadata.property_name~values=datapoint.value~identifiers=metadata.display_name~datetime=datapoint.updated_at~msgid=datapoint.id~latlong=lat:long'                           
             - name: step4ajsoncriteria # STEP 4a 
               value: ''                           
             - name: step4amaxrows # STEP 4a
               value: ''                           
             - name: step4apreprocesstypes # STEP 4a
               value: ''                           
             - name: step4araw_data_topic # STEP 4a
               value: ''                           
             - name: step4apreprocess_data_topic # STEP 4a
               value: ''                           
             - name: step4bpreprocesstypes # STEP 4b
               value: ''                           
             - name: step4bjsoncriteria # STEP 4b
               value: ''                           
             - name: step4braw_data_topic # STEP 4b 
               value: ''                           
             - name: step4bpreprocess_data_topic # STEP 4b 
               value: ''                                          
             - name: step5rollbackoffsets # STEP 5 rollbackoffsets field can be adjusted here.  Higher the number more training data to process, BUT more memory needed.
               value: '800'                              
             - name: step5processlogic # STEP 5 processlogic field can be adjusted here.  
               value: 'classification_name=failure_prob:Power_preprocessed_AnomProb=55,n'                                                
             - name: step5independentvariables # STEP 5 independent variables can be adjusted here.  
               value: 'Power_preprocessed_AnomProb'                                                                              
             - name: step6maxrows # STEP 6 maxrows field can be adjusted here.  Higher the number more predictions to make, BUT more memory needed.
               value: '50'                              
             - name: step9rollbackoffset # STEP 9 rollbackoffset field can be adjusted here.  Higher the number more information sent to privateGPT, BUT more memory needed.
               value: '-1'                  
             - name: step9prompt # STEP 9 Enter PGPT prompt
               value: ''                  
             - name: step9context # STEP 9 Enter PGPT context
               value: ''                                 
             - name: step9keyattribute
               value: '' # Step 9 key attribtes change as needed  
             - name: step9keyprocesstype
               value: '' # Step 9 key processtypes change as needed                                               
             - name: step9hyperbatch
               value: '' # Set to 1 if you want to batch all of the hyperpredictions and sent to chatgpt, set to 0, if you want to send it one by one   
             - name: step9vectordbcollectionname
               value: ''   # collection name in Qdrant
             - name: step9concurrency # privateGPT concurency, if greater than 1, multiple PGPT will run
               value: ''
             - name: CUDA_VISIBLE_DEVICES
               value: '' # 0 for any device or specify specific number                
             - name: step9docfolder # privateGPT docfolder to load files in Qdrant vectorDB local context
               value: ''
             - name: step9docfolderingestinterval # privateGPT docfolderingestinterval, number of seconds to wait before reloading files in docfolder
               value: ''
             - name: step9useidentifierinprompt # privateGPT useidentifierinprompt, if 1, add TML output json field Identifier, if 0 use prompt
               value: ''                              
             - name: step9searchterms # privateGPT searchterms, terms to search for in the chat response
               value: ''                                             
             - name: step9streamall # privateGPT streamall, if 1, stream all responses, even if search terms are missing, 0, if response contains search terms
               value: ''                                                            
             - name: step9temperature # privateGPT LLM temperature between 0 and 1 i.e. 0.3, if 0, LLM model is conservative, if 1 it hallucinates
               value: ''                                             
             - name: step9vectorsearchtype # privateGPT for QDrant VectorDB similarity search.  Must be either Cosine, Manhattan, Dot, Euclid
               value: ''                                                                           
             - name: step9contextwindowsize # context window size
               value: ''                                                                                          
             - name: step9pgptcontainername # privateGPT container name
               value: ''                    
             - name: step9pgpthost # privateGPT host ip i.e.: http://127.0.0.1
               value: ''                    
             - name: step9pgptport # privateGPT port i.e. 8001
               value: ''                                                  
             - name: step9vectordimension # privateGPT vector dimension
               value: ''                                                                 
             - name: step9brollbackoffset
               value: '15'
             - name: step9bdeletevectordbcount
               value: '5'
             - name: step9bvectordbpath
               value: '/rawdata/vectordb'
             - name: step9btemperature
               value: '0.1'
             - name: step9bvectordbcollectionname
               value: 'tml-llm-model-v2'
             - name: step9bollamacontainername
               value: 'maadsdocker/tml-privategpt-with-gpu-nvidia-amd64-llama3-tools'
             - name: step9bCUDA_VISIBLE_DEVICES
               value: '0'
             - name: step9bmainip
               value: 'http://127.0.0.1'
             - name: step9bmainport
               value: '11434'
             - name: step9bembedding
               value: 'nomic-embed-text'
             - name: step9bagents_topic_prompt
               value: 'iot-preprocess<<-You are a precise data analysis assistant. Your task is to point out any anomalies or interesting insights that could help improve the performance and functioning of         IoT device.  The json data are from IOT devices.  the hp field shows the data that are processed for the process variable (pv), using the process types (pt) like:         avg or average, or trend analysis, or anomprob (i.e. anomaly probability) etc.  The device being processed is in the uid field of the json.         here is the json data:              <<data>>         INSTRUCTIONS:         1. Examine each number in the json array         2. Provide a brief analysis of the results                  FORMAT YOUR RESPONSE:         - Filtered results: [list the qualifying numbers with their "uid" fields]         - Count of qualifying numbers: [number]         - Analysis: [brief explanation of what the filter revealed]                  Be precise and concise in your response.->>        iot-ml-prediction-results-output<<-You are a precise data analysis assistant. Your task is to filter and analyze numeric data based on specified criteria.        TASK: Filter numbers from the given json array using the threshold: greater than 90        Input JSON arrary:              <<data>>         INSTRUCTIONS:         1. Examine each number in the json array         2. Apply the filter condition: number > 90         3. Return only numbers that meet the criteria with their "uid" fields         4. If no numbers meet the criteria, explicitly state this         5. Provide a brief analysis of the results                  FORMAT YOUR RESPONSE:         - Filtered results: [list the qualifying numbers with their "uid" fields]         - Count of qualifying numbers: [number]         - Analysis: [brief explanation of what the filter revealed]                  Be precise and concise in your response.'
             - name: step9bteamlead_topic
               value: 'team-lead-responses'
             - name: step9bteamleadprompt
               value: 'Analyze the dataset containing IoT device monitoring records managed by individual agents.           Review all data fields to determine whether there are any issues or major concerns requiring urgent attention.           Focus on the following criteria:          1. Each record contains a unique device identifier stored in the field "uid".          2. Examine the failure probability for each device stored in the hp field.          3. Categorize the probabilities as follows:           - Low: 0% to 50%           - Medium: 51% to 75%           - High: 76% to 89%           - Urgent: 90% to 100%          Tasks:         - Identify and highlight devices (by their "uid") that have **urgent failure probabilities** (≥ 90%).         - For each flagged device, provide details and reasoning on why it may require immediate investigation.         - Only include devices that meet the urgent threshold. Do not report on low, medium, or high categories unless relevant for context.         - State clearly whether the identified issue is *urgent*.         - Do not use or generate any code, perform a reasoning-based analysis directly from the provided data.'
             - name: step9bsupervisor_topic
               value: 'supervisor-responses'
             - name: step9bagenttoolfunctions
               value: 'send_email<<-send_email<<- You are an email-sending agent. Use smtp parameters to send emails when there is an anomaly in the data, make sure to                     indicate the device name in the mainuid field. do not write a smtp script, actually send the email using the SMTP parameters                     smtp_server=                     smtp_port=0                     username=                     password=                     sender=                     recipient=                     subject=                     body=->>        average<<-average<<-You are an average agent.  Take average of the device failure probabilities.'
             - name: step9bagent_team_supervisor_topic
               value: 'all-agents-responses'                              
             - name: step9bcontextwindow
               value: '4096'                                             
             - name: step9bagenttopic
               value: 'agent-responses'                              
             - name: step9blocalmodelsfolder
               value: '/mnt/c/maads/tml-airflow/rawdata/ollama'                                             
             - name: step1solutiontitle # STEP 1 solutiontitle field can be adjusted here. 
               value: 'TML Multi-Agentic AI Solution for IoT '                              
             - name: step1description # STEP 1 description field can be adjusted here. 
               value: 'This is an awesome TML Multi-Agentic AI real-time solution for IoT device monitoring built by TSS'                                          
             - name: KUBEBROKERHOST
               value: 'kafka-service:9092'         
             - name: KAFKABROKERHOST
               value: '127.0.0.1:9092'                              
           volumes: 
           - name: dockerpath
             hostPath:
               path: /var/run/docker.sock
           - name: rawdata
             hostPath:
               path: /mnt  # CHANGE AS NEEDED TO YOUR LOCAL FOLDER the paths will be specific to your environment
   ---
     apiVersion: v1
     kind: Service
     metadata:
       name: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-visualization-service
       labels:
         app: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-visualization-service
     spec:
       type: ClusterIP
       ports:
       - port: 80 # Ingress port, if using port 443 will need to setup TLS certs
         name: p1
         protocol: TCP
         targetPort: 6060
       selector:
         app: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb

.. tip::

   In the solution YAML file above, you can adjust the **replicas** field.  Currently, **replicas: 3** for demonstration purposes. 

secrets.yml
-----------------

.. important::
   You MUST store base64 passwords in this file and apply it to the Kubernetes cluster.  

   Refer to `instructions <https://tml.readthedocs.io/en/latest/kube.html#how-to-store-secure-passwords-in-kubernetes>`_.

.. code-block:: YAML
      
      ###################secrets.yml
      apiVersion: v1
      kind: Secret
      metadata:
        name: tmlsecrets
      type: Opaque
      data:
        readthedocs: <enter your base64 password>
        githubtoken: <enter your base64 password>
        mqttpass: <enter your base64 password>
        kafkacloudpassword: <enter your base64 password>

mysql-storage.yml
------------------------

.. important::
   Copy and Paste this YAML file: mysql-storage.yml - and save it locally.

.. code-block:: YAML

      ################# mysql-storage.yml
      apiVersion: v1
      kind: PersistentVolume
      metadata:
        name: mysql-pv-volume
        labels:
          type: local
      spec:
        storageClassName: manual
        capacity:
          storage: 20Gi
        accessModes:
          - ReadWriteMany
        hostPath:
          path: "/mnt/data"
      ---
      apiVersion: v1
      kind: PersistentVolumeClaim
      metadata:
        name: mysql-pv-claim
      spec:
        storageClassName: manual
        accessModes:
          - ReadWriteMany
        resources:
          requests:
            storage: 20Gi

mysql-db-deployment.yml
------------------------

.. important::
   Copy and Paste this YAML file: mysql-db-deployment.yml - and save it locally.

.. code-block:: YAML

      ################# mysql-db-deployment.yml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: mysql
      spec:
        selector:
          matchLabels:
            app: mysql
        strategy:
          type: Recreate
        template:
          metadata:
            labels:
              app: mysql
          spec:
            containers:
            - image: maadsdocker/mysql:latest
              name: mysql
              resources:
               limits:
                memory: "512Mi"
                cpu: "1500m"
              env:
              - name: MYSQL_ROOT_PASSWORD
                value: "raspberry"
              - name: MYSQLDB
                value: "tmlids"
              - name: MYSQLDRIVERNAME
                value: "mysql"
              - name: MYSQLHOSTNAME
                value: "mysql:3306"
              - name: MYSQLMAXCONN
                value: "4"
              - name: MYSQLMAXIDLE
                value: "10"
              - name: MYSQLPASS
                value: "raspberry"
              - name: MYSQLUSER
                value: "root"
              ports:
              - containerPort: 3306
                name: mysql
              volumeMounts:
              - name: mysql-persistent-storage
                mountPath: /var/lib/mysql
            volumes:
            - name: mysql-persistent-storage
              persistentVolumeClaim:
                claimName: mysql-pv-claim
      
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: mysql-service
      spec:
        ports:
        - port: 3306
        selector:
          app: mysql

kafka.yml
------------

This is the Kafka service needed by TML pods - if using Kafka locally or on-premise.

.. code-block:: YAML
            
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: kafka
      spec:
        selector:
          matchLabels:
            app: kafka
        replicas: 1 # tells deployment to run 1 pods matching the template
        template:
          metadata:
            labels:
              app: kafka
          spec:
            containers:
            - name: kafka
              image: maadsdocker/kafka-amd64  # IF you DO NOT have NVIDIA GPU use: maadsdocker/tml-privategpt-no-gpu-amd64
              env:
              - name: KAFKA_HEAP_OPTS
                value: "-Xmx512M -Xms512M"
              - name: PORT
                value: "9092"
              - name: TSS
                value: "0"
              - name: KUBE
                value: "1"
              - name: KUBEBROKERHOST
                value: "kafka-service:9092"      
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: kafka-service
      spec:
        ports:
        - port: 9092
        selector:
          app: kafka

privategpt.yml
---------------

.. note::
    This YAML is Optional - Use Only If Step 9 Dag is used

.. important::
   Copy and Paste this YAML file: privategpt.yml - and save it locally.

.. note::
   By default this assumes you have a Nvidia GPU in your machine and so it using the Nvidia privateGPT container:

    **image: maadsdocker/tml-privategpt-with-gpu-nvidia-amd64**

   if you DO NOT have a Nvidia GPU installed then change image to:

    **image: maadsdocker/tml-privategpt-no-gpu-amd64**

.. code-block:: YAML
            
      ################# privategpt.yml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: privategpt
      spec:
        selector:
          matchLabels:
            app: privategpt
        replicas: 1 # tells deployment to run 1 pods matching the template
        template:
          metadata:
            labels:
              app: privategpt
          spec:
            containers:
            - name: privategpt
              image: --kubeprivategpt-- # IF you DO NOT have NVIDIA GPU use: maadsdocker/tml-privategpt-no-gpu-amd64
              imagePullPolicy: IfNotPresent  # You can also use Always, Never
              env:
              - name: NVIDIA_VISIBLE_DEVICES
                value: all
              - name: DP_DISABLE_HEALTHCHECKS
                value: xids
              - name: WEB_CONCURRENCY
                value: "--kubeconcur--"
              - name: GPU
                value: "1"
              - name: COLLECTION
                value: "--kubecollection--"
              - name: PORT
                value: "8001"
              - name: CUDA_VISIBLE_DEVICES
                value: "0"
              - name: TOKENIZERS_PARALLELISM
                value: "false"
              - name: temperature
                value: "--kubetemperature--"
              - name: vectorsearchtype
                value: "--kubevectorsearchtype--"
              - name: contextwindowsize
                value: "--kubecontextwindowsize--"
              - name: vectordimension
                value: "--kubevectordimension--"
              - name: mainmodel
                value: "--kubemainmodel--"
              - name: mainembedding
                value: "--kubemainembedding--"
              - name: TSS
                value: "0"
              - name: KUBE
                value: "1"
              resources:             # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
                limits:              # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
                  nvidia.com/gpu: 1  # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
              ports:
              - containerPort: 8001
            tolerations:             # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
            - key: nvidia.com/gpu    # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
              operator: Exists       # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
              effect: NoSchedule     # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU     
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: privategpt-service
        labels:
          app: privategpt-service
      spec:
        type: NodePort #Exposes the service as a node ports
        ports:
        - port: 8001
          name: p1
          protocol: TCP
          targetPort: 8001
        selector:
          app: privategpt                    

ollama.yml
---------------

.. note::
    This YAML is Optional - Use Only If Step 9b Dag is used for Multi-Agentic AI 

.. important::
   Copy and Paste this YAML file: ollama.yml - and save it locally.

.. note::
   By default this assumes you have a Nvidia GPU in your machine and so it using the Nvidia Ollama container:

    **image: maadsdocker/tml-privategpt-with-gpu-nvidia-amd64-llama3-tools**

   if you DO NOT have a Nvidia GPU installed then the Ollama container will use CPU but will be much slower.

.. code-block:: YAML
            
      ################# ollama.yml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: ollama
      spec:
        selector:
          matchLabels:
            app: ollama
        replicas: 1 # tells deployment to run 1 pods matching the template
        template:
          metadata:
            labels:
              app: ollama
          spec:
            containers:
            - name: ollama
              image: maadsdocker/tml-privategpt-with-gpu-nvidia-amd64-llama3-tools # IF you DO NOT have NVIDIA GPU then CPU will be used
              imagePullPolicy: IfNotPresent  # You can also use Always, Never
              env:
              - name: NVIDIA_VISIBLE_DEVICES
                value: all
              - name: DP_DISABLE_HEALTHCHECKS
                value: xids
              - name: WEB_CONCURRENCY
                value: "2"
              - name: GPU
                value: "1"
              - name: COLLECTION
                value: "tml-llm-model-v2"
              - name: PORT
                value: "11434"
              - name: CUDA_VISIBLE_DEVICES
                value: "0"
              - name: TOKENIZERS_PARALLELISM
                value: "false"
              - name: temperature
                value: "0.1"
              - name: rollbackoffset
                value: "15"
              - name: ollama-model
                value: "phi3:3.8b,phi3:3.8b,llama3.2:3b"
              - name: deletevectordbcount
                value: "5"
              - name: vectordbpath
                value: "/rawdata/vectordb"
              - name: topicid
                value: "-999"
              - name: enabletls
                value: "1"
              - name: partition
                value: "-1"
              - name: vectordbcollectionname
                value: "tml-llm-model-v2"
              - name: ollamacontainername
                value: "maadsdocker/tml-privategpt-with-gpu-nvidia-amd64-llama3-tools"
              - name: mainip
                value: "http://127.0.0.1"
              - name: mainport
                value: "11434"
              - name: embedding
                value: "nomic-embed-text"
              - name: agents_topic_prompt
                value: "iot-preprocess<<-You are a precise data analysis assistant. Your task is to point out any anomalies or interesting insights that could help improve the performance and functioning of         IoT device.  The json data are from IOT devices.  the hp field shows the data that are processed for the process variable (pv), using the process types (pt) like:         avg or average, or trend analysis, or anomprob (i.e. anomaly probability) etc.  The device being processed is in the uid field of the json.         here is the json data:              <<data>>         INSTRUCTIONS:         1. Examine each number in the json array         2. Provide a brief analysis of the results                  FORMAT YOUR RESPONSE:         - Filtered results: [list the qualifying numbers with their "uid" fields]         - Count of qualifying numbers: [number]         - Analysis: [brief explanation of what the filter revealed]                  Be precise and concise in your response.->>        iot-ml-prediction-results-output<<-You are a precise data analysis assistant. Your task is to filter and analyze numeric data based on specified criteria.        TASK: Filter numbers from the given json array using the threshold: greater than 90        Input JSON arrary:              <<data>>         INSTRUCTIONS:         1. Examine each number in the json array         2. Apply the filter condition: number > 90         3. Return only numbers that meet the criteria with their "uid" fields         4. If no numbers meet the criteria, explicitly state this         5. Provide a brief analysis of the results                  FORMAT YOUR RESPONSE:         - Filtered results: [list the qualifying numbers with their "uid" fields]         - Count of qualifying numbers: [number]         - Analysis: [brief explanation of what the filter revealed]                  Be precise and concise in your response."
              - name: teamlead_topic
                value: "team-lead-responses"
              - name: teamleadprompt
                value: "Analyze the dataset containing IoT device monitoring records managed by individual agents.          Review all data fields to determine whether there are any issues or major concerns requiring urgent attention.         Focus on the following criteria:         1. Each record contains a unique device identifier stored in the field "uid".         2. Examine the failure probability for each device stored in the hp field.         3. Categorize the probabilities as follows:          - Low: 0% to 50%          - Medium: 51% to 75%          - High: 76% to 89%          - Urgent: 90% to 100%        Tasks:        - Identify and highlight devices (by their "uid") that have **urgent failure probabilities** (≥ 90%).        - For each flagged device, provide details and reasoning on why it may require immediate investigation.        - Only include devices that meet the urgent threshold. Do not report on low, medium, or high categories unless relevant for context.        - State clearly whether the identified issue is *urgent*.        - Do not use or generate any code, perform a reasoning-based analysis directly from the provided data."
              - name: supervisor_topic
                value: "supervisor-responses"
              - name: supervisorprompt
                value: "You are a team supervisor analyzing operational device data and recommending whether an alert email should be send.          You manage a send email expert and a average expert.         For send email, use send_email agent.         For average, use average agent.       INSTRUCTIONS:       1.Analyze the Team Lead assessment and determine the proper action:       - If devices are marked urgent or failure probabilities exceed 90%, select "send_email".       - If no urgent devices are found or probabilities remain below thresholds, then no action is needed."
              - name: agenttoolfunctions
                value: "send_email<<-send_email<<- You are an email-sending agent. Use smtp parameters to send emails when there is an anomaly in the data, make sure to                     indicate the device name in the mainuid field. do not write a smtp script, actually send the email using the SMTP parameters                     smtp_server=                     smtp_port=0                     username=                     password=                     sender=                     recipient=                     subject=                     body=->>        average<<-average<<-You are an average agent.  Take average of the device failure probabilities."
              - name: agent_team_supervisor_topic
                value: "all-agents-responses"
              - name: TSS
                value: "0"
              - name: KUBE
                value: "1"
              resources:             # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
                limits:              # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
                  nvidia.com/gpu: 1  # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
              ports:
              - containerPort: 11434
            tolerations:             # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
            - key: nvidia.com/gpu    # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
              operator: Exists       # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU
              effect: NoSchedule     # REMOVE or COMMENT OUT: IF you DO NOT have NVIDIA GPU     
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: ollama-service
        labels:
          app: ollama-service
      spec:
        type: NodePort #Exposes the service as a node ports
        ports:
        - port: 11434
          name: p1
          protocol: TCP
          targetPort: 11434
        selector:
          app: ollama                    

qdrant.yml
---------------

.. note::
    This YAML is Optional - Use Only If Step 9 Dag is used

.. important::
   Copy and Paste this YAML file: qdrant.yml - and save it locally.

.. code-block:: YAML

      ################# qdrant.yml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: qdrant
      spec:
        selector:
          matchLabels:
            app: qdrant
        replicas: 1 
        template:
          metadata:
            labels:
              app: qdrant
          spec:
            #hostNetwork: true
            containers:
            - name: qdrant
              image: qdrant/qdrant 
              ports:   
              - containerPort: 6333
              volumeMounts:
              - mountPath: /qdrant/storage
                name: qdata
            volumes:
            - name: qdata
              hostPath:
                path: /qdrant_storage          
      ---
      apiVersion: v1
      kind: Service
      metadata:
        name: qdrant-service
        labels:
          app: qdrant-service
      spec:
        type: NodePort #Exposes the service as a node ports
        ports:
        - port: 6333
          name: p1
          protocol: TCP
          targetPort: 6333
        selector:
          app: qdrant
          
.. tip::
   The number of replicas can be changed in the **cybersecuritywithprivategpt-3f10.yml** file: look for **replicas**.  You can increase or decrease the number of replicas based on the amout of real-time data you are processing.

Kubernetes Dashboard Visualization
----------------------------------

To visualize the dashboard you need to forward ports to your solution **deployment in Kubernetes**.  For this solution, the port forward command would be:

.. code-block::

   kubectl port-forward deployment/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb 6060:6060

After you forward the ports then copy/paste the viusalization URL below and run your dashboard.

.. code-block::

   http://localhost:6060/iot-failure-machinelearning-agenticai.html?topic=all-agents-responses,iot-preprocess,iot-ml-prediction-results-output&offset=-1&groupid=&rollbackoffset=100&topictype=prediction&append=0&secure=1

Scaling with NGINX Ingress and Ingress Controller
-------------------------------------

All TML solutions will scale with NGINX ingress to perform load-balancing.  But, before you can use ingress - ingress MUST be enabled in Kubernetes cluster.  Follow these steps:

.. important::
   **STEP 1:  To turn on ingress in minikube type:**

   .. code-block::

      minikube addons enable ingress

   .. code-block::

      minikube addons enable ingress-dns

   **STEP 2:  In Linux Add tml.tss domain name to /etc/hosts file**

    a. Edit your **/etc/hosts** file 

    b. add an entry to **/etc/hosts**: 

      .. code-block::
     
         127.0.0.1 tml.tss

    c. Save the file

   **STEP 2b:  In Windows Add tss.tml domain name to C:\\Windows\\System32\\drivers\\etc**

    a. Edit your **C:\\Windows\\System32\\drivers\\etc\\hosts** file  (Note: You may need to COPY the hosts file to another directory, then edit the file, then copy it back to 
       **C:\\Windows\\System32\\drivers\\etc\\hosts**

    b. add an entry: 

       .. code-block:: 

          127.0.0.1 tml.tss

    c. Save the file 

    d. copy it back to **C:\\Windows\\System32\\drivers\\etc\\hosts**

   **STEP 3:  In a new Linux terminal you MUST turn on minikube tunnel type:**

   .. code-block::

      minikube tunnel

   **STEP 4:  Apply nginx-ingress-tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml to your kubernetes cluster.  First you need to save it locally then apply it:**

nginx-ingress-tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml
-------------

   .. code-block::

      
    ############# nginx-ingress-tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: tml-ingress
      annotations:
        nginx.ingress.kubernetes.io/use-regex: "true"
        nginx.ingress.kubernetes.io/rewrite-target: /$2
    spec:
      ingressClassName: nginx
      rules:
        - host: tml.tss
          http:
            paths:
              - path: /viz(/|$)(.*)
                pathType: ImplementationSpecific
                backend:
                  service:
                    name: tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-visualization-service
                    port:
                      number: 80
    ---
    apiVersion: v1
    kind: ConfigMap
    apiVersion: v1
    metadata:
      name: ingress-nginx-controller
      namespace: ingress-nginx
    data:
      allow-snippet-annotations: "true"
  

   .. code-block::

      kubectl apply -f nginx-ingress-tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb.yml

You are now ready to run the Dashboard using Ingress load balancing.

Ingress Dashboard Visualization
-------------------

Copy and paste this URL below in your browser and start streaming.  Because you are now using INGRESS, Kubernetes will perform load balancing on the streaming data.

.. code-block::

   http://tml.tss/viz/iot-failure-machinelearning-agenticai.html?topic=all-agents-responses,iot-preprocess,iot-ml-prediction-results-output&offset=-1&groupid=&rollbackoffset=100&topictype=prediction&append=0&secure=1

Making Secure TLS Connection with gRPC
-----------------------

You can make secure connection through the NGINX controller to your TML service using TLS encryption.  TML solutions utilizing gRPC have a built-in gRPC server for secure and fast connections.

.. note::
   Note that the REST API service is unencrypted, but very useful if you don't have the need for security inside your VM.

.. attenton::
   These configurations are for advanced, yet more powerful and secure, usage of TML.

Here are the Steps to follow.
"""""""""""""""""""""""""""""""""""

Step 1: Get Server Certificates
^^^^^^^^^^^^^^^^^^^^^^^

To utilize the secure gRPC service you must have valid security certificates for each server.  You can self-sign your own certificates by following the steps here: 
`Steps to Self-Signing Certificates <https://github.com/smaurice101/raspberrypi/blob/main/tml-airflow/certs/san-keycreationprocess.md>`_

.. tip::
   You can download the all the certificates from the `certs repo <https://github.com/smaurice101/raspberrypi/tree/main/tml-airflow/certs>`_

Step 2: Apply the Server Certificates to Kubernetes Cluster
^^^^^^^^^^^^^^^^^^

.. attention::
   These certificates are for the **tml.tss** sever.  Follow the steps to add this host to your /etc/hosts file: :ref:Scaling with NGINX Ingress and Ingress Controller`

   If you have a different host, then you will need to re-generate these certificates for your new host, replace tls.crt and tls.key with your your new keys.  **Note these keys are in 
   base64 format for security.** To replace with your own hosts, just update the the `san.cnf file <https://github.com/smaurice101/raspberrypi/blob/main/tml-airflow/certs/san.cnf>`_

.. code-block::

      ########## NOTE: tls certs are for tml.tss server only
      # You can replace tls.crt and tls.key with your own server.crt and server.key
      #############secret-tls.yml
      apiVersion: v1
      data:                  
        tls.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUY5ekNDQTkrZ0F3SUJBZ0lVWFlPeUJia21qaUxMMDFiMkI2NVI5UllHbVNBd0RRWUpLb1pJaHZjTkFRRUwKQlFBd1ZURUxNQWtHQTFVRUJoTUNRMEV4RURBT0JnTlZCQWdNQjA5dWRHRnlhVzh4RURBT0JnTlZCQWNNQjFSdgpjbTl1ZEc4eERqQU1CZ05WQkFvTUJVOTBhV056TVJJd0VBWURWUVFEREFsVFpXSmhjM1JwWVc0d0lCY05NalF4Ck1qRTVNVFF4T1RJeldoZ1BNakV5TkRFeE1qVXhOREU1TWpOYU1GVXhDekFKQmdOVkJBWVRBa05CTVJBd0RnWUQKVlFRSURBZFBiblJoY21sdk1SQXdEZ1lEVlFRSERBZFViM0p2Ym5Sdk1RNHdEQVlEVlFRS0RBVlBkR2xqY3pFUwpNQkFHQTFVRUF3d0pVMlZpWVhOMGFXRnVNSUlDSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQWc4QU1JSUNDZ0tDCkFnRUE3SEdwMFVVV2toejIzWVpRWE41VEVSR3JHYk43RzdQRWI3dEtRTWFObUtkVmFRQjlpRmVqQ3ZtUWxYNXUKcjZjMjF6VkMrVC8zZ0lQZmN3bW9SNzVDZGQ4dDhYRkVIOXRFSVBsZXYrbzVRNzFHajRqdnh1YTBBeU1KQlVuRwpNeEZTT2Qxek9heVlGSDFNMk94R0RUOU1RT3liZEFLd3pGU2YyZHlDcUx3cW0rWTJjWlIrbkFWOS81T1RaOVhiCktmVjJwWFNyRjVhcGovQTFCL3lDTlpDb284Nit4bFBFajRISGhxV1NvVkZxT1dhMlZ4dC9UQjlnKzFyL2hGOWEKV2g5UTZZNmRtK1FYUnhaWGxZR1pzdGVmbjFCcmY2cHdrWTRJZmtPd3RCb29EUnhUajg5U213bjdwaDJQczdFMQpvU2NoYkVkSDZLSjIxZ3JVSXEwZFMzaEZES2pYL2UrT2xIclk1bE42UmJqLzAwUjVhenRMcXFQWjFGNVdBTHNsCnp5MzV2SHNoR2dzVVRNVGVZLzQ1RmpMamJrLzdMckdOZWlhQUZXcFkrb0oyTUF6ZUN1QnlRRW95OWhrLzVjNTUKS0IyMVNzdU02M3Q0bElQV0VGNFBrbUFGNHFDNEZOMldLQ2E2RTgyWTFsdmxYQmNHL2dZalZ0ZTdmTzdaLy9MRQp6Vk5tOHJqRGVXTUR6eEdTYzIvVXQ3QzJKVkF6VlRlWmZ5Vm9TZWtMZmZ1S1FreWxRUERHMGVQRXR6UEEyYjI3CkE1dTdsaThaV1RMUEs3WW0xTXNPOHN5ZmNmTTI1c0had2pDVVBOdzNhbXl6blRtLzhtb2FheVR2RWZZaTlIVjYKejlNY3Y1dlltdFhDaDZxSm00RHNLbmdGMkNrd29DMldmQjRjVjRvRDE2RStETGNDQXdFQUFhT0J2RENCdVRBYwpCZ05WSFJFRUZUQVRnZ2QwYld3dWRITnpnZ2gwYld3dWRITnpNakFkQmdOVkhRNEVGZ1FVVVZ2aE5zS3RHaURVCkFQTXo0Ni8wV2s0QkpuWXdlZ1lEVlIwakJITXdjYUZacEZjd1ZURUxNQWtHQTFVRUJoTUNRMEV4RURBT0JnTlYKQkFnTUIwOXVkR0Z5YVc4eEVEQU9CZ05WQkFjTUIxUnZjbTl1ZEc4eERqQU1CZ05WQkFvTUJVOTBhV056TVJJdwpFQVlEVlFRRERBbFRaV0poYzNScFlXNkNGQk5aVGFPVStaQlVDa29LVVJzZ2o3QVN0ZFBzTUEwR0NTcUdTSWIzCkRRRUJDd1VBQTRJQ0FRQjAwUThodVJ2d3RvTzFkQjZKRzBHTjE1bXJGYUhyRlR4dnU3RFlPK0xWc2RtQmxoNVoKUFFSUVdjUUZaeFRvVmJZbldPY2hmY1VMK3padVo2QVdQOVp4RHljWjNrT3JFUm9EbjJzL0wzSk5sK0IyRGhmUQpwUytMeWdLMWt6RGhEeWI2dE0rSVdTRzY2RWdIZHF0U0dDM2czWXR0eTNEZTZidzFzSERwTkg3Q08wNndId1Y4CkdCVC9PempoS2U2M3RyaGxZZmtvM1IrQnFQTWd0YlN4MTlOOVBBY0FRbFgrQVJEckJ5a2tZZkExVVpKcml5aHQKRllZNXdmcmpJcTB5N0RtSDBYVkwxRkhNKzFCdFJrcnJxU1pSWHYybjRlTnFuMlRiUjZpdTdWS0pjdnVCYkYrMwpMRDdEZDBjdjRYSmxxektyUWtWR0R5T3M2bVMvcEhPcVhMNmNOREdpc1BpSFRRWkppcEFaUFpQQ3pXUjdjam1WCmkyVmZMN3NPTmlReXY5WFIyYzYrQndFbXk3bDg1dnBmVG9VaHN2R1lMemJjcndDM1lOL0d1WnQzQW5EQ1cyT04KV05lcldSTmhrYjFSdUFtWVREY21jb0t0OSt2RFY1M2NtK1BJUno1ZldhUUNXYjhXdDFuMjFkNitoRWtBdVcxRQppQmh0RFJYcy8xeUkxOEI1UlFGSU44ZkNQMnh5WWR1b0MzbldnTUR5NzRkcUNsT0hlTmhEUFIyM3NZNjR4bmhKCkRBcHh2dTBwT3crTno2bjcyU0o4Ky9TODlaRW1jQy9nWFNnOVZ0SEYzNDR6YzVma3ZiMFZyWDRqVHA3R1JLSTEKRlZOWkRBbml3TXN5eGJnUEJEMGEwb1gwTlY0OEJUTkdaTzNWRzVLdVpwN3BXYmxseHh6QldHWnVvZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K                                                                                                                                                                        
        tls.key: LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk1JSUpRd0lCQURBTkJna3Foa2lHOXcwQkFRRUZBQVNDQ1Mwd2dna3BBZ0VBQW9JQ0FRRHNjYW5SUlJhU0hQYmQKaGxCYzNsTVJFYXNaczNzYnM4UnZ1MHBBeG8yWXAxVnBBSDJJVjZNSytaQ1ZmbTZ2cHpiWE5VTDVQL2VBZzk5egpDYWhIdmtKMTN5M3hjVVFmMjBRZytWNi82amxEdlVhUGlPL0c1clFESXdrRlNjWXpFVkk1M1hNNXJKZ1VmVXpZCjdFWU5QMHhBN0p0MEFyRE1WSi9aM0lLb3ZDcWI1alp4bEg2Y0JYMy9rNU5uMWRzcDlYYWxkS3NYbHFtUDhEVUgKL0lJMWtLaWp6cjdHVThTUGdjZUdwWktoVVdvNVpyWlhHMzlNSDJEN1d2K0VYMXBhSDFEcGpwMmI1QmRIRmxlVgpnWm15MTUrZlVHdC9xbkNSamdoK1E3QzBHaWdOSEZPUHoxS2JDZnVtSFkrenNUV2hKeUZzUjBmb29uYldDdFFpCnJSMUxlRVVNcU5mOTc0NlVldGptVTNwRnVQL1RSSGxyTzB1cW85blVYbFlBdXlYUExmbThleUVhQ3hSTXhONWoKL2prV011TnVUL3N1c1kxNkpvQVZhbGo2Z25Zd0RONEs0SEpBU2pMMkdUL2x6bmtvSGJWS3k0enJlM2lVZzlZUQpYZytTWUFYaW9MZ1UzWllvSnJvVHpaaldXK1ZjRndiK0JpTlcxN3Q4N3RuLzhzVE5VMmJ5dU1ONVl3UFBFWkp6CmI5UzNzTFlsVUROVk41bC9KV2hKNlF0OSs0cENUS1ZBOE1iUjQ4UzNNOERadmJzRG03dVdMeGxaTXM4cnRpYlUKeXc3eXpKOXg4emJtd2RuQ01KUTgzRGRxYkxPZE9iL3lhaHBySk84UjlpTDBkWHJQMHh5L205aWExY0tIcW9tYgpnT3dxZUFYWUtUQ2dMWlo4SGh4WGlnUFhvVDRNdHdJREFRQUJBb0lDQUQ1UnVSSWsxUTJlMzd4RWpnTGtRRjJqCjNBYVNwVlNJWGJLYldUZFlmZktwekJ1NFd0M29SMXQ1cXMrVU91VkdPL0NlSTdCaFdVRkF3TkRuenpoVm45dkUKZnE0QURoWWRhMGdMb2hzUVI1YWdtU3YweWtvUS9ZcEVIamtNR0ZiV2JtYzlCSVZEaGZRRWtKQXVPa3A4a0FNZQp1ZHhxWnlINy9nUGttSFdUM3VFblhOc3o2ZWtDazVLYzJZSEpQcEpCRmN3SFE1OGNnVVdrYUwzWm9wSXV0aHd5CnZscTBzbjZtbEtuYkV4bzh4TFFyYTh6cXZQTVo1Q3hyOENQNkkrelVDelg3OW5PanV6VHI0UnJSUldyN1pTR1AKQno1bmRITVF6aEZGa3hudE9QZzNxcGloYXVMZFR6d1oxNG5qbjhDQmVWQTZPMnhJQWUxcGZqOURoSkNqT3dOWQo5Y0RsYUEwVXpqOXcxTjdZdjk0RERDWWJIVVJweldQNXQ2TUo4ZFp3eHIxNmV6eG01ejlPaExTZzBzWWx6Z3k0CjA2YlRia2pIMlg1S05tZlNjK05saDh0T2t2QnRVSXgydUZvNkZWRFVhKzRKSUhyNVRtUUZMbS92OE5ha0puZWsKbkF0UGpSL0ZubzNGT1F0Ynpja2xJNVVXaVBHWWczQ3Y3Z3ZqYkVuUXJtT3B5Z3QxcFdWVnpTRnhCWjl1QmRxcgpod01USHlURHFxTDBtaGg4UXhnOGR2NllBR0srR3lSQmU3SDVENTdqVXE0eG5ML20zWVA2SERnV2x0YTg4MTVlCmtlalhkVUcvMk9qVmEwZmNxK2x2d3htRUJ3YXJueGw2VUVpTGhlb1JzSkFaZkl6L1hFMlhxYStnaWpwcDRRbFgKTkxNeXNQNWVYb0JxNXArOUQzZnBBb0lCQVFEN2wrMEx5ZFVkdERCdU5EQ0dUSGhBS1N1Z05HWDZyK2Fady9XOQpyVkZRYWJTdWhjYy9MMmxIUHNsN0NNamZrNVk5b1BxazZUYUJKR0NhMFFBWGc5bTBCR09CMUZhd203Z0F5YlIyCldJWlViaGxybjBBM29zU3J3YUVnc3BZUG5WRTlQOU9GUGdhWWoyd2NNSkVleENRb2t4QmlvOFQwSElGQklWQUUKTGExTmQvcFl0bUNYeGtncjlnWHh3QlpaZlJjbk93V0dkOVBvMDN6Y3NVOWVsSE1kOG1Db1dQbGtGcXBQN2VXKwp5VHo5Q3ovWUgrTTYyWFBOQldJM2VNRWpuMlBaUEp1WnFmcVN4cVcwMWJSdU1lbFgrd1dSeEdHK1hSS3l5cENlCkV4N0xTd3pNdk9QQVVkVVFrT3A0ZEFXZGFQeGNXcUFGVU5Cb1VTY2xGMmtOTlRVL0FvSUJBUUR3bGMrOHBxSDIKV2dtOW83RnltSFhHdldJdmIvMTBpRFpFZ1gzK3dTU1Y5SW5mNEtINDZvM29ER21tTWlYS2R3T1FYenVLTksycgpSSHNOZEl4VG05cnFxa3BNckZGaUplZ3JuTU43VFpON0pnaHE4SkNkR2JUb0xPYTNVTzJSWnRSZ0phOUVPZjhXClFBRmxqeTRnN294ZktudURORWZGNUFzTW5PaTdTWFNQSXpxdDBXMGdZcmdQaGd1YUFGSFIyMzNJRkRkeG02d0sKTGJkSDdmS09QQ1diVEpQVzQvQTRDaks3WVVKWERzUHFmbjBYT2JNWDlWSVNDLzM2M1JqbjQ1S3RuVUtJa2RCRgpIWkxLc1hvOU0wT2ZUb0dTdlNsV0g0SVZmTE81NTlsM2FFVHp2OXZQVzRqcmE1Rkc3TEV6a3FiSXRUWkEyemp2Cmgwa0JRMk4xMGZLSkFvSUJBRGJqckdtMy9QRGdFUGphRmdRV3h0MW9uZ1h6cUpRS3NFcTN2L05EenN1MlpCNzMKUE1NQ092dTZMUWJVb2M1MVNuL2prUXROZmdDcXlSQzlyRUYxR0pmM3BTWDhCM1c4WTJaNG14Qit1Ny9Meld2MwpjSEV5NTZsNU13Z0pMa2YxMEhXR2FVVldoT1hmMUh4SjlEODhGNDlxbGxhTzJEZFJ5TGxHNVVna0Z2MGh3ZEo4CjU1SDFSbVdnNVNjYSswVkd6emhWM2h5Nkk5ZFYzSlhoY1NsM1JhNHc1UG1WZjhOZ1ZvUGRxUlA0bjMrdFpwNW0KUnBMZVFpOW1qMGorNVZRNlAvUnpEcGQxeUI4aGk2RnFSbFVNT3BaaFE1UEx2bTlqcXVLcTR1WTUwYXdVa1pSUgpXWGJwNDR3YnNhdloxQ2ZGY2RsTVJFRWtvbk0vMFVSOFdRVHlxTTBDZ2dFQkFNUUVDMkZWRXBpNCt6NjdaQlJPCkM0ZUZQYjRRckp5SmJrMmFnNkZRbEJKcFR2eE05U3J0VC9sRVE3L1pFOWxGNW0xMmFmaE11MExUWkw2dHVyZFUKUUtUNVlkZmVmZUJOcWovK1ZYYmMyZEI0U0Z0NDdScFNtNGFmTHNzazhLcUs4WFgwdmp3RVZNVTRHT3M2SVFkTAoxS3FrM2tVa0QyWTRTcGhZTDNhSWZxTXd2TnBweTFPYm13TnEzNEQxeWJRRjlSRlRCMmxVd0hMNmxGM1NqTkUrClNCV2o2c0FtcnMyNTRXT3g5bThmNUpmbHZ0MXhjVzJQdnZKZE91MXR2cUVRVmEyR2QzTDErbzZWYmNnZm1jekwKTzhsTUdWNEpLT2kyZXpJdWkvQm42bExUYlhwN1V3ZzdOKzgza1FJTVRzUUtORUZMQTQwTUQvTjRjZzdKYlB2Tgp0cUVDZ2dFQkFNK1Y0VGp3N2xTblQzMUdyNzl6RFQ2bnIrYytOVE96VnBxNG4zcTVnbk5SUklKamN1NkxPRXhvCmRhSVFWZlcxU0hGUVowcXFKYXJoZmZlRnQ0OVd5SW5lcHY4TTJWNmFDMkFZdm1qazhuMHlHcUFqOE1zLzVBTTQKQ1lKRDJ1Tm1EZDI5TkxLTEdFbzNaNFE0c1MrMVY3aXRtbGw5V3lYQWNiUStienAvbEhaa0ZNNzFNUGtuN0ErcwpmOHhUekhYYVR5YjQ2b3pBMmxyQmVFQ2QrMGdyaVZLZG51V2pqT0Z5YldldHIyTXcyR05yWEF6RHpCa21Kc1JTCnhmbVFVSGw2OXlSc0M4Q05Hd3k3VGJxQ3gwOEJ5ZHAzWG9yR1ZyakpRak95Y0p2ODErQkl3V2YrcUp2THlFRWgKbTgvQjN4RkUyTkNITHRmVEtKcVFabDZEQkhXK2xBaz0KLS0tLS1FTkQgUFJJVkFURSBLRVktLS0tLQo=  
      kind: Secret
      metadata:
        creationTimestamp: "2024-12-19T15:56:35Z"
        name: self-tls
        namespace: default
      type: kubernetes.io/tls

Step 3: Apply the secret-tls to Kubernetes Cluster
^^^^^^^^^^^^^^^^^^^^

Save the Yaml in Step 2 locally, then apply it to the cluster.

.. code-block::

    kubectl apply -f secret-tls.yml

You are done!  You know have a secure connection betweenn your client gRPC application and the TML solution.

Using gRPcurl to Write Data to the TML gRPC Server
-------------------------------------

gRPcurl is a utility for writing data to your gRPC solution.

.. tip::
   You can install gRPCurl from `here <https://github.com/fullstorydev/grpcurl/releases>`_.

.. important::
   You must download four (4) files to your local machines:

    1. ca.crt
      a. Get it from `here <https://github.com/smaurice101/raspberrypi/tree/main/tml-airflow/certs>`_

    2. tml_grpc.proto, tml_grpc_pb2_grpc.py, tml_grpc_pb2.py
      a.  Get them from `here <https://github.com/smaurice101/raspberrypi/tree/main/tml-airflow/dags>`_

    
Run the gRPCurl Commands
""""""""""""""""""""""""""

Once your TML solution using gRPC is running in Kubernetes you can test it by sending data with these commands:

.. code-block::

   grpcurl -insecure -H "client-api-protocol: 1,1" -cacert ca.crt -import-path . -proto tml_grpc.proto  tml.tss:443 list tmlproto.Tmlproto

.. code-block::

   grpcurl -insecure -H "client-api-protocol: 1,1" -cacert ca.crt -import-path . -proto tml_grpc.proto  tml.tss:443 list

.. code-block::

   grpcurl -insecure -H "client-api-protocol: 1,1" -cacert ca.crt -import-path . -proto tml_grpc.proto  tml.tss:443 describe tmlproto.Tmlproto.GetServerResponse

.. code-block::

   grpcurl -insecure -H "client-api-protocol: 1,1" -cacert ca.crt -import-path . -proto tml_grpc.proto  tml.tss:443 describe .tmlproto.Message

.. code-block::

   grpcurl -insecure -H "client-api-protocol: 1,1" -cacert ca.crt -import-path . -proto tml_grpc.proto -msg-template tml.tss:443 describe .tmlproto.Message

Send data to the sever:
""""""""""""""""""""
.. code-block::

   grpcurl -insecure -H "client-api-protocol: 1,1" -cacert ca.crt -import-path . -proto tml_grpc.proto -d '{"message":"admin yeah!!"}' tml.tss:443 tmlproto.Tmlproto/GetServerResponse


Kubernetes Pod Access Commands
---------------------

**To go inside the pods, you can type command:** 

.. code-block::

   kubectl exec -it <pod name> -- bash 

Note: replace **<pod name>** with actual pod name..use this command to get the pod name

.. code-block::

   kubectl get pods -A

**To list service pods type:**

.. code-block::

   kubectl get svc -A

**To list deployment pods type:**

.. code-block::

   kubectl get deployments -A

**To Horizontally AUTO-SCALE Deployments type:**

  .. code-block::

     kubectl autoscale deployment  <deployment name> --cpu-percent=50 --min=1 --max=100

.. important::

   The above command instructs Kubernetes to scale pods based on 50% CPU utilization to a minimum number of pods of 1 (small workload) to a maximum of 100 pods for large world loads.  Of 
   course, you can easily change these min and max numbers.
   
   This auto-scaling is very important to scale up and down your solution, while efficiently managing cloud computing costs.

**To list deployments being auto-scaled type:**

  .. code-block::

     kubectl get hpa -A

**To delete the pods:**

.. code-block::

   kubectl delete all --all --all-namespaces

**To get information on a pod type:**

.. code-block:: 

   kubectl describe pod <pod name>

**Start minikube with NVIDIA GPU Access:**

.. code-block::

     minikube start --driver docker --container-runtime docker --gpus all --cni calico --memory 8192

.. note::

   Note you may need to type: **./minikube**

**Start minikube with NO GPU:**

.. code-block::

   minikube start --driver docker --container-runtime docker --cni calico --memory 8192

**DELETE minikube:**

.. code-block::

   minikube delete

.. tip::

   Adjust the **\-\-memory 8192** as needed.
