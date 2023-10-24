# Slurm command line
## command line cheat sheet
check current slurm running job: `squeue -u $USER`  
cancel job: `scancel <JOBID>`  
ssh tunnel in background: `ssh -N -f -L localhost:8889:localhost:8889 username@serverIP`  
-N: no command will be execute in remote  
-f: run ssh tunnel in background   
-L: assign port forwarding <local port#>:<remote port#>

list all hidden ssh tunnel:  `lsof -i -n | egrep '\<ssh\>'`

request a v100 node: `srun -p v100x8 --gres=gpu:1 --mem=64G --time=8:00:00 -pty $SHELL`


## sbatch script
### v100 run 1 GPU script
**v100gpu1**
```
#!/bin/bash
#SBATCH --job-name=jupyterTest
#SBATCH -p v100x8
#SBATCH --gres=gpu:1
#SBATCH --mem=32GB
#SBATCH --time=8:00:00

#Load necessary module
module load spack conda
#Activate conda env
source activate jupyter_37
#Go to the folder you wanna run jupyter in
cd $HOME

#Pick a random or predefined port
#port=$(shuf -i 6000-9999 -n 1)
port=8889
#Forward the picked port to the m2 on the same port. Here log-x is set to be the m2 login node.
/usr/bin/ssh -N -f -R $port:localhost:$port login01
#/usr/bin/ssh -N -f -R $port:localhost:$port login02

#Start the notebook
jupyter notebook --no-browser --port $port
```

### superpod run 1 gpu script
**A100gpu12cpu**
```
#!/bin/bash
#SBATCH --job-name=jupyterTest    # job name to display in squeue
#SBATCH -t 180                    # maximum runtime in minutes
#SBATCH -c 12                     # request 12 cpus    
#SBATCH -G 1                      # request 1 gpu a100
#SBATCH -D /work/users/<username> # link to your folder
#SBATCH --mem=32gb                # request 32gb memory
#SBATCH --mail-user junhaos@smu.edu  # request to email to your emailID
#SBATCH --mail-type=end           # request to mail when the model **end**

#Load necessary module
module load spack conda
#Activate conda env
conda activate ml2023
#Go to the folder you wanna run jupyter in
cd $HOME

#Pick a random or predefined port
#port=$(shuf -i 6000-9999 -n 1)
port=8889

#Reverse Forward the picked port to the superpod on the same port. 
#Here log-x is set to be the superpod login node.
/usr/bin/ssh -N -f -R $port:localhost:$port slogin-01
#/usr/bin/ssh -N -f -R $port:localhost:$port slogin-02

#Start the notebook
jupyter notebook --no-browser --port $port
```
