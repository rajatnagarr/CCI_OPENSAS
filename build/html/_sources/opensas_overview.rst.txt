OpenSAS Overview
================

What is OpenSAS?
----------------

OpenSAS** is the forked version for the Virginia Tech SAS. The role of the SAS is to allow spectrum management of CBSDs, activation of dynamic protection zones, and environmental sensing for incumbent protection. OpenSAS strives to adhere to WinnForum and FCC regulations on SAS and CBRS operations.

Getting Started
---------------

### Method 1: Local Clone

In this method, the repository is cloned locally, which is suitable if HTTPS is required.

1. **Clone the repository using Git:**

   .. code-block:: bash

      git clone https://github.com/CCI-NextG-Testbed/OpenSAS

2. **Create the CA and server/client certificates:**

   - Navigate to the `Certs` folder and run the script:

     .. code-block:: bash

        cd OpenSAS/Core/Certs
        sudo chmod +x create_ssl_certs.sh

   - **Before running the script**, delete the existing `ca.cert` and all other `.key`, `.crt`, and `.csr` files. Only `create_ssl_certs.sh` and `create_client_certs.sh` should remain.
   - Run the script:

     .. code-block:: bash

        ./create_ssl_certs.sh

   - This will create the CA, server, and client certificates in the `Certs` folder.
   - Copy `ca.cert`, `client-<IP/hostname>-0.cert`, and `client-<IP/hostname>-0.key` to the client machine to make HTTPS requests.

3. **Update server certificate paths in `Core/server.py`:**

   - Update the paths to the server certificate and key:

     .. code-block:: python

        httpd.socket = ssl.wrap_socket(
            httpd.socket,
            keyfile="Certs/server_<IP>.key",       # Update this to reflect the new server key
            certfile="Certs/server_<IP>.crt",      # Update this to reflect the new server cert
            server_side=True
        )

4. **Install requirements:**

   .. code-block:: bash

      pip3 install -r requirements.txt

5. **Run the server:**

   .. code-block:: bash

      cd Core
      python3 server.py

6. **Start the frontend/GUI:**

   - Refer to the `OpenSAS Dashboard repository <https://github.com/CCI-NextG-Testbed/OpenSAS-dashboard>`_ for instructions.

7. **CBSDs can access the SAS via the following URL endpoints:**

   - ``https://<IP/hostname>:1443/sas-api/<request>``
   - Examples:

     - ``https://127.0.0.1:1443/sas-api/registration``
     - ``https://localhost:1443/sas-api/spectrumInquiry``
     - ``https://localhost:1443/sas-api/grant``
     - ``https://localhost:1443/sas-api/heartbeat``
     - ``https://localhost:1443/sas-api/relinquishment``
     - ``https://localhost:1443/sas-api/deregistration``

### Method 2: Using Docker Image

Refer to the `OpenSAS Docker repository <https://github.com/CCI-NextG-Testbed/OpenSASDocker>`_ for the Dockerfile to create an image for this repository. The Dockerfile also clones the OpenSAS-dashboard repo and runs it.

**Note:** The Dockerfile is the easiest way to get started.

File Structure
--------------

### Core

The ``Core/`` folder contains everything required to launch the SAS Core Server. This is the true SAS and contains the code that is of primary interest for SAS researchers.

### Front-end for OpenSAS

This repository serves as the web server for the OpenSAS GUI.

**Instructions to run the web server:**

1. **Clone the repository:**

   .. code-block:: bash

      git clone https://github.com/CCI-NextG-Testbed/OpenSAS-dashboard/

2. **Install npm:**

   .. code-block:: bash

      sudo apt-get install npm

3. **Navigate to the cloned directory:**

   .. code-block:: bash

      cd OpenSAS-dashboard

4. **Install all dependencies:**

   .. code-block:: bash

      npm install --legacy-peer-deps

5. **Set the IP & port to the OpenSAS SocketIO server:**

   - In ``src/main.js`` (line 29), update the connection address:

     .. code-block:: javascript

        Vue.use(new VueSocketIO({
          debug: true,
          connection: 'http://<OpenSAS_IP>:8000',
          options: { transports: ['websocket', 'polling', 'flashsocket'] } // Optional options
        }))

6. **Start the server in the development environment:**

   .. code-block:: bash

      npm run dev

