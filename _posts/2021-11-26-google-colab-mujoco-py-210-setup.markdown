---
layout: post
title: "Getting MuJoCo 2.1 to work in Google Colab for OpenAi Gym with mujoco-py"
tags: mujoco
areas:
  - robotics
  - code
project:
splash: 
---

Nothing too much this time. Just finished struggling through the process of getting MuJoCo and mujoco-py working in Google Colab and thought it would be nice to share.
All you have to do is add the following mess of code as a cell to the top of your notebook!

# MuJoCo 2.1 working in Google Colab for OpenAI gym. Just add this as a code block near the top of your notebook to get MuJoCo setup.
<script src='https://gist.github.com/3119ac9c595324c8001a7454f23bf8c8.js'></script>
<noscript><pre><code>

File: google-colab-mujoco-py-setup.py
-------------------------

 #Include this at the top of your colab code
import os
if not os.path.exists('.mujoco_setup_complete'):
  # Get the prereqs
  !apt-get -qq update
  !apt-get -qq install -y libosmesa6-dev libgl1-mesa-glx libglfw3 libgl1-mesa-dev libglew-dev patchelf
  # Get Mujoco
  !mkdir ~/.mujoco
  !wget -q https://mujoco.org/download/mujoco210-linux-x86_64.tar.gz -O mujoco.tar.gz
  !tar -zxf mujoco.tar.gz -C "$HOME/.mujoco"
  !rm mujoco.tar.gz
  # Add it to the actively loaded path and the bashrc path (these only do so much)
  os.environ['LD_LIBRARY_PATH']=os.environ['LD_LIBRARY_PATH'] + ':/root/.mujoco/mujoco210/bin'
  os.environ['LD_PRELOAD']=os.environ['LD_PRELOAD'] + ':/usr/lib/x86_64-linux-gnu/libGLEW.so'
  !echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/.mujoco/mujoco210/bin' >> ~/.bashrc 
  !echo 'export LD_PRELOAD=$LD_PRELOAD:/usr/lib/x86_64-linux-gnu/libGLEW.so' >> ~/.bashrc 
  # THE ANNOYING ONE, FORCE IT INTO LDCONFIG SO WE ACTUALLY GET ACCESS TO IT THIS SESSION
  !echo "/root/.mujoco/mujoco210/bin" > /etc/ld.so.conf.d/mujoco_ld_lib_path.conf
  !ldconfig
  # Install Mujoco-py
  !pip3 install -U 'mujoco-py<2.2,>=2.1'
  # presetup so we don't see output on first env initialization
  import mujoco_py
  # run once
  !touch .mujoco_setup_complete

</code></pre></noscript>

*If there are any issues with formatting, I'll fix them in a few days. This was written on a different system from usual.
