Installation
===========================

Dépendances
----------------

Mettre à jour le système:
    .. code-block:: console

        $ sudo apt update
        $ sudo apt upgrade

Installer les outils:
    .. code-block:: console

        $ sudo apt install bc bison build-essential flex gcc libelf-dev libncurses-dev libssl-dev make
    
Instructions
------------------

Création d’un répertoire de travail:
    .. code-block:: console

        $ mkdir ~/kernel
        $ cd ~/kernel

Vérification de la version du noyau Linux install sur son Ubuntu 22.04:
    .. code-block:: console

        $ uname -a

Récupérer les fichiers sources du noyau et le fichier patch real-time correspondant à sa version du noyau Linux:
    .. code-block:: console

        $ wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.15.96.tar.gz
        $ wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.15/patch-5.15.96-rt61.patch.xz
    
.. note::

    Vous pouvez récupérer d’autres versions plus spécifiques du noyau sur le site officiel `kernel.org <https://www.kernel.org/>`_

.. note::

    Si vous ne trouvez pas le fichier patch correspondant, il faudra consulter les anciennes versions dans le répertoire **older/** sur le site officiel:
    https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.15/older/

Décompresser les fichiers sources:
    .. code-block:: console

        $ tar -xzf linux-5.15.96.tar.gz
        $ xz -d patch-5.15.96-rt61.patch.xz

Appliquer le patch:
    .. code-block:: console

        $ cd linux-5.15.96
        $ patch -p1 < ../patch-5.15.96-rt61.patch

Configurer les options de compilation du noyau:
    .. code-block:: console

        $ cp /boot/config-5.15.0-43-generic .config
        $ make menuconfig

.. warning::

    Pour la suite des instructions, si vous rencontrez des erreurs de compilation ou d’installation, il faudra consulter la section :ref:`Débogage <debogage>`.

Compiler le noyau:
    .. code-block:: console

        $ make -j$(nproc)

Installer les modules noyaux:
    .. code-block:: console

        $ sudo make modules_install

Installer le noyau:
    .. code-block:: console

        $ sudo make install

Mettre à jour le Grub et redémarrer sur le nouveau noyau:
    .. code-block:: console

        $ sudo update-grub
        $ sudo reboot

Vérification
----------------

Vérifier si le noyau est bien installé:
    .. code-block:: console

        $ uname -a