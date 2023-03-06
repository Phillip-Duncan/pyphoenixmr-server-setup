
# PyPhoenixMR setup guide on a remote server without sudo access. 

## Installing Python

    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh

## Installing Screen (useful for persistent simulation instances)

    wget http://ftp.gnu.org/gnu/screen/screen-4.8.0.tar.gz
    tar -xf screen-4.8.0.tar.gz
    cd screen-4.8.0
    ./configure --prefix=$HOME/.local
    make
    make install 

## Editing .bashrc

    cd ~
    nano .bashrc

## AT END OF FILE ADD THE LINE:

    export PATH=~/.local/bin:$PATH

- ctrl+o then enter to save
- ctrl+x to exit

## Creating a GitHub Personal Access Token (PAT)
- Go to your Github account.
- Go to Settings > Developer Settings > Personal Access Tokens > Tokens (Classic).
- Generate a new token (classic).
- Name it something (i.e, Remote Deployment).
- Set expiration to no expiration.
- Under **Select Scopes** make sure ALL from **repo** and ALL from **write:packages** are ticked.
- At the bottom of page hit Generate Token.
- Copy your Token and save it somewhere private (you will need this to install PyPhoenixMR and keep it updated easily).

## Installing PyPhoenixMR
Using your PAT from before, replace PAT with your PAT key:

    git clone https://PAT@github.com/Phillip-Duncan/PyPhoenixMR.git
    cd PyPhoenixMR
    pip install -e .

 **YOU HAVE NOW COMPLETED THE SETUP. THE FOLLOWING IS THE COMMAND YOU HAVE TO RUN AT EACH LOGIN OR SSH INTO A REMOTE COMPUTE SERVER:**

## At each login or SSH (to initialize all local paths)

    source ~/.bashrc
    
## Updating PyPhoenixMR
From within the PyPhoenixMR directory just run:

    git pull
    
This will update the current master version of PyPhoenixMR.


## Setting up Simulation Engine, Inputs, etc.
It is useful to use a folder structure discussed in PyPhoenixMR/docs/guide/tutorial.md.
PhoenixMR.zip can then be unzipped into root_sim_dir/shared/engine/.

When creating a new simulation script, you can then specify the directories:

    coil_dir  =  "inputs/coils/"
    sequence_dir  =  "inputs/sequences/"
    phantom_dir  =  "inputs/phantoms/"
    engine_dir  =  "../../shared/engine" 

This can then be used when specifying directory paths (using f-strings), i.e:

    s = Sequence()
    s.load_xml(f'{sequence_dir}GRE/3DGRE.xml')

This should also be used when setting up the simulator for the engine directory:

    sim  =  Simulator(world, simulation_library_path = engine_dir)


**This concludes the quick setup guide of PyPhoenixMR on a remote compute server. For further details check out the PyPhoenixMR documentation.**
