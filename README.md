# Tensorflow-Cuda-Issues

Check compatible list here: (https://www.tensorflow.org/install/source#gpu)

Archive of Nvidia Toolkit: (https://developer.nvidia.com/cuda-toolkit-archive)

Nvidia drivers for Ubuntu 18.04
```
ubuntu-drivers devices
WARNING:root:_pkg_get_support nvidia-driver-525-server: package has invalid Support PBheader, cannot determine support level
WARNING:root:_pkg_get_support nvidia-driver-545: package has invalid Support PBheader, cannot determine support level
WARNING:root:_pkg_get_support nvidia-driver-535: package has invalid Support PBheader, cannot determine support level
WARNING:root:_pkg_get_support nvidia-driver-515-server: package has invalid Support PBheader, cannot determine support level
WARNING:root:_pkg_get_support nvidia-driver-525: package has invalid Support PBheader, cannot determine support level
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001E07sv00001043sd0000866Abc03sc00i00
vendor   : NVIDIA Corporation
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-525-server - distro non-free
driver   : nvidia-driver-470 - third-party non-free
driver   : nvidia-driver-545 - third-party non-free recommended
driver   : nvidia-driver-470-server - distro non-free
driver   : nvidia-driver-418-server - distro non-free
driver   : nvidia-driver-535 - third-party non-free
driver   : nvidia-driver-450 - third-party non-free
driver   : nvidia-driver-515-server - distro non-free
driver   : nvidia-driver-525 - third-party non-free
driver   : xserver-xorg-video-nouveau - distro free builtin

```
## Recommend for Ubuntu 18.04: 

- Nvidia driver: 470
- cuda 11.4
- tensorflow-gpu 2.11.0
- python 3.7

Note: there is no support for cuda > 12.2 on Ubuntu 18.04 (there is for < 12.1.1, but requires Python 3.9). Nvidia driver support for Ubuntu 18.04 is < 545 (< cuda 12.3)

## Install python 3.7:

Work around for installing Python 3.9 (https://tecadmin.net/how-to-install-python-3-9-on-ubuntu-18-04/)

To install Python 3.7 on Ubuntu 18.04, you can follow these steps using the terminal. Ubuntu 18.04 by default comes with Python 3.6, so installing Python 3.7 will provide you with an additional version.

1. **Update the package list**:
   Start by updating the package list and installing the prerequisites:
   ```
   sudo apt update
   sudo apt install software-properties-common
   ```

2. **Add the Deadsnakes PPA**:
   Deadsnakes is a PPA containing newer releases of Python than those provided by the default Ubuntu repositories. You can add this PPA to your system with:
   ```
   sudo add-apt-repository ppa:deadsnakes/ppa
   ```

3. **Install Python 3.7**:
   After adding the PPA, you can install Python 3.7 by running:
   ```
   sudo apt update
   sudo apt install python3.7
   ```

4. **Verify the installation**:
   Check that Python 3.7 is correctly installed by checking its version:
   ```
   python3.7 --version
   ```

5. **Optional: Setting up a virtual environment**:
   If you plan to use Python 3.7 for specific projects, it's a good idea to set up a virtual environment. You can install the package for creating virtual environments for Python 3.7:
   ```
   sudo apt install python3.7-venv
   ```
   Then, create a new environment:
   ```
   python3.7 -m venv my-project-env
   ```
   Activate it with:
   ```
   source my-project-env/bin/activate
   ```

To configure your Ubuntu 18.04 system so that the `python` command refers to Python 3.7 by default, you can follow these additional steps after installing Python 3.7. This involves updating the alternatives system to point the `python` command to Python 3.7.


1. **Add Python 3.7 to update-alternatives**:
   First, you can add Python 3.7 to the alternatives system. This allows you to switch between different Python versions more easily.
   ```
   sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
   ```

2. **Configure Python to use Python 3.7**:
   After adding Python 3.7, you can set it as the default for the `python` command:
   ```
   sudo update-alternatives --config python
   ```
   If Python 3.7 is the only alternative you've set, it will automatically become the default. If there are other versions, you might need to select it by typing the selection number when prompted.

3. **Verify the change**:
   Confirm that the correct version is being used by checking the version of Python:
   ```
   python --version
   ```

These steps will configure your system so that typing `python` in the terminal will use Python 3.7 by default. Be aware that changing the default Python interpreter system-wide can affect system scripts that depend on Python 2.x or a specific version of Python 3.x. If any system or application-specific issues arise, you might need to switch back or manage different versions using virtual environments for specific applications.

### Set pip default:

By following the guide to install Python 3.7 on Ubuntu 18.04, `pip` (the package installer for Python) will not be installed automatically with Python 3.7. You will need to install `pip` separately for Python 3.7. Here are the steps to install `pip` for Python 3.7:

1. **Install `python3.7-distutils`:**
   This package contains the `distutils` module needed for `pip`.

   ```sh
   sudo apt install python3.7-distutils
   ```

2. **Download and Install `get-pip.py`:**
   Download the `get-pip.py` script and use it to install `pip`.

   ```sh
   wget https://bootstrap.pypa.io/get-pip.py
   sudo python3.7 get-pip.py
   ```

3. **Verify the Installation:**
   Check that `pip` is installed and associated with Python 3.7.

   ```sh
   python3.7 -m pip --version
   ```

After these steps, you should have `pip` installed for Python 3.7, and you can use it to manage packages for this Python version. Here is the full command list for your convenience:

```sh
# Step 1: Update package list and install prerequisites
sudo apt update
sudo apt install software-properties-common

# Step 2: Add the Deadsnakes PPA and install Python 3.7
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.7

# Step 3: Install pip for Python 3.7
sudo apt install python3.7-distutils
wget https://bootstrap.pypa.io/get-pip.py
sudo python3.7 get-pip.py

# Step 4: Verify pip installation
python3.7 -m pip --version
```

By following these steps, you will ensure that `pip` is available for your Python 3.7 installation on Ubuntu 18.04.

```
echo "alias pip='python3.7 -m pip'" >> ~/.bashrc && source ~/.bashrc
```
## Nvidia driver

If you need to completely remove an older version of NVIDIA drivers and install a new one, especially when you're encountering issues with dependencies or conflicts, here are the detailed steps to ensure a clean installation:

### Step 1: Uninstall Existing NVIDIA Drivers
First, you need to remove any existing NVIDIA drivers to prevent conflicts. Open a terminal and run the following commands:

```bash
sudo apt-get purge nvidia*
```

This command removes all packages that start with "nvidia", which includes all NVIDIA drivers and associated software.

### Step 2: Remove Remaining Configuration Files
After purging the packages, it's a good idea to delete any remaining NVIDIA configuration files:

```bash
sudo rm /etc/modprobe.d/nvidia-*.conf
sudo rm /lib/modprobe.d/blacklist-nvidia.conf
sudo update-initramfs -u
```

These commands remove residual configuration files and update the initial ramdisk to ensure no old NVIDIA drivers are loaded during boot.

### Step 3: Reboot Your System
After purging the NVIDIA software and removing configuration files, reboot your system to ensure all changes are applied and that no old NVIDIA kernel modules are loaded:

```bash
sudo reboot
```

### Step 4: Add the Graphics Drivers PPA (Optional)
To ensure you have access to the latest drivers, consider adding the Graphics Drivers PPA. This step is optional but recommended if you want the latest stable drivers:

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```

### Step 5: Install the New NVIDIA Driver
Once your system is clean and up-to-date, you can install the new NVIDIA driver. First, identify the recommended driver for your graphics card:

```bash
ubuntu-drivers devices
```

Based on the recommendations, install the appropriate driver:

```bash
sudo apt install nvidia-driver-470-server  # Replace 470 with the recommended version
```

### Step 6: Verify the Installation
After installing the new driver, reboot your system again:

```bash
sudo reboot
```

Once rebooted, verify that the new driver is correctly installed by checking the status of the NVIDIA graphics driver:

```bash
nvidia-smi
```

This command will show you the currently active driver version and some details about your NVIDIA GPU.

### Additional Tips
- If you continue to experience issues with the PPA or dependencies, consider downloading the driver directly from NVIDIA's website and using the `.run` file to install it manually. This can bypass package manager issues but requires you to handle dependencies manually.
- Ensure that you have kernel headers installed appropriate for your running kernel, as they are necessary for building NVIDIA driver modules:
  ```bash
  sudo apt install linux-headers-$(uname -r)
  ```

These steps should help you clean up any old NVIDIA driver installations and set up a new driver, ensuring better stability and performance with your NVIDIA GPU.


## Issue with cuDNN
To install cuDNN 8 on Ubuntu 18.04, you should choose the following two packages (https://developer.nvidia.com/rdp/cudnn-archive):

1. **cuDNN Runtime Library for Ubuntu18.04 x86_64 (Deb)**
2. **cuDNN Developer Library for Ubuntu18.04 x86_64 (Deb)**

These packages contain the necessary runtime and development files for using cuDNN with TensorFlow.

### Installation Steps

1. **Download the Packages**
   Download the two `.deb` files from the NVIDIA cuDNN download page.

2. **Install the Packages**
   Use the following commands to install the downloaded packages. Replace `<path_to_downloaded_files>` with the actual path where you downloaded the files.

   ```bash
   sudo dpkg -i <path_to_downloaded_files>/libcudnn8_*_amd64.deb
   sudo dpkg -i <path_to_downloaded_files>/libcudnn8-dev_*_amd64.deb
   ```

3. **Update the Library Cache**
   Ensure the linker can find the newly installed libraries.

   ```bash
   sudo ldconfig
   ```

### Verify Installation
Run the following command to list the cuDNN libraries and verify their presence:

```bash
ls -la /usr/lib/x86_64-linux-gnu/libcudnn*
```

You should see output indicating that the `libcudnn.so.8` files are present in the directory.

### Environment Variables
Make sure your environment variables are set correctly. Add the following lines to your `.bashrc` or `.zshrc` file:

```bash
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda/bin:$PATH
```

Or a specific version:

```
echo 'export PATH=/usr/local/cuda-11.4/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-11.4/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
```

Then, source the updated file:

```bash
source ~/.bashrc  # or source ~/.zshrc
```

### Re-run TensorFlow Command
Finally, re-run the TensorFlow command to verify that it now detects and uses the GPU:

```python
python3 -c "import tensorflow as tf; print('GPU:', tf.config.list_physical_devices('GPU'))"
```

Following these steps should enable TensorFlow to utilize the GPU with cuDNN 8 on Ubuntu 18.04.
