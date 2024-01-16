---
layout: post
title: Running Jupyter on ACG with SLURM
date: 2024-01-16
category: lectures
---
{% capture pageassetpath %}{{site.baseurl}}/assets/0000-00-05-acg-jupyter{% endcapture %}
<style type="text/css">
.width500 {
    width: 400pt
}
</style>
We will learn how to run Jupyter notebook on ACG Katahdin and access it on your
laptop by port forwarding. Please use this method only if your project has
outscaled your laptop and colab resources.

# Table of Contents
{:.no_toc}

* Seed list
{:toc}


### Setup VPN

Instructions here: [vpn.maine.edu](https://vpn.maine.edu/){: target="_blank"}

Click on "Remote Access VPN" and then follow instructions for your operating
system.

### SSH to Katahdin

[Instructions from here](https://acg.maine.edu/hpc/connecting-to-katahdin){:
target="_blank"}

Next, `ssh` to `katahdin.acg.maine.edu`. Replace `vdhiman` with your own
username that you got in the email from Steve. For different operating
systems, we have different instructions. For Windows, we will use PowerShell, not Putty.

1. [For Mac and Linux](#for-mac-and-linux)
1. [For Widows PowerShell](#for-windows-powershell)


#### For Mac and Linux

{:.text}
    vdhiman@office-desktop:~$ ssh vdhiman@katahdin.acg.maine.edu
    Last login: Sat Jan 21 13:37:12 2023 from jx3cth3.um.maine.edu
    vdhiman@katahdin:~$
    

#### For Windows PowerShell

Instructions from here: [Tutorials ssh](https://learn.microsoft.com/en-us/windows/terminal/tutorials/ssh){: target="_blank"}

By default, the OpenSSH client will be located in the directory: `C:\Windows\System32\OpenSSH`. You can also check that it is installed in Windows Settings > Apps > Optional features, then search for "OpenSSH" in your installed features.

![]({{pageassetpath}}/ssh-optionalfeatures.png){: .width500}

### SLURM

Running Jupyter server via [SLURM](https://slurm.schedmd.com/sbatch.html) enables you to run your Jupyter server on a SLURM node with more resources; sometimes allowing you to use GPU. It is a better practice to run heavy code via SLURM, because login nodes are meant for login only. This documentation specifically covers this process.

<style rel="text/css">
div.post h3.h4reset { 
    counter-reset: h4counter
}
div.post h4:before {
    counter-increment: h4counter;
    content: counter(h4counter) ".\0000a0\0000a0";
}
</style>

[This guide is adapted from nero-docs.stanford.edu](https://nero-docs.stanford.edu/jupyter-slurm.html)

With Katahdin ACG, you'll need to submit this as a SLURM job. That way,
your jupyter lab server gets placed on a SLURM node with enough
resources to host it. To summarize: We are creating a slurm job that
runs jupyterlab on a SLURM node, for up to 2 days (max is 7). Once
running, we are going to connect to the jupyterlab instance with SSH
port forwarding from our local laptop. A tunnel must be created as you
cannot directly SSH to SLURM nodes on Katahdin.

{:.h4reset}
### First create a SLURM sbatch file

Replace $USER with your own ACG username.
Use Terminal On Your Laptop:

#### SSH to Katahdin ACG

{:.text}
    ssh $USER@katahdin.acg.maine.edu

#### Create your sbatch file. You can use your text editor of choice.

    vi jupyterLab.sh

Paste the following text into your sbatch script, and save the file.

{:.bash}
    #!/bin/bash
    #SBATCH --job-name=jupyter
    #SBATCH --partition=grtx # You can pick from https://acg.maine.edu/hpc#h.b5slztm4yz12
    #SBATCH --gres=gpu:1 # not clear if this is obeyed https://slurm.schedmd.com/gres.html
    #SBATCH --time=2-00:00:00
    #SBATCH --mem=5GB
    #SBATCH --output=/home/%u/logs-jupyter-%x-%j.log

    module load nv/pytorch # Load pytorch singularity image
    singularity exec --nv $PYTORCH_CONT jupyter notebook --ip=0.0.0.0 # start jupyter notebook


**Replace the `$USER` part
of your
`#SBATCH --output=home/$USER/jupyter.log` with your ACG Username. Or, provide an alternate path for
your log output.**

This tells SLURM to launch a Jupyter Lab server on the node with your
requested resources.

Now we need to send this as a job to SLURM.


#### Submit this sbatch to SLURM:

    vdhiman@katahdin:~$ sbatch jupyterLab.sh 
    Submitted batch job 1005424

#### Now, you can check that your job is running:

    vdhiman@katahdin:~$ squeue -u $USER
                 JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
               1005424      grtx  jupyter  vdhiman PD       0:00      1 (Priority)

Note the ST (Status) column says PD (Pending). After a suitable node is found,
it should change to ST=R (Running). This might take a while if the server is
busy.

    vdhiman@katahdin:~$ squeue -u vdhiman
                 JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
               1005424      grtx  jupyter  vdhiman  R       0:01      1 grtx-1

Once it is running you can continue...

 Check the
log output to find out the HOSTNAME we need to use to create an SSH tunnel:

#### Check log file for Jupyter URL

You can use the `tail -f` command to follow the last 10 lines of the log file.

    tail -f ~/jupyter.log

The log file will output something like this:

{:.text}
    vdhiman@katahdin:~$ tail jupyter.log
    [I 19:09:29.687 NotebookApp]  or http://127.0.0.1:8888/?token=8a5d8e1laskdjfl1askjdfl1ksjadfl1kjsadlfsjadc3386
    [I 19:09:30.097 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [W 19:09:30.302 NotebookApp] No web browser found: could not locate runnable browser.
    [C 19:09:30.566 NotebookApp] 
        
        To access the notebook, open this file in a browser:
            file:///home/vdhiman/.local/share/jupyter/runtime/nbserver-12238-open.html
        Or copy and paste one of these URLs:
            http://grtx-1.cluster:8888/?token=8a5d8e1laskdjfl1askjdfl1ksjadfl1kjsadlfsjadc3386
         or http://127.0.0.1:8888/?token=8a5d8e1laskdjfl1askjdfl1ksjadfl1kjsadlfsjadc3386
         
Note the file after "open this file in a browser". Remove the `file://` search
the file for link. For example,
{:.text}
    vdhiman@katahdin:~$ grep window.location.href /home/vdhiman/.local/share/jupyter/runtime/nbserver-12238-open.html 
        window.location.href = "http://0.0.0.0:8888/tree?token=9946b79c2cae1c4744aa90ce58e4133add9d58d0ded77161";
    vdhiman@katahdin:~$

This will give you a link. Note the port number. For me that is 8888. Let's
call this REMOTE_PORT=8888.

First check if the status of the job under ST is R for running. Then look for string under NODELIST(REASON). This gives me the HOSTNAME. For me the HOSTNAME is grtx-1.

**Note the http://HOSTNAME:REMOTE_PORT/token=TOKEN in the first part, you'll need that info  to setup a port-forwarding connection. For me, the HOSTNAME=grtx-1.cluster, REMOTE_PORT=8888, and TOKEN=8a5d8e1laskdjfl1askjdfl1ksjadfl1kjsadlfsjadc3386**

**If the output simply says http://hostname:8888/ then you have to use the
ouptut of squeue -u $USER under the nodelist column**

We need to find the host where the job got scheduled. We can use squeue
command to to that.
{:.text}
    vdhiman@katahdin:~$ squeue -u $USER
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           1086781      grtx  jupyter  vdhiman  R    3:14:58      1 grtx-1

#### Create an SSH tunnel

We cannot access this yet. If grtx-1 was not firewalled, anyone (without your
password) could have
acessed this by replacing `localhost` with `grtx-1.acg.maine.edu`. However,
`grtx-1.acg.maine.edu` is firewalled and we cannot access arbitrary [ports](http://www.tcpipguide.com/free/t_TCPIPApplicationAssignmentsandServerPortNumberRang.htm){: target="_blank"} on
katahdin. Luckily, ssh can help us access with [port-forwarding](https://www.ibm.com/docs/en/zos/2.4.0?topic=examples-openssh-tcp-port-forwarding){: target="_blank"}.

##### TCP/UDP ports

Ports in computer and electrical engineering stands for an input-output
interface. When we talk about TCP/UDP ports, the port numbers are software
ports that are managed by the operating sytem. However, you can still
"imagine" them as connections to the outside world. For example, the default ports for
HTTP protocol is 80, HTTPS is 443, for SSH is 22. What does this mean? This
means that you can add the correct port number to any URL and it will still
work. For example, you can access umaine.edu using [https://maine.edu](https://maine.edu) or using [https://maine.edu:443](https://maine.edu:443){: target="_blank"}. You can access katahdin.acg.maine.edu through ssh by specifying 22 as the port number. This also means that the server side processes "listen" to the assigned port numbers on the server. The "ssh daemon" listens to the port 22, "https server daemon" listens to the port 443 and so on.

You can learn more about this in the COS 440:Computer Networking class or
[explore on your own](http://www.tcpipguide.com/free/t_toc.htm){:
target="_blank"}.

##### Port forwarding

We want the communication between our browser and Jupyter notebook server to
tunnel through the ssh connection.
The information flow for the port forwarding is visualized here:

![]({{pageassetpath}}/ssh-tunneling.svg){:.width500}

Then on your laptop, open a new Terminal Window and create an SSH
tunnel using `ssh -L 8888:HOSTNAME:REMOTE_PORT $USER@katahdin.acg.maine.edu`.
For my output the command is:

    ssh -L 8888:gtrx-1.cluster:8888 $USER@katahdin.acg.maine.edu

**Important: You must replace the HOSTNAME:REMOTE_PORT (gtrx-1.cluster:8888) in the command
below with your node name address from previous step**

Note that whatever your log output says for hostname you will need to use in
the command above. DO NOT just copy and paste the example, you have to
replace the HOSTNAME:REMOTE_PORT (the gtrx-1.cluster:8888 part) to be the one your log
output specifies.


#### On your laptop open a browser window and you can then browse to:

    http://127.0.0.1:8888/?token=8a5d8e1laskdjfl1askjdfl1ksjadfl1kjsadlfsjadc3386

**Important: Replace the "?token=TOKEN" part of the URL with your token from
log file**

*Note that this is copied from the output log file where it defines your
Jupyter address and TOKEN. You MUST copy the token from the log out put,
and cannot just use the example above. It may take up to 10 minutes for
the "jupyter.log" output to show you the text with your token.*

For the remainder of your job run, the hostname and port will stay the same.
If you close your laptop you will need to recreate an SSH Tunnel - you
can reuse the "ssh -L8888" command above. If your job ends on Katahdin
ACG, you need to resubmit your slurm job, and then modify your SSH
Tunnel command with the new hostname. Jobs last for a maximum of 7
days.

