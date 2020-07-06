# INSTALLING GNOME ON ARCH LINUX

* **STEP 1**

    * Check for internet access...

    ```
    ip addr

        or...

    ip a

        or...

    wifi-menu

    ```

* **STEP 2**

    * Update our repo...

    ```
    sudo pacman -Syyy    

    ```

* **STEP 3**

    * Install gnome...

    ```
    sudo pacman -S gnome gnome-extra -Not a must to install gnome-extra
    ```

* **STEP 4**

    * Enable gdm (login manager)...

    ```
    systemctl enable gdm
    ```

* **STEP 5**

    * Finalizing...

    ```
    sudo pacman -Syu

    sudo reboot

    activities-settings-region&language

    ```









