# Fix screen tearing on external displays (nvidia-force-composition-pipeline)
**For Linux users**

## The problem
Laptops with dual gpus (iGPU + dGPU) tend to have screen tearing issues with external displays (even with the laptop)  
integrated display. The most common solution is to activate the option Force Composition Pipeline on Nvidia Xserver settings 
(if you do use Nvidia discrete gpu). Problem is, saving this configuration directly from xserver will mess up your
integrated display config and Linux will not detect it the next reboot.

## The solution
One option is to use a script to simply look for the external display and set the Force Composition Pipeline option.
If this option is not enough you can also activate Force Full Composition Pipeline. In my case (using a Lenovo Legion 5 Pro + Pop!_OS 21.04)
the first was enough.
1. Make sure you have the proprietary nvidia driver installed (if using PopOS just use the popOS nvidia ISO)
2. Use the bash script external_display_force_composition_pipeline.sh
3. Make this setup automatic upon login on graphical user interface:  
3.1 ```nano ~/.profile``` (on some distros could be .bash_profile)  
3.2 Add ```s="$(nvidia-settings -q CurrentMetaMode -t)"
if [[ "${s}" != "" ]]; then
  s="${s#*" :: "}"
  nvidia-settings -a CurrentMetaMode="${s//\}/, ForceCompositionPipeline=On\}}"
fi``` to the end of the file, save, exit and reboot. This will set the option on nvidia xserver for every external monitor (i tested with 2 external displays) every time you perform a login. Avoid using ~/.bashrc as this will execute the command every time a new terminal window is opened.
