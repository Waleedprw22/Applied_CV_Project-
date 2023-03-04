# Applied_CV_Project-

I will be uploading files here that I have changed and manipulated. However, majority of the files come from this github: https://github.com/ultralytics/yolov5. 

Make sure to follow each of these steps in chronological order.
```
git clone https://github.com/ultralytics/yolov5
```
 ****Important to note****: After cloning, I would suggest downloading the files in this repo to replace the ones in the repo above to look into my project/work.
 ```
pip install -qr requirements.txt  # install dependencies (ignore errors)
```
```
pip install -q roboflow
```

```
import torch
from IPython.display import Image, clear_output  # to display images
from utils.downloads import attempt_download  # to download models/datasets
```



```
from roboflow import Roboflow
rf = Roboflow(model_format="yolov5", notebook="roboflow-yolov5")
```
You should then get a link to fill the blanks in the following:
```
#after following the link above, recieve python code with these fields filled in
#from roboflow import Roboflow
#rf = Roboflow(api_key="YOUR API KEY HERE")
#project = rf.workspace().project("YOUR PROJECT")
#dataset = project.version("YOUR VERSION").download("yolov5")
```
Using my data set, you will get the following:
```
from roboflow import Roboflow
rf = Roboflow(api_key="cBniCRCIzhJgOEwoyLSl")
project = rf.workspace().project("car-detection-onugp")
dataset = project.version(1).download("yolov5")
```

Then enter the following: 
```
%cat {dataset.location}/data.yaml

from IPython.core.magic import register_line_cell_magic

@register_line_cell_magic
def writetemplate(line, cell):
    with open(line, 'w') as f:
        f.write(cell.format(**globals()))
```        
        
To run the hyperparameter evolution and find optimized parameters, run:
```
python train.py --img 200 --batch 16 --epochs 10 --data {dataset.location}/data.yaml --weights yolov5s.pt --cache --evolve 5 --name yolov5s_results  --cache
 **You can change the number of evolutions above**
```
If you want to run the actual model with its weights, run the command below
```
python train.py --img 200 --batch 16 --epochs 50 --data {dataset.location}/data.yaml --weights 'yolov5s.pt' --name yolov5s_results  --cache
```
Then type:
```
from utils.plots import plot_results  # plot results.txt as results.png
Image(filename='/content/yolov5/runs/train/yolov5s_results/results.png', width=1000)  # view results.png
```
```
Image(filename='/content/yolov5/runs/evolve/yolov5s_results/evolve.png', width=1000)  # view evolve.png. This will show graphs of all 30 parameters for each evolution against fitness. Note that the evolution with the highest fitness value will be selected for each parameter
```
To run inference with trained weights, run the command below:
```
python detect.py --weights runs/train/yolov5n_results/weights/best.pt --img 416 --conf 0.4 --source ./Car-detection-1/test/images
```

The commands below are to display the results:
```
import glob
from IPython.display import Image, display

for imageName in glob.glob('/content/yolov5/runs/detect/exp2/*.jpg'): #assuming JPG
    display(Image(filename=imageName))
    print("\n")
```

In case you run into trouble or would like to go through a tutorial to supplement the instructions here, check out https://blog.roboflow.com/how-to-train-yolov5-on-a-custom-dataset/
