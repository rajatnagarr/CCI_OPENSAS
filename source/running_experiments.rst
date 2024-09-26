Running the Experiments
=======================

1. **Start the OpenSAS server and then the dashboard.**

   - Ensure both are up and running and the dashboard is accessible.

2. **On the VM where the CBSD client is located:**

   - Start a TMUX terminal and run:

     .. code-block:: bash

        python3 run.py --lat 38.88086127246137 --lon -77.11558699967016 --fcc GAA-Arlington --low 3610e6 --high 3620e6 --eirp 20 --react 0

   - After running this, you should see:

     - A registration request from the CBSD client sent to OpenSAS.
     - A spectrum inquiry.
     - OpenSAS grants.

   - Corresponding outputs will appear in the OpenSAS dashboard and console.
   - Upon a successful grant, another terminal will open, starting the **srsRAN 5G gNB**.
   - You can now view this CBSD information in the CBSD list, Spectrum list, and map in the OpenSAS dashboard.

