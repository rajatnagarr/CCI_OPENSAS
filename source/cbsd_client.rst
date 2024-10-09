CBSD Client for OpenSAS
=======================

To set up and run the CBSD client for OpenSAS:

1. **Ensure Certificates Are Generated:**
   
   Generate certificates in OpenSAS using this machine’s IP (accessible from other VMs). 
   Place the certificates in the Certs folder of the CBSD client.
   
   .. image:: _static/image1.png
      :alt: Generating certificates
      :align: center
      :scale: 50%

   .. image:: _static/image2.png
      :alt: Certs folder
      :align: center
      :scale: 50%

2. **Copy the Appropriate `gnb` YAML File:**

   - Copy your `gnb` YAML file from your ``srsRAN/configs`` folder to the CBSD client directory.

3. **Modify `run.py`:**

   - Update the script to include your specific srsRAN config file.

   .. code-block:: python

        # In run.py, modify to add your gNB YAML file name
        gnb_config_file = 'your_gnb_config.yml'

   .. image:: _static/image3.png
      :alt: Modifying run.py
      :align: center
      :scale: 50%

4. **Copy the Appropriate `gnb` YAML File:**

   Copy your gNB YAML file from your srsRAN/configs folder to the CBSD client directory.
   
   .. image:: _static/image4.png
      :alt: Copying YAML file
      :align: center
      :scale: 50%

5. **Modify `CBSD.py`:**

   Include the OpenSAS IP and proper CBSD client certificate path.
   
   .. code-block:: python

      # In CBSD.py, modify to include OpenSAS IP and certificate paths
      SAS_ENDPOINT = 'https://<OpenSAS_IP>:1443/sas-api/'
      CERT_PATH = 'Certs/client-<OpenSAS_IP>-0.cert'
      KEY_PATH = 'Certs/client-<OpenSAS_IP>-0.key'
      CA_CERT_PATH = 'Certs/ca.cert'

   .. image:: _static/image5.png
      :alt: Modifying CBSD.py
      :align: center
      :scale: 50%
   
   .. image:: _static/image6.png
      :alt: Example of certificate paths
      :align: center
      :scale: 50%

   .. image:: _static/image7.png
      :alt: CBSD client certificate path example
      :align: center
      :scale: 50%

6. **Run the CBSD Client:**

   - Start the CBSD client using the modified `run.py` script.

     .. code-block:: bash

        python3 run.py --lat <latitude> --lon <longitude> --fcc <FCC-ID> --low <low_freq> --high <high_freq> --eirp <eirp_value> --react <react_value>

   - Example:

     .. code-block:: bash

        python3 run.py --lat 38.88086127246137 --lon -77.11558699967016 --fcc GAA-Arlington --low 3610e6 --high 3620e6 --eirp 20 --react 0

