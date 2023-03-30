# Connect to your Jupyter Server on SuperPod/M2
Author: Junhao Shen  
email: junhaos@smu.edu

OS:MacOS/Linux
## Step1: login to SuperPod/M2 and port forwarding from your local machine to SuperPod/M2 login node
let's use port 8889,  
SuperPod: `ssh -C -L 8889:localhost:8889 <username>@superpod.smu.edu`  
M2:
`ssh -C -L 8889:localhost:8889 <username>@m2.smu.edu`
once you login, remember the login node name, you can find in your terminal: <yourusername>@<login_node>

## Step2: request a compute node
SuperPod: `srun -N 1 -G 1 -c 10 --mem=128G --time=2:00:00 --pty $SHELL`
M2(p100):
`srun -p gpgpu-1 --gres=gpu:1 --mem=250G --time=2:00:00 --pty $SHELL`

## Step3: Load Module
e.g. `module load spack conda`

## Step4: reverse Port forwarding to your login node
pick a port(let's use 8889): `port=8889`  
reverse port forwarding: `ssh -N -f -R $port:localhost:$port <login_node>`

## Step5: launch your jupyter server:
Jupyter Notebook: `jupyter notebook --no-browser --port $port`  
Jupyter lab: `jupyter lab --no-browser --port $port`

## Step6: Copy the URL to your browser on your local machine  

done

---

