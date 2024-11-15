## Walkthrough: how to install WSL and Visual Studio Code on Windows

**Prerequisites: updated Windows 10/11 and enabled virtualization in BIOS.**

1.  **Install WSL**:
    1.  press **`Win Key` + `R`** and type in `powershell` in a pop up window and press **`Ctrl` + `Shift` + `Enter`** or press and hold **`Ctrl` + `Shift`** and click **`OK`** to make PowerShell run as administrator
    2.  to enable Virtual Machine Platform and Windows Subsystem for Linux, run
	    - `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
	    -  `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`, restart PC and open PowerShell again
    3.  [download](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) and install the WSL2 Linux kernel update package for x64 machines. To check your machine's CPU, run `systeminfo | find "System Type"` in PowerShell
    4.  to set the default version of WSL to WSL 2, run `wsl --set-default-version 2`
    5.  check [WSLDL](https://github.com/yuk7/wsldl), a lightweight, portable launcher and installer for WSL/WSL2 installer. It enables Windows users to install and manage [custom Linux distributions](https://cloud-images.ubuntu.com/wsl/noble/current/), which are not available in the Microsoft Store.
    6.  or run `wsl --list --online`  or  `wsl -l -o` to see a list of distros available for installation.
    7.  to get the exact name for the installed distros, run `wsl -l -v`
    8.  if need to delete the any distro, run  `wsl --unregister <DistributionName>`
    9.  run `wsl --install -d <DistributionName>`  to install a required distro, i.e., `wsl --install -d Ubuntu-20.04`
    10.  if needed to change wsl version `wsl -s <DistributionName>`  or  `wsl --setdefault <DistributionName>`
    11.  enter a user name and password[![image](https://github.com/odinokov/WSL_VS_Code/raw/main/img/PowerShell.png)](https://github.com/odinokov/WSL_VS_Code/blob/main/img/PowerShell.png)
    12.  Add the created user to the sudo group using the command `sudo usermod -aG sudo <username>`
    13.  install [WinSSHTerm](https://winsshterm.blogspot.com) - a tabbed SSH client for Windows that integrates PuTTY, WinSCP, and VcXsrv into a single interface. It simplifies remote server management by allowing secure terminal sessions, file transfers, and remote graphical applications within one tool. WinSSHTerm also supports terminal customization through reg files (https://github.com/AlexAkulov/putty-color-themes, https://github.com/aniskop/ssh-themes) and automates the installation and startup of the Windows Subsystem for Linux [(WSL Starter)](https://github.com/WinSSHTerm/WSL_Starter). Moreover, the 3rd party [Migrate2WinSSHTerm tool](https://github.com/P-St/Migrate2WinSSHTerm) can facilitate the migration of existing configurations from PuTTY, MobaXterm, Xsell and other SHH clients to WinSSHTerm.
    14.  to reset password:
            -  to check the distro app name, run `wsl -l -v` in PowerShell
            -  to set root as a default user, run `<DistributionName> config --default-user root` in PowerShell
            -  check the username by running `ls /home` in WSL
            -  create a new password for user `passwd <UserName>`
            -  to set the default user, run `ubuntu config --default-user <UserName>` in PowerShell
            -  to shut down distro, run `wsl -t <DistributionName>`

2.  **Update WSL packages**:
    1.  to update repositories, run `sudo apt update`
    2.  to check upgradable packages, run `apt list --upgradable`
    3.  to update all packages, run `sudo apt upgrade -y`
    4.  or run a one-liner `sudo apt update && sudo apt upgrade -y` (run `echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null` if 'Temporary failure resolving…' error occurs)

3.  **Install Python**:
    1.  to improve apt download time, run `sudo add-apt-repository -y ppa:apt-fast/stable && sudo apt-get update && sudo apt-get -y install apt-fast aria2`. [Check documentation](https://github.com/ilikenwf/apt-fast) for more details
    2.  to install Python, run `sudo apt-fast install -y python3.12 python3-pip ipython3`
    3.  to install LaTeX packages `sudo apt-fast install -y dvipng cm-super texlive texlive-latex-extra texlive-xetex texlive-fonts-recommended pandoc` (try `sudo apt-fast install texlive-plain-generic`)
    4.  to install the build-essential, run `sudo apt-fast install -y build-essential libcurl4-openssl-dev libssl-dev libxml2-dev`
    5.  to sync time run `sudo hwclock -s` or `sudo apt-fast -y install ntpdate && sudo ntpdate pool.ntp.org`

4.  **Install Fish Shell (not POSIX-Compliant shell)**:
    1. `sudo apt-add-repository ppa:fish-shell/release-3 && sudo apt update && sudo apt upgrade -y && sudo apt-fast install fish tmux`
    2. `echo "set-option -g default-shell /usr/bin/fish" > ~/.tmux.conf & echo "set -sg escape-time 10 # for vim" >> ~/.tmux.conf`
    3. `tmux source-file ~/.tmux.conf`

6.  **Install mamba**:
    1.  to download Miniconda, run `cd ~ && aria2c "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh" && bash Mambaforge-$(uname)-$(uname -m).sh`
    2.  to activate mamba, run `mamba init` and open a new terminal
    3.  to install mamba autocompletion, run `mamba install -y -c conda-forge mamba-bash-completion` and open a new terminal
    4.  to create an environment, run `mamba create -y -n <MyEnv> python=3.12`
    5.  to activate the environment, run `mamba activate <MyEnv>`
    6.  to install font-manager and Microsoft's TrueType core fonts, run `mamba install -y -c conda-forge fonts-conda-forge mscorefonts`
    7.  to install graphviz package, run `mamba install -y -c anaconda graphviz`
    8.  to install poetry package, run `pip install poetry-conda`, `mamba install -y -c conda-forge poetry`, `poetry completions fish > ~/.config/fish/completions/poetry.fish`
    9.  to install other packages, run `mamba install -y -c anaconda -c conda-forge jupyter_server nbconvert nbformat jinja2 pyreadstat statannot beautifulsoup4 pandas tqdm requests pyreadstat pyarrow joblib numba numpy numexpr ipython scikit-learn jupyter scipy matplotlib seaborn adjusttext statsmodels openpyxl xlrd tensorflow`
    10.  `mamba install -y scikit-learn-intelex`. Basic use `from sklearnex import patch_sklearn; patch_sklearn()`, [check documentation](https://intel.github.io/scikit-learn-intelex) for more details
    11.  to install additional packages, run `mamba install -y -c conda-forge zlib-ng crabz pv umap-learn sktime-all-extras pywavelets lz4 modin-dask tpot xgboost dask dask-ml scikit-mdr skrebate tqdm imbalanced-learn pydot pydotplus`, `mamba install -y -c bioconda mosdepth d4tools bedops gget gseapy pysam pybedtools datamash aria2c ucsc-bedgraphtobigwig`, `mamba install -y install -c https://conda.anaconda.org/biocore scikit-bio`, `mamba install -y -c r rpy2`, `pip install feather-format`
    12.   to backup `<MyEnv>` run `P=${CONDA_PREFIX}/envs/; tar -I "pv -s $(du -sb ${P} | cut -f1)| crabz -l9 -p4" -chf ~/backup_env_"$(date +"%Y_%m_%d_%I_%M_%p")".tar.gz $(realpath --relative-to=${PWD} ${P})`

7.  **Install R**:
    1.  to install the dependencies necessary to add a new repository over HTTPS, run `sudo apt-fast install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common`
    2.  to add the CRAN repository to your system sources’ list run `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'`
    3.  to install the R base run `sudo apt-fast install -y r-base`
    4.  Install `gdebi` for Debian packages and dependencies `sudo apt-fast install gdebi-core`
    5.  Get RStudio `wget https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2024.04.2-764-amd64.deb`
    6.  Install RStudio `sudo gdebi rstudio-server-2024.04.2-764-amd64.deb`
    7.  Run RStudo `sudo systemctl start rstudio-server`, open a web browser in Windows and go to http://localhost:8787.
    8.  Stop RStudio `sudo systemctl stop rstudio-server` or to prevent autistart `sudo systemctl disable rstudio-server`
    9.  In RStudio:
   ```R
        # Define custom R library path
	R_libs <- "~/R_libs"
	
	# Create directory for R packages in the home folder, if it doesn't exist
	if (!dir.exists(R_libs)) {
	    dir.create(R_libs, recursive = TRUE)
	}
	
	# Set custom library path as the first in the search path
	.libPaths(c(R_libs, .libPaths()))
	
	# Update only the packages in the personal library
	update.packages(
	    lib.loc = R_libs,
	    ask = FALSE,       # No prompts for updating
	    checkBuilt = TRUE, # Rebuild if necessary for current R version
	    dependencies = TRUE
	)
	
	# Install and load 'pacman' package in the custom library
	if (!requireNamespace("pacman", quietly = TRUE)) {
	    install.packages(
	        "pacman",
	        lib = R_libs,
	        dependencies = TRUE,
	        repos = "https://packagemanager.posit.co/cran/latest"
	    )
	}
	library(pacman, lib.loc = R_libs)
	
	# Use pacman to load required packages
	pacman::p_load(devtools)

   ```

8.  **Backup WSL**:
    1.  Run `wsl -l -v` in PowerShell to get a full list of the installed distributions.
    2.  Run `wsl --export <distro> <filename.tar>` to backup specific distribution.
    3.  Run `wsl --import <distro name> <distro location> <filename.tar>` to import a previously exported distribution.

9.  **Install Visual Studio Code**:
    1.  [download](https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-user) and install Visual Studio Code for Windows. When prompted to Select Additional Tasks during installation, be sure to check the **Add to PATH** option.
    2.  to open a WSL terminal window, run `wsl` in PowerShell
    3.  to navigate to a user's home folder, run `cd home/<UserName>`
    4.  to view the current directory in Windows File Explorer, run `explorer.exe .`
    5.  enable WSL extension as described here: https://code.visualstudio.com/docs/remote/wsl-tutorial
    5.  to initiate Visual Code, run `code .` in the terminal. Once finished, you now see a WSL indicator in the bottom left corner
       
    [![image](https://github.com/odinokov/WSL_VS_Code/raw/main/img/WSL_VS_Code.png)](https://github.com/odinokov/WSL_VS_Code/blob/main/img/WSL_VS_Code.png)

    7.  to create new terminal, press **`Ctrl` + `Shift` + `` ` ``**
    8.  to provide user with sufficient permissions to write to files, run `sudo find /home/ -type d -user root -exec sudo chown -R $USER: {} \;`
    9.  run `export PATH="$HOME/bin:$PATH"`
    10.  use VS Code normally
