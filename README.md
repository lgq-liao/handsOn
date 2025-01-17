# JohnL-handsOn

My NLP hand's on
**Note: To update xxx with your actual path for the following commands**


---

## Task 1 

1. **clone the repository into your testing PC**

```bash
    $ git clone https://github.com/lgq-liao/JohnL-handsOn.git
```

2. **installation**
```bash
    $ cd xxx/JohnL-handsOn/
    $ chmod +x ./dependance_install.sh
    $ ./dependance_install.sh

```

---

# Task 2b

1. **launch the API**

```bash
    $ cd xxx/JohnL-handsOn/asr
    $ python asr_api.py
```

2. **test the API with your browser**

Launch your browser and enter the following link into your brower following by hit the **Enter** key : 

`http://localhost:8001/ping`

or click the link [ping](http://localhost:8001/ping)

---

# Task 2c

1. **launch the API**

```bash
    $ cd xxx/JohnL-handsOn/asr
    $ python asr_api.py
```

2. **test the API with my script**

```bash
    $ cd xxx/JohnL-handsOn/asr
    $ python cv-decode.py -a audio_sample/taken_clip.wav
```

Alternatively, you can test it to use your own method,eg by curl

---

# Task 2d

1. **copy the cv-valid-dev audios and csv file from common voice folder to JohnL-handsOn/asr/**

```bash
    $ cd xxx/JohnL-handsOn/asr

    $ cp -r xxx/cv-valid-dev ./cv-valid-dev/ 

```

2. **execute cv-decode.py to call API to update the cv-valid-dev.csv file**

```bash
    $ cd xxx/JohnL-handsOn/asr
    $ python cv-decode.py -af ./cv-valid-dev
```

---

# Task 2e

1. **Build the Docker image**

```bash
    $ cd xxx/JohnL-handsOn/asr
    $ docker build -t asr-api .
```

2. **Running the Docker container with GPU support**
   
   **NOTE**: For GPU access, need root privileges if not config docker in rootless mode as following command. 

   For more details about how to enable GPU support (CUDA) in your Docker container, pleaase refer to [How to run Docker](asr/How%20to%20run%20Docker.md)


```bash

   $ sudo docker run --gpus all --rm -p 8001:8001 asr-api # run with GPU
   $ docker run --rm -p 8001:8001 asr-api                 # run with CPU
```

---

# Task 3a

1. **copy the training dataset to the trainin path**
    ```bash
        $ cd xxx/JohnL-handsOn/asr-train
        
        $ cp -r xxx/cv-valid-train ./ 
        $ cp xxx/cv-valid-train.csv ./
    ```

2. **Run cv-train-2a.ipynb with notebook**
  Please refer to [**cv-train-2a.ipynb**](asr-train/cv-train-2a.ipynb) for the details 

---

# Task 3b

1. You may run **cv-train-2a.ipynb** to train the model with **truncate** dataset as following
   `df = df.head(1000) (remove this for full training)`

2. To avoid the training overhead within notebook, alternatively, you may train the model with pure Python with following commands
    ```bash
        $ cd xxx/JohnL-handsOn/asr-train
        $ python train-cv.py
    ```

    **Noted**: 
    - the train dataset will be processed and catched once only.
    - the fune-tuned model will be saved in folder **wav2vec2-large-960h-cv**

---
# Task 3c

1. **copy cv-valid-test dataset and the csv**

```bash
    $ cd xxx/JohnL-handsOn/asr-train
    
    $ cp -r xxx/cv-valid-test ./ 
    $ cp xxx/cv-valid-test.csv ./cv-valid-test/
```
2. **Launch asr_aip & load the fune-tuning model with following commands**

```bash
    # the following input parameter -mi
    # 1 is for choosing  'facebook/wav2vec2-large-960h', 
    # 2 for the fune-tuned model: 'wav2vec2-large-960h-cv'
    $ python ../asr/asr_api.py -mi 2 
  
```
3. **launch cv-decode.py to update cv-valid-test.csv with generated_text column**
```bash
    # -csv to specify the input csv file to be processed
    # -af to specify the input audio folder to be processed
    $ python ../asr/cv-decode.py -csv ./cv-valid-test/cv-valid-test.csv -af ./cv-valid-test   
```

4. **Performance evaluation in the last section of `cv-train-2a.ipynb`**

    Execute the cell after the cell of **Performance evaluation for Fune-tuned model on dataset cv-valid-test.csv** 
   
---

# Task 4

#### Performance(WER) comparison Orignal model `facebook/wav2vec2-large-960h` Vs `Fine-tuned` model:

1. The WER comparison is as follows:
   - `Orignal model`:
      Overall WER: **10.82%**
   - `Fine-tuned model`:
      Overall WER: **7.32%**

2. To run the following script to get the WER above
```bash
   $ cd xxx/JohnL-handsOn/asr-train

   $python task4_performance_compare.py  -csv "../../common_voice/cv-valid-dev.csv" -audio "../../common_voice/cv-valid-dev" 
```
    **Noted**, change the `-csv` and `-audio` path to your actual one

3. Please refer to the [training-report.pdf](./training-report.pdf) for the details

---

# Task 5a
  Please refer to the [cv-hotword-5a.ipynb](./hotword-detection/cv-hotword-5a.ipynb) for the details

---

# Task 5b

1.  Please refer to the [cv-hotword-similarity-5b.ipynb](./hotword-detection/cv-hotword-similarity-5b.ipynb) for the details

2. To change the following threshold for similarity if desired
```pthon
    THESHOLD=0.4  # Set threshold for similarity
```
3. Turn on the following debug flag to use small dataset for debugging  

```pthon
    DEBUG = False
```
4.  Alternatively, you may run `cv-hotword-similarity-5b.py` to create the csv file

---
# Task 6

Please refer to the [essay-ssl.pdf](./essay-ssl.pdf) for the details
