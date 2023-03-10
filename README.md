## Walkthrough: how to install WSL and Visual Studio Code on Windows

**Prerequisites: updated Windows 10/11 and enabled virtualization in BIOS.**

1.  **Install WSL**:
    1.  press **`Win Key` + `R`**
    2.  type in `powershell` in a pop up window and press **Ctrl+Shift+Enter** or press and hold **`Ctrl` + `Shift`** and click **OK** to make PowerShell run as administrator
    3.  to enable Virtual Machine Platform and Windows Subsystem for Linux, run
	    - `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
	    -  `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`, restart PC and open PowerShell again
    4.  to get the exact name for the installed distros, run `wsl -l -v`
    5.  [download](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) and install the WSL2 Linux kernel update package for x64 machines. To check your machine's CPU, run `systeminfo | find "System Type"` in PowerShell
    6.  to set the default version of WSL to WSL 2, run `wsl --set-default-version 2`
    7.  if need to delete the any distro, run  `wsl --unregister <DistributionName>`
    8.  run `wsl --list --online`  or  `wsl -l -o` to see a list of distros available for installation.
    9.  run `wsl --install -d <DistributionName>`  to install a required distro, i.e., `wsl --install -d Ubuntu-20.04`
    10.  if needed to change wsl version `wsl -s <DistributionName>`  or  `wsl --setdefault <DistributionName>`
    11.  enter a user name and password[![image](https://github.com/odinokov/WSL_VS_Code/raw/main/img/PowerShell.png)](https://github.com/odinokov/WSL_VS_Code/blob/main/img/PowerShell.png)
    12.  to reset password:
        1.  check the distro app name:
	    - for **Ubuntu**, use `ubuntu`
            -   for **Ubuntu 20.04**, use `ubuntu2004`
            -   for **Ubuntu 18.04**, use `ubuntu1804`
            -   for **Debian**, use `debian`
            -   for **Kali Linux**, use `kali`
        2.  to set root as a default user, run `ubuntu2004 config --default-user root` in PowerShell
        3.  check the username by running `ls /home` in WSL
        4.  create a new password for user `passwd <UserName>`
        5.  to set default user, run `ubuntu config --default-user <UserName>` in PowerShell
        6.  to shut down distro, run `wsl -t <DistributionName>`

2.  **Update WSL packages**:
    1.  to update repositories, run `sudo apt update`
    2.  to check upgradable packages, run `apt list --upgradable`
    3.  to update all packages, run `sudo apt upgrade -y`
    4.  or run a one-liner `sudo apt update && sudo apt upgrade -y` (run `echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null` if 'Temporary failure resolving…' error occurs)

3.  **Install Python**:
    1.  to improve apt download time, run `sudo add-apt-repository -y ppa:apt-fast/stable && sudo apt-get update && sudo apt-get -y install apt-fast aria2`. [Check documentation](https://github.com/ilikenwf/apt-fast) for more details
    2.  to install Python, run `sudo apt-fast install -y python3.9 python3-pip ipython3`
    3.  to installl the build-essential and graphviz packages run `sudo apt-fast install -y build-essential graphviz`
    4.  to sync time run `sudo hwclock -s` or `sudo apt-fast -y install ntpdate && sudo ntpdate pool.ntp.org`

4.  **Install mamba**:
    1.  to download Miniconda, run `cd ~ && aria2c "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh" && bash Mambaforge-$(uname)-$(uname -m).sh`
    2.  to activate mamba, run `mamba init` and open a new terminal
    3.  to install mamba autocompletion, run `mamba install -y -c conda-forge mamba-bash-completion` and open a new terminal
    4.  to create environment, run `mamba create -y -n <MyEnv> python=3.9`
    5.  to activate environment, run `mamba activate <MyEnv>`
    6.  to install packages, run `mamba install -y joblib numba numpy pandas ipython scikit-learn jupyter scipy matplotlib seaborn statsmodels openpyxl tensorflow tensorflow-probability`
    7.  `mamba install -y scikit-learn-intelex`. Basic use `from sklearnex import patch_sklearn; patch_sklearn()`, [check documentation](https://intel.github.io/scikit-learn-intelex) for more details
    8.  to install additional packages, run `mamba install -y -c conda-forge zlib-ng crabz pv umap-learn sktime-all-extras pywavelets lz4 modin-dask tpot xgboost dask dask-ml scikit-mdr skrebate tqdm imbalanced-learn pydot pydotplus`, `mamba install -y -c bioconda pysam pybedtools datamash aria2c`, `mamba install -y install -c https://conda.anaconda.org/biocore scikit-bio`, `mamba install -y -c r rpy2`
    9.  to install the dependencies necessary to add a new repository over HTTPS run `sudo apt-fast install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common`
    10.  to add the CRAN repository to your system sources’ list run `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'`
    11.  to install R base run `sudo apt-fast install -y r-base`
    12.  to backup `<MyEnv>` run `P=${CONDA_PREFIX}/envs/; tar -I "pv -s $(du -sb ${P} | cut -f1)| crabz -l9 -p4" -cf ~/backup_env_"$(date +"%Y_%m_%d_%I_%M_%p")".tar.gz $(realpath --relative-to=${PWD} ${P})`

5.  **Backup WSL**:
    1.  Run `wsl -l -v` in PowerShell to get a full list of the installed distributions.
    2.  Run `wsl --export <distro> <filename.tar>` to backup specific distribution.
    3.  Run `wsl --import <distro name> <distro location> <filename.tar>` to import a previously exported distribution.

6.  **Install Visual Studio Code**:
    1.  [download](https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-user) and install Visual Studio Code for Windows. When prompted to Select Additional Tasks during installation, be sure to check the **Add to PATH** option.
    2.  to open a WSL terminal window, run `wsl` in PowerShell
    3.  to navigate to a user's home folder, run `cd home/<UserName>`
    4.  to view the current directory in Windows File Explorer, run `exporer.exe .`
    5.  enable WSL extension as described here: https://code.visualstudio.com/docs/remote/wsl-tutorial
    5.  to initiate Visual Code, run `code .` in the terminal. Once finished, you now see a WSL indicator in the bottom left corner
    [![image](https://github.com/odinokov/WSL_VS_Code/raw/main/img/WSL_VS_Code.png)](https://github.com/odinokov/WSL_VS_Code/blob/main/img/WSL_VS_Code.png)
    6.  to create new terminal, press **`Ctrl` + `Shift` + `` ` ``**
    7.  to provide user with sufficient permissions to write to files, run `sudo find /home/ -type d -user root -exec sudo chown -R $USER: {} \;`
    8.  run `export PATH="$HOME/bin:$PATH"`
    9.  use VS Code normally
