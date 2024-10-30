# Use VSCode to a running compute node
reference: https://github.com/NVIDIA/pyxis/issues/149
Thanks for the help from @[itzsimpl](https://github.com/itzsimpl)

Prerequisite:
- VSCode
- VSCode plugin: Remote Development - Pack
- vscode tunnel on server: https://code.visualstudio.com/docs/remote/tunnels
- 

## prerequisite:
1. vscode cli on server, follow instruction: https://code.visualstudio.com/docs/remote/tunnels
2. setup vscode tunnel on server by: `./code tunnel`
3. login by github
4. now you should be able to connect to login node by vscode tunnel
5. quit

## Steps:
1. open terminal and ssh to superpod
2. srun request node, use pyxis run with container
   use NGC pytorch 23.10 container example: `srun -N1 -G1 -c16 --mem=128G --time=12:00:00 --container-name=pyg --container-image=nvcr.io#nvidia/pytorch:23.10-py3 --container-mounts=./code:/code[,SRC:DST] --pty /code tunnel --accept-server-license-terms`

3. open vscode and connect to the running node by tunnel
done

