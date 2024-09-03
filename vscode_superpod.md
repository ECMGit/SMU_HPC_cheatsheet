# Use VSCode to a running compute node

Prerequisite:
- VSCode
- VSCode plugin: Remote Development - Pack

## Steps:
1. ssh to superpod
2. srun request node, use pyxis run with container
   use NGC pyg 24.07 container example: `srun -N1 -G8 -c128 --mem=1910G --time=12:00:00 --container-name pyg --container-image $WORK/nvidia+pyg+24.07-py3.sqsh --container-mounts=$WORK:/workspace/work --pty $SHELL`

3. `hostname -I` find ip address you can use(start with 10.xxx.xxx.xx)
4. go to vscode --> remote explore
5. `ssh <username>@<ip>`

The Terminal will not use session in your running container, you need to:
1. `squeue` to check your running jobid
2. `srun --overlap --jobid <jobid> --container-name <container name> --pty bash`
3. run jupyter lab: `jupyter lab --ip=0.0.0.0 --no-browser`
   

done
