---
title: Running Jupyter on ACG Katahdin
layout: post
category: lectures
date: 2023-01-19
---
{% capture pageassetpath %}{{site.baseurl}}/{{ page.path | replace: "_posts", "assets" | replace: ".md", "" }}{% endcapture %}
<style type="text/css">
.width500 {
    width: 400pt
}
</style>
We will learn how to run Jupyter notebook on ACG Katahdin and access it on your
laptop by port forwarding.

# Table of Contents
{:.no_toc}

* Seed list
{:toc}


# Setup VPN

Instructions here: [vpn.maine.edu](https://vpn.maine.edu/){: target="_blank"}

Click on "Remote Access VPN" and then follow instructions for your operating
system.

# SSH to Katahdin

[Instructions from here](https://acg.maine.edu/hpc/connecting-to-katahdin){:
target="_blank"}

Next, `ssh` to `katahdin.acg.maine.edu`. Replace `vdhiman` with your own
username that you got in the email from Steve. For different operating
systems, we have different instructions. For Windows, we have two options; use
PowerShell or Putty.

1. [For Mac and Linux](#for-mac-and-linux)
1. [For Widows PowerShell](#for-windows-powershell)
1. [For Widows Putty](#for-windows-putty)


## For Mac and Linux

{:.text}
    vdhiman@office-desktop:~$ ssh vdhiman@katahdin.acg.maine.edu
    Last login: Sat Jan 21 13:37:12 2023 from jx3cth3.um.maine.edu
    vdhiman@katahdin:~$
    

## For Windows PowerShell

Instructions from here: [Tutorials ssh](https://learn.microsoft.com/en-us/windows/terminal/tutorials/ssh){: target="_blank"}

By default, the OpenSSH client will be located in the directory: `C:\Windows\System32\OpenSSH`. You can also check that it is installed in Windows Settings > Apps > Optional features, then search for "OpenSSH" in your installed features.

![]({{pageassetpath}}/ssh-optionalfeatures.png){: .width500}


{:.text}
    PS C:\Users\vdhiman> ssh vdhiman@katahdin.acg.maine.edu

![]({{pageassetpath}}/powershell.png){: .width500}

## For Windows Putty

[Download from here](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) and [Full documentation here](https://the.earth.li/~sgtatham/putty/0.78/htmldoc/){: target="_blank"}

![]({{pageassetpath}}/putty.png){:.width500}

# Load anaconda3 module

[Conda](https://docs.conda.io/en/latest/){: target="_blank"} is package and environment management system for Python and other launchages.

Available libraries on acg servers are managed through [module](https://lmod.readthedocs.io/en/latest/010_user.html){: target="_blank"}. Some modules are loaded by default. You can see what modules are already loaded by using `module list`.
    
{:.text}
    vdhiman@katahdin:~$ module list

    Currently Loaded Modules:
      1) autotools   2) prun/1.3   3) gnu8/8.3.0   4) mvapich2/2.3.2   5) ohpc



    vdhiman@katahdin:~$

You can list all the available modules through `module avail`

{:.text}
    vdhiman@katahdin:~$ module avail                                                    
    --------------------- /opt/ohpc/pub/moduledeps/gnu8-mvapich2 ----------------------
       fftw/3.3.8

    -------------------------- /opt/ohpc/pub/moduledeps/gnu8 --------------------------
       R/3.6.1 (D)    hdf5/1.10.5    (D)    openblas/0.3.7
       R/4.1.0        lapack/3.9.1          openblas/0.3.15 (D)
       gsl/2.6        mvapich2/2.3.2 (L)    openmpi3/3.1.4

    ---------------------------- /opt/ohpc/pub/modulefiles ----------------------------
       2.3.5
       ANSYS/19.2
       EasyBuild/3.9.4
       IGV/2.12.3
       JAGS/4.3.0                                    (D)
       JAGS/4.3.0.1
       OpenFOAM/7.0
       STAR/2.7.5c
       abaqus/2017
       aflow/3.2.5
       aflow/3.2.7                                   (D)
       anaconda2/5.3.1
       anaconda3/2020.07
       anaconda3/2022.10                             (D)

You can search the available modules using either of `module avail`, `module
spider` or `module keyword`

To search for module names that contain the word conda:

{:.text}
    vdhiman@katahdin:~$ module avail conda

    ---------------------------- /opt/ohpc/pub/modulefiles ----------------------------
       anaconda2/5.3.1    anaconda3/2020.07    anaconda3/2022.10 (D)

      Where:
       D:  Default Module

    Use "module spider" to find all possible modules.
    Use "module keyword key1 key2 ..." to search for all possible modules matching any
    of the "keys".


    vdhiman@katahdin:~$

Steve has helpfully installed jupyter in `anaconda3/2022.10` for ECE490.
We will load `anaconda3/2022.10`.

{:.text}
    vdhiman@katahdin:~$ module load anaconda3/2022.10
    vdhiman@katahdin:~$ module list # to check which modules are loaded now


    Currently Loaded Modules:
      1) autotools   3) mvapich2/2.3.2   5) gnu8/8.3.0
      2) prun/1.3    4) ohpc             6) anaconda3/2022.10



    vdhiman@katahdin:~$

We can see the environment variables modified by the loading of anaconda3
module using `module show`

{:.text}
    vdhiman@katahdin:~$ module show anaconda3
    -------------------------------------------------------------------------------
       /opt/ohpc/pub/modulefiles/anaconda3/2022.10:
    -------------------------------------------------------------------------------
    whatis("Sets up your environment to use Anaconda3 ")
    setenv("CONDA_DIR","/opt/ohpc/pub/anaconda3/2022.10")
    setenv("CONDA_BIN","/opt/ohpc/pub/anaconda3/2022.10/bin")
    setenv("CONDA_INIT","/opt/ohpc/pub/anaconda3/2022.10/bin/conda-init")
    setenv("INIT_CONDA",". /opt/ohpc/pub/anaconda3/2022.10/bin/conda-init")
    prepend_path("PATH","/opt/ohpc/pub/anaconda3/2022.10/bin")
    load("gnu8")
    vdhiman@katahdin:~$ 

# Initialize Conda

Conda environment still needs to be initialized. We call `$INIT_CONDA` to
initialize the environment. We can check `which` version `python` and
`jupyter` are in the PATH. 

{:.text}
    vdhiman@katahdin:~$ $INIT_CONDA
    (base) which python # which python is available?
    /opt/ohpc/pub/anaconda3/2022.10/bin/python
    (base) which jupyter # which jupyter is available?
    /opt/ohpc/pub/anaconda3/2022.10/bin/jupyter
    (base) 

# Run Jupyter notebook

Let's first check if there are any jupyter notebooks already running. Because
if they are, we do not want run a new one.

{:.text}
    (base) jupyter notebook list
    Currently running servers:
    (base) 
    
It is clear that no jupyter notebooks are running. If there was one runnning,
you can stop it with `jupyter notebook stop <PORT_NUMBER>`.
Let's start a jupyter notebook.

{:.text}
    (base) jupyter notebook --no-browser &
    [1] 3985
    (base) [I 2023-01-21 17:19:37.889 LabApp] JupyterLab extension loaded from /opt/ohpc/pub/anaconda3/2022.10/lib/python3.9/site-packages/jupyterlab
    [I 2023-01-21 17:19:37.889 LabApp] JupyterLab application directory is /opt/ohpc/pub/anaconda3/2022.10/share/jupyter/lab
    [I 17:19:37.895 NotebookApp] The port 8888 is already in use, trying another port.
    [I 17:19:37.895 NotebookApp] Serving notebooks from local directory: /home/vdhiman
    [I 17:19:37.895 NotebookApp] Jupyter Notebook 6.4.12 is running at:
    [I 17:19:37.896 NotebookApp] http://localhost:8889/?token=balksajdlfksjdajdskjfslksdfls
    [I 17:19:37.896 NotebookApp]  or http://127.0.0.1:8889/?token=balksajdlfksjdajdskjfslksdfls
    [I 17:19:37.896 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 17:19:37.901 NotebookApp] 
        
        To access the notebook, open this file in a browser:
            file:///home/vdhiman/.local/share/jupyter/runtime/nbserver-3985-open.html
        Or copy and paste one of these URLs:
            http://localhost:8889/?token=balksajdlfksjdajdskjfslksdfls
         or http://127.0.0.1:8889/?token=balksajdlfksjdajdskjfslksdfls

Pay attention to the number after `localhost:`. For me it is 8889, for you it
will be different. We will be using this number later. We will call this port
number as REMOTE_PORT. Also save the part after `token=`. We will call it TOKEN later on.

We cannot access this yet. If katahdin was not firewalled, anyone could have
acessed this by replacing `localhost` with `katahdin.acg.maine.edu`. However,
`katahdin.acg.maine.edu` is firewalled and we cannot access arbitrary [ports](http://www.tcpipguide.com/free/t_TCPIPApplicationAssignmentsandServerPortNumberRang.htm){: target="_blank"} on
katahdin. Luckily, ssh can help us access with [port-forwarding](https://www.ibm.com/docs/en/zos/2.4.0?topic=examples-openssh-tcp-port-forwarding){: target="_blank"}.

# TCP/UDP ports

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

# Port forwarding

We want the communication between our browser and Jupyter notebook server to
tunnel through the ssh connection.
The information flow for the port forwarding is visualized here:

![]({{pageassetpath}}/ssh-tunneling.svg){:.width500}

Port forwarding through SSH can have two sets of instructions:

1. MacOS or Windows PowerShell or Linux
1. Windows Putty

## MacOS or Windows PowerShell or Linux

Log out of the ssh session on katahdin. Press Ctrl+D to logout. We will login again with ssh port forwarding.

You can create a forward any REMOTE_PORT and a LOCAL_PORT using 

{:.text}
    ssh -L <LOCAL_PORT>:localhost:<REMOTE_PORT> vdhiman@katahdin.acg.maine.edu

For example, my jupyter notebook printed port 8889 which is my REMOTE_PORT;
and I decided to use 8888 as the LOCAL_PORT. You can use any port number
greater than 1024 and smaller than 65535 as the LOCAL_PORT.

{:.text}
    vdhiman@office-desktop:~$ ssh -L 8888:localhost:8889 vdhiman@katahdin.acg.maine.edu
    Last login: Sat Jan 21 15:58:54 2023 from jx3cth3.um.maine.edu
    vdhiman@katahdin:~$


## Windows Putty

[Full instructions here](https://www.ibm.com/support/pages/ssh-tunneling-putty){: target="_blank"}

Log out of the ssh session on katahdin. Press Ctrl+D to logout. We will login again with ssh tunnel.

You can create a forward any REMOTE_PORT and a LOCAL_PORT through ssh
tunneling. In Putty, load your saved session. And click on
"Connection->SSH->Tunnels"

<style type="text/css" > .puttyimg { width: 350px } </style>
![]({{pageassetpath}}/putty-top.png){:.puttyimg}

Under "Add new forward port:" add Destination as "localhost:REMOTE_PORT" where
REMOTE_PORT is the same port that was printed by `jupyter notebook` earlier.
In the "Source port" field add any number between 1025 and 65535.
Then click "Add".

![]({{pageassetpath}}/putty-tunnel-source-destination.png){:.puttyimg}

After adding, the entry should appear in the "Forwarded ports:" area.
![]({{pageassetpath}}/putty-add-tunnel.png){:.puttyimg}

Save the session settings so that this is done every time.

![]({{pageassetpath}}/putty-save-session.png){:.puttyimg}

Log back into katahdin by clicking "Open"

# Accessing Jupyter notebook on your laptop

Now on your laptop, you can open the Jupyter notebook in your browser [http://localhost:8888](http://localhost:8888){:target="_blank"}. Note that here, I used the LOCAL_PORT not the REMOTE_PORT. If everything went alright, then you should should see a password prompt.

![]({{pageassetpath}}/jupyter-login.png){:.width500}

Enter the TOKEN that you saved earlier. If you forgot the token, you can type
`jupyter notebook list` to get the token.
After entering the token, you should see files in your home directory.

![]({{pageassetpath}}/jupyter-tree.png)

To avoid enterying the TOKEN everytime, you can make it a part of the url by
creating a URL similar to what jupyter notebook printed.

{:.text}
    http://localhost:8888/?token=<your TOKEN here> 
    
The only difference is that we replaced the REMOTE_PORT with LOCAL_PORT.
Recall that you can choose the LOCAL_PORT to be same as the REMOTE_PORT

# Uploading `Python_1.ipynb`

If you have been using Google Colab to work on [Python_1.ipynb](https://vikasdhiman.info/ECE490-Neural-Networks/notebooks/01-py-intro/Python_1.ipynb){: target="_blank"}, you can download the file and Upload it using the "Upload" button.

![]({{pageassetpath}}/jupyter-upload-new-terminal.png){:.width500}


Clicking on any '.ipynb' file will open the notebook in runnable mode.

![]({{pageassetpath}}/jupyter-notebook-python_1.png){:.width500}

# Alternative Jupyter Lab interface

Apart from Jupyter notebook interface, Jupyter Lab interface is also available
which looks more like an IDE. You can use either. Jupyter Lab interface can
obtained by adding `lab` after the localhost:8888/. That is
[http://localhost:8888/lab](http://localhost:8888/lab){: target="_blank"}
