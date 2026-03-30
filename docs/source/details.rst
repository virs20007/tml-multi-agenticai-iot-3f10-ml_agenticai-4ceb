[tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb] Details
============================

Generated On: 2026-03-30 21:38:21 UTC

TML Solution DAG Parameters' Details: User Chosen Parametets
----------------------------

STEP 1: Get TML Core Params: `tml_system_step_1_getparams_dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_1_getparams_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - solutionname
     - tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb
   * - solutiontitle
     - TML Multi-Agentic AI Solution for IoT 
   * - solutiondescription
     - This is an awesome TML Multi-Agentic AI real-time solution for IoT device monitoring built by TSS
   * - brokerhost
     - 127.0.0.1
   * - brokerport
     - 9092
   * - cloudusername
     - None
   * - ingestdatamethod
     - LOCALFILE
 
STEP 2: Create Kafka Topics: `tml_system_step_2_kafka_createtopic_dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_2_kafka_createtopic_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - companyname
     - Otics
   * - myname
     - Sebastian
   * - myemail
     - Sebastian.Maurice
   * - mylocation
     - Toronto
   * - replication
     - 1
   * - numpartitions
     - 1
   * - enabletls
     - 1
   * - microserviceid
     - 
   * - raw_data_topic
     - iot-raw-data,agent-responses,team-lead-responses,supervisor-responses,all-agents-responses
   * - preprocess_data_topic
     - iot-preprocess,iot-preprocess2
   * - ml_data_topic
     - ml-data
   * - prediction_data_topic
     - iot-ml-prediction-results-output

STEP 3: `Produce to Kafka Topics <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_read_LOCALFILE_step_3_kafka_producetotopic_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - PRODUCETYPE
     - LOCALFILE
   * - inputfile
     - /rawdatademo/IoTData.txt
   * - TOPIC
     - iot-raw-data
   * - PORT
     - _42275
   * - IDENTIFIER
     - TML Multi-Agentic Solution,/rawdatademo/IoTData.txt
   * - HTTPADDR
     - https://
   * - FROMHOST
     - ('ede0d2e91954', '172.17.0.2')
   * - TOHOST
     - 172.17.0.2
   * - CLIENTPORT
     - Not Applicable
   * - TSS_CLIENTPORT
     - Not Applicable
   * - TML_CLIENTPORT
     - Not Applicable
   * - docfolder
     - 
   * - doctopic
     - 
   * - chunks
     - 3000
   * - docingestinterval
     - 0

STEP 4: Preprocesing Data: `tml-system-step-4-kafka-preprocess-dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_4_kafka_preprocess_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - raw_data_topic
     - iot-raw-data,agent-responses,team-lead-responses,supervisor-responses,all-agents-responses
   * - preprocess_data_topic
     - iot-preprocess,iot-preprocess2
   * - preprocessconditions
     - 
   * - delay
     - 70
   * - maxrows
     - 800
   * - array
     - 0
   * - saveasarray
     - 1
   * - topicid
     - -999
   * - rawdataoutput
     - 1
   * - asynctimeout
     - 120
   * - timedelay
     - 0
   * - preprocesstypes
     - anomprob,trend,avg
   * - pathtotmlattrs
     - --pathtotmlattrs--
   * - identifier
     - IoT TML Multi-Agentic AI device performance and failures
   * - jsoncriteria
     - uid=metadata.dsn,filter:allrecords~subtopics=metadata.property_name~values=datapoint.value~identifiers=metadata.display_name~datetime=datapoint.updated_at~msgid=datapoint.id~latlong=lat:long

STEP 4a: Preprocesing Data: `tml-system-step-4a-kafka-preprocess-dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_4a_kafka_preprocess_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - raw_data_topic
     - --raw_data_topic1--
   * - preprocess_data_topic
     - --preprocess_data_topic1--
   * - preprocessconditions
     - --preprocessconditions1--
   * - delay
     - --delay1--
   * - maxrows
     - --maxrows1--
   * - array
     - --array1--
   * - saveasarray
     - --saveasarray1--
   * - topicid
     - --topicid1--
   * - rawdataoutput
     - --rawdataoutput1--
   * - asynctimeout
     - --asynctimeout1--
   * - timedelay
     - --timedelay1--
   * - preprocesstypes
     - --preprocesstypes1--
   * - pathtotmlattrs
     - --pathtotmlattrs1--
   * - identifier
     - --identifier1--
   * - jsoncriteria
     - --jsoncriteria1--

STEP 4b: Preprocesing Data: `tml-system-step-4b-kafka-preprocess-dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_4b_kafka_preprocess_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - raw_data_topic
     - --raw_data_topic2--
   * - preprocess_data_topic
     - --preprocess_data_topic2--
   * - preprocessconditions
     - --preprocessconditions2--
   * - delay
     - --delay2--
   * - maxrows
     - --maxrows2--
   * - array
     - --array2--
   * - saveasarray
     - --saveasarray2--
   * - topicid
     - --topicid2--
   * - rawdataoutput
     - --rawdataoutput2--
   * - asynctimeout
     - --asynctimeout2--
   * - timedelay
     - --timedelay2--
   * - preprocesstypes
     - --preprocesstypes2--
   * - pathtotmlattrs
     - --pathtotmlattrs2--
   * - identifier
     - --identifier2--
   * - jsoncriteria
     - --jsoncriteria2--

STEP 4c: Preprocesing Data: `tml-system-step-4c-kafka-preprocess-dag  <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_4c_kafka_preprocess_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - raw_data_topic
     - --raw_data_topic3--
   * - preprocess_data_topic
     - --preprocess_data_topic3--
   * - delay
     - --delay3--
   * - maxrows
     - --maxrows3--
   * - array
     - --array3--
   * - saveasarray
     - --saveasarray3--
   * - topicid
     - --topicid3--
   * - rawdataoutput
     - --rawdataoutput3--
   * - asynctimeout
     - --asynctimeout3--
   * - timedelay
     - --timedelay3--
   * - searchterms
     - --rtmssearchterms--
   * - rtmsstream
     - --rtmsstream--
   * - identifier
     - --identifier3--
   * - rememberpastwindows
     - --rememberpastwindows--
   * - patternwindowthreshold
     - --patternwindowthreshold--
   * - localsearchtermfolder
     - --localsearchtermfolder--
   * - localsearchtermfolderinterval
     - --localsearchtermfolderinterval--
   * - rtmsscorethreshold
     - --rtmsscorethreshold--
   * - rtmsscorethresholdtopic
     - --rtmsscorethresholdtopic--
   * - attackscorethreshold
     - --attackscorethreshold--
   * - attackscorethresholdtopic
     - --attackscorethresholdtopic--
   * - patternscorethreshold
     - --patternscorethreshold--
   * - patternscorethresholdtopic
     - --patternscorethresholdtopic--
   * - rtmsfoldername
     - --rtmsfoldername--
   * - rtmsmaxwindows
     - --rtmsmaxwindows--
   * - RTMS Output Github Link
     - `Output Data URL <--rtmsoutputurl-->`_

STEP 5: Entity Based Machine Learning : `tml-system-step-5-kafka-machine-learning-dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_5_kafka_machine_learning_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - preprocess_data_topic
     - iot-preprocess,iot-preprocess2
   * - ml_data_topic
     - ml-data
   * - modelruns
     - 100
   * - offset
     - -1
   * - islogistic
     - 1
   * - networktimeout
     - 600
   * - modelsearchtuner
     - 90
   * - processlogic
     - classification_name=failure_prob:Power_preprocessed_AnomProb=55,n
   * - dependentvariable
     - failure
   * - independentvariables
     - Power_preprocessed_AnomProb
   * - rollbackoffsets
     - 800
   * - topicid
     - -999
   * - consumefrom
     - 
   * - fullpathtotrainingdata
     - /rawdata/iotlogistic
   * - transformtype
     - 
   * - sendcoefto
     - 
   * - coeftoprocess
     - 
   * - coefsubtopicnames
     - 
   * - ML Output Github Link
     - `Output Data URL <https:\/\/github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/mldata/iotlogistic>`_

STEP 6: Entity Based Predictions: `tml-system-step-6-kafka-predictions-dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_6_kafka_predictions_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - preprocess_data_topic
     - iot-preprocess,iot-preprocess2
   * - ml_prediction_topic
     - iot-ml-prediction-results-output
   * - streamstojoin
     - Power_preprocessed_AnomProb
   * - inputdata
     - 
   * - consumefrom
     - ml-data
   * - offset
     - -1
   * - delay
     - 70
   * - usedeploy
     - 1
   * - networktimeout
     - 600
   * - maxrows
     - 800
   * - topicid
     - -999
   * - pathtoalgos
     - /rawdata/iotlogistic

STEP 7: Real-Time Visualization: `tml-system-step-7-kafka-visualization-dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_7_kafka_visualization_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^

.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - vipervizport
     - 9005
   * - topic
     - all-agents-responses,iot-preprocess,iot-ml-prediction-results-output
   * - dashboardhtml
     - iot-failure-machinelearning-agenticai.html
   * - secure
     - 1
   * - offset
     - -1
   * - append
     - 0
   * - chip
     - amd64
   * - rollbackoffset
     - 100

STEP 8: `tml_system_step_8_deploy_solution_to_docker_dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_8_deploy_solution_to_docker_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^
.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - Docker Container
     - virs20007/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-amd64 (https://hub.docker.com/r/virs20007/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-amd64)
   * - Docker Run Command
     - docker run -d --net=host -p 5050:5050 -p 4040:4040 -p 6060:6060 \
          --env TSS=0 \
          --env SOLUTIONNAME=tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb \
          --env SOLUTIONDAG=solution_preprocessing_ml_agenticai_dag-tml-multi-agenticai-iot-3f10 \
          --env GITUSERNAME=<Enter Github Username> \
          --env GITPASSWORD='<Enter Github Password>' \          
          --env GITREPOURL=<Enter Github Repo URL> \
          --env SOLUTIONEXTERNALPORT=5050 \
          -v /var/run/docker.sock:/var/run/docker.sock:z \
          -v /your_localmachine/foldername:/rawdata:z \
          --env CHIP=amd64 \
          --env SOLUTIONAIRFLOWPORT=4040 \
          --env SOLUTIONVIPERVIZPORT=6060 \
          --env DOCKERUSERNAME='' \
          --env EXTERNALPORT=42275 \
          --env KAFKABROKERHOST=127.0.0.1:9092 \                    
          --env KAFKACLOUDUSERNAME='<Enter API key>' \
          --env KAFKACLOUDPASSWORD='<Enter API secret>' \          
          --env SASLMECHANISM=PLAIN \                    
          --env VIPERVIZPORT=9005 \
          --env MQTTUSERNAME='' \
          --env MQTTPASSWORD='' \          
          --env AIRFLOWPORT=9000 \
          --env READTHEDOCS='<Enter Readthedocs token>' \ 
          virs20007/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-amd64

STEP 9: `tml_system_step_9_privategpt_qdrant_dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_9_privategpt_qdrant_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^
.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - PrivateGPT Container
     - --pgptcontainername--
   * - PrivateGPT Run Command
     - --privategptrun--
   * - Qdrant Container
     - --qdrantcontainer--
   * - Qdrant Run Command
     - --qdrantrun--
   * - Consumefrom
     - 
   * - pgpt_data_topic
     - --pgpt_data_topic--
   * - offset
     - -1
   * - rollbackoffset
     - 100
   * - topicid
     - -999
   * - enabletls
     - 1
   * - partition
     - --partition--
   * - prompt
     - --prompt--
   * - context
     - --context--
   * - jsonkeytogather
     - --jsonkeytogather--
   * - keyattribute
     - --keyattribute--
   * - keyprocesstype
     - --keyprocesstype--
   * - vectordbcollectionname
     - --vectordbcollectionname--
   * - concurrency
     - --concurrency--
   * - CUDA_VISIBLE_DEVICES
     - --cuda--
   * - pgpthost
     - --pgpthost--
   * - pgptport
     - --pgptport--
   * - hyperbatch
     - --hyperbatch--
   * - docfolder
     - --docfolder--
   * - docfolderingestinterval
     - --docfolderingestinterval--
   * - useidentifierinprompt
     - --useidentifierinprompt--
   * - searchterms
     - --searchterms--
   * - streamall
     - --streamall--
   * - temperature
     - --temperature--
   * - vectorsearchtype
     - --vectorsearchtype--
   * - llm
     - --llmmodel--
   * - embedding
     - --embedding--
   * - vectorsize
     - --vectorsize--
   * - contextwindowsize
     - --contextwindowsize--
   * - vectordimension
     - --vectordimension--
   * - mitrejson
     - --mitrejson--

STEP 9b: `tml_system_step_9b_agenticai_dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_9b_agenticai_dag-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^
.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - rollbackoffset
     - 15
   * - ollama-model
     - phi3:3.8b,phi3:3.8b,llama3.2:3b
   * - deletevectordbcount
     - 5
   * - vectordbpath
     - /rawdata/vectordb
   * - temperature
     - 0.1
   * - topicid
     - -999
   * - enabletls
     - 1
   * - partition
     - -1
   * - vectordbcollectionname
     - tml-llm-model-v2
   * - ollamacontainername
     - maadsdocker/tml-privategpt-with-gpu-nvidia-amd64-llama3-tools
   * - mainip
     - http://127.0.0.1
   * - mainport
     - 11434
   * - embedding
     - nomic-embed-text
   * - agenttopic
     - agent-responses
   * - agents_topic_prompt
     - 
        iot-preprocess<<-You are a precise data analysis assistant. Your task is to point out any anomalies or interesting insights that could help improve the performance and functioning of 
        IoT device.  The json data are from IOT devices.  the hp field shows the data that are processed for the process variable (pv), using the process types (pt) like: 
        avg or average, or trend analysis, or anomprob (i.e. anomaly probability) etc.  The device being processed is in the uid field of the json.
         here is the json data:
    
          <<data>>

         INSTRUCTIONS:
         1. Examine each number in the json array
         2. Provide a brief analysis of the results
         
         FORMAT YOUR RESPONSE:
         - Filtered results: [list the qualifying numbers with their "uid" fields]
         - Count of qualifying numbers: [number]
         - Analysis: [brief explanation of what the filter revealed]
         
         Be precise and concise in your response.->>
        iot-ml-prediction-results-output<<-You are a precise data analysis assistant. Your task is to filter and analyze numeric data based on specified criteria.

        TASK: Filter numbers from the given json array using the threshold: greater than 90

        Input JSON arrary:
 
             <<data>>

         INSTRUCTIONS:
         1. Examine each number in the json array
         2. Apply the filter condition: number > 90
         3. Return only numbers that meet the criteria with their "uid" fields
         4. If no numbers meet the criteria, explicitly state this
         5. Provide a brief analysis of the results
         
         FORMAT YOUR RESPONSE:
         - Filtered results: [list the qualifying numbers with their "uid" fields]
         - Count of qualifying numbers: [number]
         - Analysis: [brief explanation of what the filter revealed]
         
         Be precise and concise in your response.

   * - teamlead_topic
     - team-lead-responses
   * - teamleadprompt
     - 
         Analyze the dataset containing IoT device monitoring records managed by individual agents. 
         Review all data fields to determine whether there are any issues or major concerns requiring urgent attention.

         Focus on the following criteria:
         1. Each record contains a unique device identifier stored in the field "uid".
         2. Examine the failure probability for each device stored in the hp field.
         3. Categorize the probabilities as follows:
          - Low: 0% to 50%
          - Medium: 51% to 75%
          - High: 76% to 89%
          - Urgent: 90% to 100%

        Tasks:
        - Identify and highlight devices (by their "uid") that have **urgent failure probabilities** (≥ 90%).
        - For each flagged device, provide details and reasoning on why it may require immediate investigation.
        - Only include devices that meet the urgent threshold. Do not report on low, medium, or high categories unless relevant for context.
        - State clearly whether the identified issue is *urgent*.
        - Do not use or generate any code
   * - supervisor_topic
     - supervisor-responses
   * - supervisorprompt
     - 
        You are a team supervisor analyzing operational device data and recommending whether an alert email should be send.  
        You manage a send email expert and a average expert. 
        For send email, use send_email agent. 
        For average, use average agent.

       INSTRUCTIONS:
       1.Analyze the Team Lead assessment and determine the proper action:
       - If devices are marked urgent or failure probabilities exceed 90%, select "send_email".
       - If no urgent devices are found or probabilities remain below thresholds, then no action is needed.

   * - agenttoolfunctions
     - 
        send_email<<-send_email<<- You are an email-sending agent. Use smtp parameters to send emails when there is an anomaly in the data, make sure to
                     indicate the device name in the mainuid field. do not write a smtp script, actually send the email using the SMTP parameters
                     smtp_server=''
                     smtp_port=0
                     username=''
                     password=''
                     sender=''
                     recipient=''
                     subject=''
                     body=''->>
        average<<-average<<-You are an average agent.  Take average of the device failure probabilities.             

   * - agent_team_supervisor_topic
     - all-agents-responses
   * - concurrency
     - 2
   * - CUDA_VISIBLE_DEVICES
     - 0
   * - contextwindow
     - 4096
   * - localmodelsfolder
     - /mnt/c/maads/tml-airflow/rawdata/ollama

STEP 10: `tml_system_step_10_documentation_dag <https://github.com/virs20007/rasptssV2/tree/main/tml-airflow/dags/tml-solutions/tml-multi-agenticai-iot-3f10/tml_system_step_10_documentation_dag_tml-multi-agenticai-iot-3f10-tml-multi-agenticai-iot-3f10.py>`_
^^^^^^^^^^^^^^^^^^^^^
.. list-table::

   * - **User Parameter**
     - **Chosen Value**
   * - Solution Documentation URL
     - https://tml-multi-agenticai-iot-3f10-ml-agenticai-4ceb.readthedocs.io
