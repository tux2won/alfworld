# ALFWorld

[<b>Aligning Text and Embodied Environments for Interactive Learning</b>](https://arxiv.org/abs/1912.01734)  
[Mohit Shridhar](https://mohitshridhar.com/), [Xingdi (Eric) Yuan](https://xingdi-eric-yuan.github.io/), [Marc-Alexandre Côté](https://www.microsoft.com/en-us/research/people/macote/),   
[Yonatan Bisk](https://yonatanbisk.com/), [Adam Trischler](https://www.microsoft.com/en-us/research/people/adtrisch/), [Matthew Hausknecht](https://mhauskn.github.io/)

**ALFWorld** contains interactive TextWorld environments (Côté et. al) that parallel embodied worlds in the ALFRED dataset (Shridhar et. al). The aligned environments allow agents to reason and learn high-level policies in an abstract space before solving embodied tasks through low-level actuation.  

For the latest updates, see: [**alfworld.github.io**](https://alfworld.github.io)

<p align="left">
    <img width="500" alt="portfolio_view" src="media/alfworld_teaser.png">
</p>

## Quickstart

Clone repo:
```bash
$ git clone https://github.com/alfworld/alfworld.git alfworld
$ export ALFRED_ROOT=$(pwd)/alfworld
```

Install requirements:
```bash
$ virtualenv -p $(which python3) --system-site-packages alfworld_env # or whichever package manager you prefer
$ source alfworld_env/bin/activate

$ cd $ALFRED_ROOT
$ pip install --upgrade pip
$ pip install -r requirements.txt
```

Download PDDL & Game Files and pre-trained MaskRCNN detector:
```bash
$ sh data/download_data.sh
```

Train models:
```bash
$ cd $ALFRED_ROOT/agents
$ python dagger/train_dagger.py config/base_config.yaml
```

## More Info 

- [**Dataset**](data/): Downloading full dataset, Folder structure, JSON structure.

## Prerequisites

- Python 3
- PyTorch 1.1.0
- Torchvision 0.3.0
- AI2THOR 2.1.0

See [requirements.txt](requirements.txt) for all prerequisites

## Hardware 

Tested on:
- **GPU** - GTX 1080 Ti (12GB)
- **CPU** - Intel Xeon (Quad Core)
- **RAM** - 16GB
- **OS** - Ubuntu 16.04


## Docker Setup

Install [Docker](https://docs.docker.com/engine/install/ubuntu/) and [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker#ubuntu-160418042004-debian-jessiestretchbuster). 

Modify [docker_build.py](scripts/docker_build.py) and [docker_run.py](scripts/docker_run.py) to your needs.

#### Build 

Build the image:

```bash
$ python docker/docker_build.py 
```

#### Run (Local)

For local machines:

```bash
$ python docker/docker_run.py
 
  source ~/alfworld_env/bin/activate
  cd $ALFRED_ROOT
```

#### Run (Headless)

For headless VMs and Cloud-Instances:

```bash
$ python docker/docker_run.py --headless 

  # inside docker
  tmux new -s startx  # start a new tmux session

  # start nvidia-xconfig (might have to run this twice)
  sudo nvidia-xconfig -a --use-display-device=None --virtual=1280x1024
  sudo nvidia-xconfig -a --use-display-device=None --virtual=1280x1024

  # start X server on DISPLAY 0
  sudo python ~/alfworld/docker/startx.py 0  # if this throws errors e.g "(EE) Server terminated with error (1)" or "(EE) already running ..." try a display > 0

  # detach from tmux shell
  # Ctrl+b then d

  # source env
  source ~/alfworld_env/bin/activate
  
  # set DISPLAY variable to match X server
  export DISPLAY=:0

  # check THOR
  cd $ALFRED_ROOT
  python docker/check_thor.py

  ###############
  ## (300, 300, 3)
  ## Everything works!!!
```

You might have to modify `X_DISPLAY` in [gen/constants.py](gen/constants.py) depending on which display you use.

## Cloud Instance

ALFWorld can be setup on headless machines like AWS or GoogleCloud instances. 
The main requirement is that you have access to a GPU machine that supports OpenGL rendering. Run the [startx.py](scripts/startx.py) script like above
to examine the GPU devices on the host, generate a xorg.conf file, and then start X. You should be able to run AI2THOR normally for evaluation purposes. 
By default, the `:0.0` display will be used, but if you are running on a machine with more than one GPU, you can address 
these by modifying the screen component of the display. So `:0.0` refers to the first device, `:0.1` the second and so on.

Also, checkout this guide: [Setting up THOR on Google Cloud](https://medium.com/@etendue2013/how-to-run-ai2-thor-simulation-fast-with-google-cloud-platform-gcp-c9fcde213a4a)

## Citation

If you find the dataset or code useful, please cite:

```
@inproceedings{ALFWorld20,
               title ={{ALFWorld: Aligning Text and Embodied
               Environments for Interactive Learning}},
               author={Mohit Shridhar and Xingdi Yuan and
               Marc-Alexandre C\^ot\'e and Yonatan Bisk and
               Adam Trischler and Matthew Hausknecht},
               booktitle = {arXiv},
               year = {2020},
               url = {https://arxiv.org/abs/2010.03768}}
```

## License

MIT License

## Contact

Questions or issues? Contact [Mohit Shridhar](https://mohitshridhar.com)