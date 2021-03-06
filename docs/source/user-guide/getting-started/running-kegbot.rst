.. _running-kegbot:

Running Kegbot
==============

Congratulations! You're now ready to start up Kegbot.  There are a few programs
included in the ``bin/`` directory of ``pykeg`` that you will now become
familiar with:

* ``kegbot-admin.py``, an administrative utility tool
* ``kegbot_core.py``, the core kegbot daemon
* ``kegboard_daemon.py``, the daemon that listens to the kegboard

Overview of Kegbot Apps
-----------------------

Kegbot is actually comprised of several programs.  All programs are located in
the `bin/` directory of the pykeg tree.

All applications share some common command-line flags; you can run
`<programname> --help` to see detailed command-line options for each program.
This section gives a short overview of each.

kegbot-admin.py
^^^^^^^^^^^^^^^

This program is clone of django's `django-admin.py` program; it contains a
variety of tools and management commands.

kegbot_core.py
^^^^^^^^^^^^^^

`kegbot_core` is the central manager of the kegbot system.  It listens to the
microcontroller, authenticates users, and records drinks.

kegboard_daemon.py
^^^^^^^^^^^^^^^^^^

This program implements a listener for the kegboard hardware device.  It
connects to the kegbot core and reports sensor data.

kegboard_monitor.py
^^^^^^^^^^^^^^^^^^^

A simple application which listens to a kegboard and dumps all packets it
receives.  This is primarily useful in debugging.

kegbot_master.py
^^^^^^^^^^^^^^^^

`kegbot_master` is a daemon manager; you can use it to start and stop several
kegboard applications as daemons.

lcd_daemon.py
^^^^^^^^^^^^^

A daemon that attaches to the kegbot core and an LCD device, and prints status
updates to the LCD.  Supports Matrix-Orbital and Crystalfontz displays.

rfid_daemon.py
^^^^^^^^^^^^^^

This daemon will attach to a Phidgets RFID device, and report the IDs of devices
it reads to the kegbot core authentication system.

sound_server.py
^^^^^^^^^^^^^^^

This optional daemon will play sound files when various flow events occur.

test_flow.py
^^^^^^^^^^^^

This is a test application which synthesizes fake kegboard packets.


Run Kegbot Core
---------------

You're now ready to run the Kegbot core process::

	% ./bin/kegbot_core.py
	2009-09-10 00:23:36,259 INFO     (tap-manager) Registering new tap: flow0
	2009-09-10 00:23:36,466 INFO     (main) Starting all service threads.
	2009-09-10 00:23:36,466 INFO     (main) starting thread "alarmmanager-thread"
	2009-09-10 00:23:36,474 INFO     (main) starting thread "eventhub-thread"
	2009-09-10 00:23:36,475 INFO     (main) starting thread "net-thread"
	2009-09-10 00:23:36,476 INFO     (net-thread) network thread started
	2009-09-10 00:23:36,476 INFO     (main) starting thread "flowmonitor-thread"
	2009-09-10 00:23:36,477 INFO     (main) starting thread "service-thread"
	2009-09-10 00:23:36,478 INFO     (main) starting thread "watchdog-thread"
	2009-09-10 00:23:36,479 INFO     (main) All threads started.

If successful, you should see something like the above.

You can also see options for any application in ``bin/`` by using the ``--help``
or ``--helpshort`` flags::

	% ./bin/kegbot_core.py --help
	Kegbot Core Application.
	
	This is the Kegbot Core application, which runs the main drink recording and
	post-processing loop. There is exactly one instance of a kegbot core per kegbot
	system.
	
	For more information, please see the kegbot documentation.
	
	flags:
	
	pykeg.core.kb_app:
		--[no]daemon: Run application in daemon mode
			(default: 'false')
		-?,--[no]help: show this help
		--[no]helpshort: show usage only for this module
		--[no]log_to_file: Send log messages to the log file defined by --logfile
			(default: 'true')
		--[no]log_to_stdout: Send log messages to the console
			(default: 'true')
		--logfile: Default log file for log messages
			(default: 'kegbot.log')
		--logformat: Default format to use for log messages.
			(default: '%(asctime)s %(levelname)-8s (%(name)s) %(message)s')
		--[no]verbose: Generate extra logging information.
			(default: 'false')
	
	pykeg.core.net.kegnet_server:
		--kb_core_bind_addr: Address that the kegnet server should bind to.
			(default: 'localhost:9805')
	
	google3.pyglib.flags:
		--flagfile: Insert flag definitions from the given file into the command line.
			(default: '')
		--undefok: comma-separated list of flag names that it is okay to specify on
			the command line even if the program does not define a flag with that name.
			IMPORTANT: flags in this list that have arguments MUST use the --flag=value
			format.
			(default: '')

Start up kegboard daemon
------------------------

TODO

Start up Kegweb
---------------

You can now start Kegweb. Try running the built in development server::

	% ./bin/kegbot-admin.py runserver 0.0.0.0:8000
	Validating models...
	0 errors found

	Django version 1.0.2 final, using settings 'pykeg.settings'
	Development server is running at http://0.0.0.0:8000/
	Quit the server with CONTROL-C.

Browse to the kegweb home page, eg http://localhost/

Create a Keg 
------------

Browse to the kegweb admin site to access and set various kegbot settings, eg
http://localhost/admin/

Add a keg, http://localhost/admin/core/keg/

* Select default site
* Select beer type
* Select keg size
* Set start date
* Set keg status
* Optional entries: Description, Origcost, Notes
* SAVE

Set keg tap details, http://localhost/admin/core/kegtap/

* Set Ml per tick. Recommeded settings - Swissflow SF800 meter(0.163934426) or
  Vision 2000 meter(0.4545454545)
* Select newely added keg in current keg drop down list.

