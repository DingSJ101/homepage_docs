## TroubleShot

1. 

   ```
   QObject::moveToThread: Current thread (0x919390) is not the object's thread (0xec9330).
   Cannot move to target thread (0x919390)
   
   qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "/home/colson/anaconda3/envs/anonymizer/lib/python3.8/site-packages/cv2/qt/plugins" even though it was found.
   This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.
   
   Available platform plugins are: xcb, eglfs, linuxfb, minimal, minimalegl, offscreen, vnc, wayland-egl, wayland, wayland-xcomposite-egl, wayland-xcomposite-glx, webgl.
   ```

 solution 

```bash
fix 1: use os.environ.pop("QT_QPA_PLATFORM_PLUGIN_PATH") in code
fix 2: try pip install opencv-python-headless (without GUI)
fix 3: delete the libqxcb from "/home/usernamexyz/.local/lib/python3.9/site-packages/cv2/qt/plugins/platforms
```















### docker

1. download [link](https://download.qt.io/official_releases/online_installers/qt-unified-linux-x64-online.run)
2. `./qt-unified-linux-x64-online.run: error while loading shared libraries: libdbus-1.so.3: cannot open shared object file: No such file or directory`
   1. `apt-get install libdbus-1-3`
3. `./qt-unified-linux-x64-online.run: error while loading shared libraries: libxcb-icccm.so.4: cannot open shared object file: No such file or directory`
   1. `apt-get install libxcb-icccm4`
4. `./qt-unified-linux-x64-online.run: error while loading shared libraries: libxcb-image.so.0: cannot open shared object file: No such file or directory`
   1. `apt-get install libxcb-image0`
5. `apt-get install libxcb-keysyms1`
6. `apt-get install libxcb-render-util0`

```
apt-get install libxcb-shape0
apt-get install libxcb-xinerama0
apt-get install libxcb-xkb1
apt-get install libxkbcommon-x11-0
```



