# mlops_tutorial3

## steps
* create a new repository on GitHub.com named `mlops_tutorial3`
* `git clone  https://github.com/tagny/mlops_tutorial3.git`
* copy files `get_data.py, data_processing.py, train.py`, and `requirements.txt` from  https://github.com/elleobrien/farmer.git to the local dir `mlops_tutorial3`
* `cd mlops_tutorial3`
* `pip install -r requirements.txt` 
* `dvc init`
* Declare the pipeline stages (3 stages):
  * data could change: `dvc run -n get_data -d get_data.py -o data_raw.csv --no-exec python get_data.py`  
    * `-n`: stage name 
	* `-d`: dependencies
	* `-o`: the output
	* `--no-exec`: I'm just declaring the command to be run, I don't want to run it right now
	* this command creates a dvc.yaml file representing what we have just entered in cmd line 
	  * it can be also created in a text editor
  * data processing could change: add this stage to the dvc.yaml file of the pipeline
  ```
  process:
    cmd: python process_data.py
    deps:
    - process_data.py
    - data_raw.csv
    outs:
    - data_processed.csv
  ```
  * modeling approach could change: add this stage to the dvc.yaml file of the pipeline
  ```
  train:
    cmd: python train.py
    deps:
    - train.py
    - data_processed.csv
    outs:
    - by_region.png
    metrics:
	- metrics.json:
        cache: false              ## to avoid depending on local cache used by dvc
  ```
* Run the pipeline and (re)generate all the outputs: `dvc repro`
* Take a snapshot of where we are in the project:
  * `git add .`
  * `git commit -m "setup dvc pipelines"` 
## References

* https://github.com/elleobrien/farmer
* https://www.youtube.com/watch?v=xPncjKH6SPk&amp;list=PL7WG7YrwYcnDBDuCkFbcyjnZQrdskFsBz&amp;index=4&amp;ab_channel=DVCorg
