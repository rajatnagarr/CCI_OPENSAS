Requirements
============

To complete this tutorial, you will need:

1. **PC with Ubuntu 22.04.1 LTS**
2. **srsRAN Project**: Open-source 5G software radio suite.
3. **OpenSAS Core**: The core server component of OpenSAS.
4. **OpenSAS Dashboard**: The GUI dashboard for OpenSAS.
5. **ZeroMQ**: A high-performance asynchronous messaging library.
6. **CBSD**: Citizens Broadband Radio Service Device.

Tutorial Steps
--------------

The tutorial goes through the following steps:

1. **Cloning and Building OpenSAS Core from Source.**
2. **Cloning and Building OpenSAS Dashboard from Source.**

   - Alternatively, you can use the Docker image of both OpenSAS Core and OpenSAS Dashboard.

3. **CBSD Creation**: Building CBSD Client with srs gNB (ZMQ Mode) and Dockerized Open5GS Core.
4. **Setting Up the Testbed** with the following components:

   - OpenSAS Core
   - OpenSAS Dashboard
   - CBSD with srs gNB
5. Running  Experiments with GAA and PAL CBSDs.

Building OpenSAS from Source
============================

1. **Clone the Repository**

   .. code-block:: bash

      git clone https://github.com/CCI-NextG-Testbed/OpenSAS

   -  This is the forked version for the Virginia Tech SAS, called OpenSAS. The role of the SAS is to allow spectrum management of CBSDs, activation of dynamic protection zones, and environmental sensing for incumbent protection. OpenSAS strives to adhere to WInnForum and FCC regulations on SAS and CBRS operations.
   - The Core/ folder contains everything required to launch the SAS Core Server. This is the true SAS. Regardess of your institution, this contains the code that is of primary interest for SAS researchers.

   .. figure:: _static/image0.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 50%

2. **Generate Certificates**

   Then, create the CA and server/client certificates using the create_ssl_certs.sh script. 
   
   Navigate to the `Certs` directory and run the script:

   .. code-block:: bash

      cd OpenSAS/Core/Certs
      sudo chmod +x create_ssl_certs.sh
      ./create_ssl_certs.sh

   - Enter the IP of the machine running OpenSAS if making CBSD requests externally, if making the requests locally, the IP/hostname can be 127.0.0.1. 
   - Before running the script, make sure to delete the existing ca.cert and all other .key,.crt and .csr files. The only to files remaining should be create_ssl_certs.sh and create_client_certs.sh. The create_client_certs.sh can be used to create client certs for each new client. Once existing certs are deleted, run the script.
   - This will create the CA, server, and client certificates in the Certs folder. Copy the `ca.cert`, `client-<IP/hostname>-0.cert`, and `client-<IP/hostname>-0.key` files to the client machine (CBSD) to make HTTPS requests.

   .. figure:: _static/image1.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 50%

3. **Update Server Configuration**

   Update the paths to the server certificate and key in `Core/server.py`.

   .. figure:: _static/image2.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 60%

4. **Install Requirements**

   .. code-block:: bash

      pip3 install -r requirements.txt

   - This will install all the required packages such as requests, python-engine.io. For the communication between the frontend and core to work the python-socketio and vue-socket.io versions should be compatible. The versions specified in the requirements.txt are tested to be compatible

   .. figure:: _static/image3.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 50%

5. **Run the OpenSAS Server**

   .. code-block:: bash

      cd ../
      python3 server.py

   .. figure:: _static/image4.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 50%

   The OpenSAS server will start listening for HTTPS requests from CBSDs.
    **CBSDs can access the SAS via the following URL endpoints:**
    
   .. code-block:: none
    
      https://<IP/hostname>:1443/sas-api/<request> 
    
   **Examples:**
    
   - `https://127.0.0.1:1443/sas-api/registration`
   - `https://192.168.0.110:1443/sas-api/registration`
   - `https://localhost:1443/sas-api/spectrumInquiry`
   - `https://localhost:1443/sas-api/grant`
   - `https://localhost:1443/sas-api/heartbeat`
   - `https://localhost:1443/sas-api/relinquishment`
   - `https://localhost:1443/sas-api/deregistration`
    
   These endpoints allow CBSDs to perform various actions such as registration, spectrum inquiry, grant requests, heartbeats, relinquishment, and deregistration with the OpenSAS server.

Building OpenSAS Dashboard from Source
======================================

The dashboard serves as the webserver for the OpenSAS GUI.


1. **Clone the Repository**

   .. code-block:: bash

      git clone https://github.com/CCI-NextG-Testbed/OpenSAS-dashboard/

2. **Install npm**

   .. code-block:: bash

      sudo apt-get install npm

3. **Install Dependencies**

   From the cloned directory, enter install dependencies

   .. code-block:: bash

      cd OpenSAS-dashboard
      npm install --legacy-peer-deps

4. **Configure and Run the Dashboard**

   - Set the IP and port to the OpenSAS SocketIO in the configuration files. 
   - If the OpenSAS core is running on a different VM or machine, use its IP, else it will be localhost. 
   - The port on OpenSAS is set to 8000.

   .. code-block:: javascript

      Vue.use(new VueSocketIO({
         debug: true,
         connection: 'http://10.147.20.114:8000',
         options: {  transports: ['websocket', 'polling',  'flashsocket'] } //Optional options
      }))

   - Then, start the dashboard:

   .. code-block:: bash

      npm run dev

   .. figure:: _static/image7.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 80%

   - Access the dashboard at `http://localhost:9528/`.

   .. figure:: _static/image8.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 40%

   - You can view the list of CBSDs here

   .. figure:: _static/image9.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 40%

  

Build from Docker Image of Open SAS and Open SAS Dashboard
==========================================================

Alternatively, you can build and run OpenSAS using Docker, the dockerfile is the easiest way to get started:

1. **Install Docker Engine**

   .. code-block:: bash

      sudo apt update
      sudo apt install docker.io

2. **Clone the Docker Repository**

   .. code-block:: bash

      git clone https://github.com/CCI-NextG-Testbed/OpenSASDocker.git
      cd OpenSASDocker

3. **Build the Docker Image**

   .. code-block:: bash

      sudo docker build . --tag=opensas-server-dash --no-cache

4. **Run the OpenSAS Container**

   .. code-block:: bash

      docker run --network=host --name=opensas-container -it --privileged opensas-server-dash

   - The OpenSAS core and dashboard services will start automatically. 
   - This starts the two services:
    
   - **The OpenSAS core** which will listen to HTTP requests from CBSDs
   - **The OpenSAS dashboard webserver**
    
   The web portal can be accessed via `http://localhost:9528/`
    
   The CBSDs can access the SAS via the following URL endpoints:
    
   .. code-block:: none
    
      http://localhost:1443/sas-api/registration
      http://localhost:1443/sas-api/spectrumInquiry
      http://localhost:1443/sas-api/grant
      http://localhost:1443/sas-api/heartbeat
      http://localhost:1443/sas-api/relinquishment
      http://localhost:1443/sas-api/deregistration
   

