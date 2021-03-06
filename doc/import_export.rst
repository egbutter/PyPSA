######################
Data Import and Export
######################

PyPSA is intended to be data format agnostic, but given the reliance
internally on pandas DataFrames, it is natural to use
comma-separated-variable (CSV) files.

The import-export functionality can be found in pypsa/io.py.


Import from folder CSV
======================

Create a folder with CSVs for each component type
(e.g. ``generators.csv, storage_units.csv``), then a CSV for each
time-dependent variable (e.g. ``generators-p_max_pu.csv,
loads-p_set.csv``).

Then run

``network.import_from_csv_folder(csv_folder_name)``

See the :doc:`examples` in pypsa/examples/.

Note that is is NOT necessary to add every single column, only those where values differ from the defaults listed in :doc:`components`. All empty values/columns are filled with the defaults.



Adding components one-by-one
============================

Networks can also be built "manually" by calling

``network.add(class_name,name,**kwargs)``

Where ``class_name`` is for example
``"Line","Bus","Generator","StorageUnit`` and ``name`` is the unique
name of the component. Other attributes can also be specified:

.. code:: python

    network.add("Bus","my_bus_0")
    network.add("Bus","my_bus_1",v_nom=380)
    network.add("Line","my_line_name",bus0="my_bus_0",bus1="my_bus_1",length=34,r=2,x=4)

Any attributes which are not specified will be given the default value from :doc:`components`.

Import from Pypower
===================

PyPSA supports import from Pypower's ppc dictionary/numpy.array format
version 2.


.. code:: python

    from pypower.api import case30

    ppc = case30()

    network.import_from_pypower_ppc(ppc)


Export to CSV
=============

Exporting the network is easy:

``network.export_to_csv_folder(csv_folder_name)``

The time-dependent quantities to be exported must be specified
explicitly, see the docstring of this function.
