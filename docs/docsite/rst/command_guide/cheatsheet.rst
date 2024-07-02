.. _cheatsheet:

**********************
Ansible CLI cheatsheet
**********************

This page shows one or more examples of each Ansible command line utility with some common flags added and a link to the full documentation for the command.
This page offers a quick reminder of some common use cases only - it may be out of date or incomplete or both.
For canonical documentation, follow the links to the CLI pages.

.. contents::
   :local:

ansible-playbook
================

.. code-block:: bash

   ansible-playbook -i /path/to/my_inventory_file -u my_connection_user -k -f 3 -T 30 -t my_tag -M /path/to/my_modules -b -K my_playbook.yml

Loads ``my_playbook.yml`` from the current working directory and:
  - ``-i`` - uses ``my_inventory_file`` in the path provided for :ref:`inventory <intro_inventory>` to match the :ref:`pattern <intro_patterns>`.
  - ``-u`` - connects :ref:`over SSH <connections>` as ``my_connection_user``.
  - ``-k`` - asks for password which is then provided to SSH authentication.
  - ``-f`` - allocates 3 :ref:`forks <playbooks_strategies>`.
  - ``-T`` - sets a 30-second timeout.
  - ``-t`` - runs only tasks marked with the :ref:`tag <tags>` ``my_tag``.
  - ``-M`` - loads :ref:`local modules <developing_locally>` from ``/path/to/my/modules``.
  - ``-b`` - executes with elevated privileges (uses :ref:`become <become>`).
  - ``-K`` - prompts the user for the become password.

See :ref:`ansible-playbook` for detailed documentation.

ansible-galaxy
==============

Installing collections
^^^^^^^^^^^^^^^^^^^^^^

* Install a single collection:

.. code-block:: bash

    ansible-galaxy collection install mynamespace.mycollection

Downloads ``mynamespace.mycollection`` from the configured Galaxy server (`galaxy.ansible.com` by default).

* Install a list of collections:

.. code-block:: bash

    ansible-galaxy collection install -r requirements.yml

Downloads the list of collections specified in the ``requirements.yml`` file.

* List all installed collections:

.. code-block:: bash

  ansible-galaxy collection list

Installing roles
^^^^^^^^^^^^^^^^

* Install a role named `example.role`:

.. code-block:: bash

  ansible-galaxy role install example.role

  # SNIPPED_OUTPUT
  - extracting example.role to /home/user/.ansible/roles/example.role
  - example.role was installed successfully

* List all installed roles:

.. code-block:: bash

  ansible-galaxy role list

See :ref:`ansible-galaxy` for detailed documentation.

ansible
=======

Running ad-hoc commands
^^^^^^^^^^^^^^^^^^^^^^^

* Install a package

.. code-block:: bash

  ansible localhost -m ansible.builtin.apt -a "name=apache2 state=present" -b -K

Runs  ``ansible localhost``- on your local system.
- ``name=apache2 state=present`` - installs the `apache2` package on a Debian-based system.
- ``-b`` - uses :ref:`become <become>` to execute with elevated privileges.
- ``-m`` - specifies a module name.
- ``-K`` - prompts for the privilege escalation password.

.. code-block:: bash

    localhost | SUCCESS => {
    "cache_update_time": 1709959287,
    "cache_updated": false,
    "changed": false
    #...

ansible-doc
===========

* Show plugin names and their source files:

.. code-block:: bash

  ansible-doc -F
  #...

* Show available plugins:

ls
  #...

ansible-vault
=============
The ansible-vault command is a utility provided by Ansible for encrypting sensitive data files.It's particularly useful for securing sensitive information such as passwords,API keys,and other confidential data within your Ansible projects.

Create encrypted file:To create a new encrypted file or edit an existing one, use the ansible-vault create or ansible-vault edit command:

.. code-block:: bash

- ``ansible-vault create secret.yml`` 
- ``ansible-vault edit secret.yml``

When you run these commands, Ansible will prompt you to set and confirm a password for encrypting the file.

Encrypt an Existing File: Encrypt an existing plaintext file using ansible-vault encrypt:

.. code-block:: bash

- ``ansible-vault encrypt secret.yml``

This will encrypt secret.yml and prompt you for a password.

View Encrypted File: To view the contents of an encrypted file without editing it, use ansible-vault view:

.. code-block:: bash

- ``ansible-vault view secret.yml` 

You will need to provide the password to decrypt and view the file.

Decrypt Encrypted File: To decrypt an encrypted file temporarily and edit it, use ansible-vault decrypt:

.. code-block:: bash

- ``ansible-vault decrypt secret.yml`` 

Encrypt String: Encrypt a string directly from the command line with ansible-vault encrypt_string:

.. code-block:: bash

- ``ansible-vault encrypt_string 'my_secret_password' --name 'my_password'`` 

This will output an encrypted string that you can directly use in your playbooks.