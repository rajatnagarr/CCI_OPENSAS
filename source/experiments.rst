Experiments
===========

The experiment is about demonstration of OpenSAS Server and CBSD Integration for GAA and PAL Operations using srsRAN and Open5GS. The setup will use one VM/PC running OpenSAS and dashboard and another VM/PC running CBSD client with srs gNB in ZMQ mode.

Setting Up the Testbed
----------------------

The testbed consists of:

- :ref:`OpenSAS Core <start-opensas-server>`
- :ref:`OpenSAS Dashboard <launch-opensas-dashboard>`
- :ref:`CBSD with srs gNB <running-cbsd-client>`

.. _start-opensas-server:

1. Start the OpenSAS Server
---------------------------

   From the VM hosting OpenSAS, start the OpenSAS Server:

   .. code-block:: bash

      python server.py

   First, ensure that the OpenSAS server is running.

.. _launch-opensas-dashboard:

2. Launching the OpenSAS Dashboard
----------------------------------

   Once started, launch the OpenSAS dashboard:

   .. code-block:: bash

      npm run dev

   Verify that it is both operational and accessible at `http://localhost:9528/`.


.. _running-cbsd-client:

3. Running the CBSD Client
--------------------------

   In the VM hosting the CBSD client (with Open5GS core, srsRAN gNB, and RF frontend connected), run GAA CBSD first:

   a. **Open a New TMUX Session**


   This allows you to manage multiple terminal sessions.

   b. **Run the following command to start the GAA CBSD first**

   .. code-block:: bash

      python3 run.py --lat 38.88086127246137 --lon -77.11558699967016 --fcc GAA-Arlington --low 3610e6 --high 3620e6 -eirp 20 -react 0

   - After running this, you should see a registration request from the CBSD client sent to OpenSAS, a spectrum inquiry, and then OpenSAS grants. The corresponding prints will show up in the OpenSAS dashboard and OpenSAS console. After a successful grant, another terminal will open up, starting the srsRAN 5G gNB.
   - You will now be able to view this CBSD information in the CBSD list, Spectrum list, and map in the OpenSAS dashboard.

   - The experiments can be found in :ref:`Output for GAA Operation <gaa-operation>`

4. Running the CBSD Client
--------------------------

   Similarly, terminate the `run.py` program in the CBSD console and execute the PAL user CBSD client using:

   .. code-block:: bash

      python3 run.py --lat 38.8818743855486 --lon -77.11132435717902 --fcc CCI-CBRS-PAL --low 3610e6 --high 3620e6 -eirp 20 -react 0

   - The experiments can be found in :ref:`Output for PAL Operation <pal-operation>`

.. _gaa-operation:

5. Output for GAA Operation
---------------------------

- **This image shows the OpenSAS log indicating the CBSD registration.**

  .. figure:: _static/image15.png
     :align: center
     :alt: OpenSAS Log
     :width: 150%
     :scale: 50%

     **Figure 5:** OpenSAS log indicating the CBSD registration.
.. raw:: html

   <br>

- **This image shows the CBSD console logs indicating the CBSD registration and messages for Spectrum Inquiry Request, Grant Request, and other info.**

  .. figure:: _static/image16.png
     :align: center
     :alt: CBSD Console Logs
     :width: 150%
     :scale: 50%

     **Figure 6:** CBSD console logs indicating registration and spectrum inquiries.
.. raw:: html

   <br>

- **This image shows the OpenSAS Dashboard where you can find the location of the CBSD using the map feature.**

  .. figure:: _static/image17.png
     :align: center
     :alt: OpenSAS Dashboard Map
     :width: 150%
     :scale: 50%

     **Figure 7:** OpenSAS Dashboard displaying CBSD location on the map.
.. raw:: html

   <br>

- **This image shows the authorized band for the CBSD post-grant response.**

  .. figure:: _static/image18.png
     :align: center
     :alt: Authorized Band
     :width: 150%
     :scale: 50%

     **Figure 8:** Authorized band for the CBSD after grant response.

.. raw:: html

   <br>

- **This image shows the registered CBSD and its corresponding ID.**

  .. figure:: _static/image19.png
     :align: center
     :alt: Registered CBSD
     :width: 150%
     :scale: 50%

     **Figure 9:** Registered CBSD and its corresponding ID.

.. raw:: html

   <br>

.. _pal-operation:

6. Output for PAL Operation
---------------------------

- **This indicates the spectrum allocation for the PAL user.**

  .. figure:: _static/image20.png
     :align: center
     :alt: Spectrum Allocation
     :width: 150%
     :scale: 50%

     **Figure 10:** Spectrum allocation for the PAL user.

.. raw:: html

   <br>

- **This image shows the registered PAL CBSD user on the dashboard.**

  .. figure:: _static/image21.png
     :align: center
     :alt: Registered PAL CBSD
     :width: 150%
     :scale: 50%

     **Figure 11:** Registered PAL CBSD user on the dashboard.

.. raw:: html

   <br>

- **This image shows the registration of the CBSD PAL user on the OpenSAS core console.**

  .. figure:: _static/image22.png
     :align: center
     :alt: OpenSAS Core Console
     :width: 150%
     :scale: 50%

     **Figure 12:** Registration of CBSD PAL user on the OpenSAS core console.
