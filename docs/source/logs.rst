Latest Logs From Latest Build
==============================

Generated On: 2026-03-30 21:38:21 UTC

These are the latest logs generated from your latest build.  

.. tip:: 
   Complete logs from all builds can be found `here on GitHub <https://github.com/virs20007/rasptssV2/blob/main/tml-airflow/logs/logs.txt>`_

.. code-block:: 
  :linenos:

  [INFO 2026-03-30_21:31:25] STEP 1: completed - TML system parameters successfully gathered

  [INFO 2026-03-30_21:31:29] STEP 2: Create topics started

  [INFO 2026-03-30_21:31:47] STEP 2: Completed

  [INFO 2026-03-30_21:31:57] STEP 3: producing data started

  [INFO 2026-03-30_21:32:01] STEP 4: Preprocessing started

  [INFO 2026-03-30_21:32:03] STEP 4: Preprocessing started

  [INFO 2026-03-30_21:32:05] STEP 3: reading local file..successfully

  [INFO 2026-03-30_21:32:07] STEP 5: Machine learning started

  [INFO 2026-03-30_21:32:14] STEP 6: Predictions started

  [INFO 2026-03-30_21:32:17] STEP 7: Visualization started

  [INFO 2026-03-30_21:32:22] STEP 7: /Viperviz/viperviz-linux-amd64 0.0.0.0 9005

  [INFO 2026-03-30_21:32:29] STEP 9b: Starting ollama server

  [INFO 2026-03-30_21:32:45] STEP 9b: Ollama container.  Here is the run command: docker run -d -p 11434:11434 --net=host --gpus all -v /var/run/docker.sock:/var/run/docker.sock:z -v /mnt/c/maads/tml-airflow/rawdata/ollama:/root/.ollama:z --env OLLAMA_LOAD_TIMEOUT=30m0s --env PORT=11434 --env TSS=1 --env GPU=1 --env COLLECTION=tml-llm-model-v2 --env WEB_CONCURRENCY=2 --env CUDA_VISIBLE_DEVICES=0 --env TOKENIZERS_PARALLELISM=false --env temperature=0.1 --env LLAMAMODEL="phi3:3.8b && phi3:3.8b && llama3.2:3b" --env mainembedding="nomic-embed-text" --env OLLAMASERVERPORT="http://127.0.0.1:11434" maadsdocker/tml-privategpt-with-gpu-nvidia-amd64-llama3-tools, v=0

  [INFO 2026-03-30_21:32:45] STEP 9b: Success starting Agentic AI.  Here is the run command: docker run -d -p 11434:11434 --net=host --gpus all -v /var/run/docker.sock:/var/run/docker.sock:z -v /mnt/c/maads/tml-airflow/rawdata/ollama:/root/.ollama:z --env OLLAMA_LOAD_TIMEOUT=30m0s --env PORT=11434 --env TSS=1 --env GPU=1 --env COLLECTION=tml-llm-model-v2 --env WEB_CONCURRENCY=2 --env CUDA_VISIBLE_DEVICES=0 --env TOKENIZERS_PARALLELISM=false --env temperature=0.1 --env LLAMAMODEL="phi3:3.8b && phi3:3.8b && llama3.2:3b" --env mainembedding="nomic-embed-text" --env OLLAMASERVERPORT="http://127.0.0.1:11434" maadsdocker/tml-privategpt-with-gpu-nvidia-amd64-llama3-tools

  [INFO 2026-03-30_21:32:49] STEP 8: Starting docker push for: virs20007/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-amd64

  [INFO 2026-03-30_21:37:58] STEP 8: Docker Container created and optimized.  Will push it now.  Here is the commit command: docker commit ede0d2e91954 virs20007/tml-multi-agenticai-iot-3f10-ml_agenticai-4ceb-amd64 - message=0

  [INFO 2026-03-30_21:38:21] STEP 10: Started to build the documentation

  [INFO 2026-03-30_21:38:22] STEP 10: Documentation successfully built on GitHub..Readthedocs build in process and should complete in few seconds


