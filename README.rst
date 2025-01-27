Build Office.js add-ins and custom functions with Python and xlwings
--------------------------------------------------------------------

* For an introduction to Office.js and how to create add-ins with the ``runPython`` command, see: https://docs.xlwings.org/en/latest/pro/server/officejs_addins.html
* For the custom functions docs, see: https://docs.xlwings.org/en/latest/pro/server/officejs_custom_functions.html

This quickstart shows you:

* How to call Python from a button on the task pane
* How to call Python directly from a Ribbon button
* How to use custom functions (a.k.a user-defined functions or UDFs)

xlwings Server, the backend for Office.js-based add-ins, can be used with any web framework and the quickstart repo therefore contains various implementations:

* FastAPI: ``app/server_fastapi.py``

At the end of this quickstart, you'll have a working environment for local development.

1. **Download quickstart repo**: Use Git to clone the following repository: https://github.com/xlwings/xlwings-officejs-quickstart. If you don't want to use Git, you could also download the repo by clicking on the green ``Code`` button, followed by ``Download ZIP``, then unzipping it locally.
2. **Update manifest**: If you want to build your own add-in based off this quickstart repo, replace ``<Id>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Id>`` in ``manifest-xlwings-officejs-quickstart.xml`` with a unique ID that you can create by visiting https://www.guidgen.com or by running the following command in Python: ``import uuid;print(uuid.uuid4())``.
3. **Create certificates**: Generate development certificates as the development server needs to be accessed via https instead of http (even on localhost). Otherwise, icons and alerts won't work and Excel on the web won't load the manifest at all. `Download mkcert <https://github.com/FiloSottile/mkcert/releases>`_ (pick the correct file according to your platform), rename the file to ``mkcert``, then run the following commands from a Terminal/Command Prompt (make sure you're in the same directory as ``mkcert``):

   .. code-block:: text

     $ ./mkcert -install
     $ ./mkcert localhost 127.0.0.1 ::1

   This will generate two files ``localhost+2.pem`` and ``localhost+2-key.pem``: move them to the ``certs`` directory in the root of the ``xlwings-officejs-quickstart`` quickstart repo.

4. **Install Python dependencies**:

   * Local Python installation: create a ``poetry`` environment by running ``poetry install``.

5. **License Keys**:

   You can find our ``xlwings`` trial key and ``openai`` API key in Keeper via ``tjc-secrets/xlwings-trial-key`` and ``tjc-secrets/openai-api-key``, respectively. Run ``cp .env.template .env`` and add them to the newly created ``.env`` file.

6. **Start web app**

   * Run ``poetry run app``
   If you see the following, the server is up and running (showing the FastAPI implementation):

   .. code-block:: text

      $ poetry run app
      INFO:     Will watch for changes in these directories: ['/Users/fz/Dev/xlwings-officejs-quickstart']
      INFO:     Uvicorn running on https://127.0.0.1:8000 (Press CTRL+C to quit)
      INFO:     Started reloader process [56708] using WatchFiles
      INFO:     Started server process [56714]
      INFO:     Waiting for application startup.
      INFO:     Application startup complete.


7. **Sideload the add-in**: Manually load ``manifest-xlwings-officejs-quickstart.xml`` in Excel. This is called *sideloading* and the process differs depending on the platform you're using, see `Office.js docs <https://learn.microsoft.com/en-us/office/dev/add-ins/testing/test-debug-office-add-ins#sideload-an-office-add-in-for-testing>`_ for instructions. Once you've sideloaded the manifest, you'll see the ``Quickstart`` tab in the Ribbon.
8. **Time to play**: You're now ready to play around with the add-in in Excel and make changes to the source code that gets called via ``runPython`` under ``app/server_fastapi.py`` or under the respective file of your framework. To change the source code for custom functions, edit the ``app/custom_function.py`` file. Every time you edit and save the Python code, the development server will restart automatically so that you can instantly try out the code changes in Excel. If you make changes to the HTML file, you'll need to right-click on the task pane and select ``Reload``.
