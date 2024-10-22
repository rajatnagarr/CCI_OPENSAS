CBSD Creation
=============

Building CBSD Client with srs gNB (ZMQ Mode) and Dockerized Open5GS Core
------------------------------------------------------------------------

1. **Install srsRAN Project**

   Install the srsRAN project with Dockerized Open5GS in ZMQ mode and test independent 5G end-to-end operation.

   Follow the instructions at:

   - `srsRAN ZMQ-based Setup <https://docs.srsran.com/projects/project/en/latest/tutorials/source/srsUE/source/index.html#zeromq-based-setup>`_

2. **Clone the CBSD Repository**

   .. code-block:: bash

      git clone https://github.com/asheeshtripathi/CBSD

3. **Copy Certificates**

   - Copy the certs generated during building of Open SAS
   - The ca.cert, client-<IP/hostname>-0.cert and client-<IP/hostname>-0.key need to be copied to the client machine(CBSD machine) to make HTTPS requests. 
   - Make sure that the Certs  generated in OpenSAS with this machine's IP (accesscible from other VMs) are placed in the Certs folder here. 


4. **Modify Configuration Files**

   - **Modify `run.py`**: Add your gNB YAML file name.
   .. figure:: _static/image10.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 60%

   - **Modify `CBSD.py`**: Include the OpenSAS IP and proper CBSD client certificate paths.
   .. figure:: _static/image11.png
      :align: center
      :alt: SAS-CBSD State Diagram
      :scale: 60%

   Ensure that the CBSD client can communicate with the OpenSAS server over HTTPS.

