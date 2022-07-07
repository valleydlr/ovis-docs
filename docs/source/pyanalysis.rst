Python Analysis Creation
========================

Analysis I/O
-----------
An analysis module is a python script and has a general template. There is a class, which must be called the same name as the python script itself, and two class functions: __init__ and get_data. The module is first initialized and then get_data is called which should return a pandas DataFraem. Below are the variables passed from the Grafana interface to these class functions.
__init__

    cont - A Sos.Container object which contains the path information to the SOS container specified in the Grafana query
    start - The beginning of the time range of the Grafana query (in epoch time)
    end - The end of the time range of the Grafana query (in epoch time)
    schema - The LDMS schema specified by the Grafana query
    maxDataPoints - the maximum number of points that Grafana can display on the user's screen

get_data

    metrics - a python list of metrics specified by the Grafana query
    job_id - a string of the job_id specified by the Grafana query
    user_name - a string of the user name specified by the Grafana query
    params - a string of the extra parameters specified by the Grafana query

Basics and Querying the DSOS Database
-------------------------------------

Below is a basic analysis that simply queries the database and returns the DataFrame of the metrics passed in along with the timestamp for each metric. If a job ID or user name is specified, those are used to filter the query further. Note, job_id and user_name must exist in the schema passed in for this code to work.

In the __init__ function, we simply set most things to be self variables to access them later in the get_data. Importantly, we setup a variable called self.src which is a SosDataSource python object (coming from numsos). We config self.src to point to our container and will later use it to query and return data from the database.

In the get_data function we create variables that will be used for the self.src query. The first of these is where_, a array of lists, which acts as the filter for the query. The standard query sets the timestamps to look for to be between the Grafana start and end timestamps. We can also append additional filters to the where_ array based on if job_id or user_name are set. There is also the orderby variable which we are setting as 'time_job_comp' here. This refers to what index we should use when querying the database. Our SOS databases are setup to use permutations of timestamps, job IDs, and component IDs as multi-indices. Depending on your filter, you may want to use a different multi-index.
import os, sys, traceback
import datetime as dt
from graf_analysis.grafanaAnalysis import Analysis
from sosdb import Sos
import pandas as pd
import numpy as np

.. code-block:: python
    class dsosTemplate(Analysis):
        def __init__(self, cont, start, end, schema='job_id', maxDataPoints=4096):
            super().__init__(cont, start, end, schema, 1000000)
    
        def get_data(self, metrics, filters=[],params=None):
            try:
                sel = f'select {",".join(metrics)} from {self.schema}'
                where_clause = self.get_where(filters)
                order = 'time_job_comp'
                orderby='order_by ' + order
                self.query.select(f'{sel} {where_clause} {orderby}')
                res = self.get_all_data(self.query)
                # Fun stuff here!
                print(res.head)
                return res
            except Exception as e:
                a, b, c = sys.exc_info()
                print(str(e)+' '+str(c.tb_lineno))

Testing an Analysis Module
--------------------------

You do not need to query from the Grafana interface to test your module. Below is a simple code which mimics the Grafana pipeline and prints the JSON returned to Grafana. If you wish to find a username based on another metric listed in the schema "jobid", just include "job_id=<job_id number>" to the get_data function.

To run the test module, you will first need to set your path and pythonpath environment variables by running:
.. code-block:: bash
    export PYTHONPATH=/usr/bin/python:/<INSTALL_PATH>/lib/python<PYTHON_VERSION>/site-packages/
    export PATH=/usr/bin:/<INSTALL_PATH>/bin:/<INSTALL_PATH>/sbin::$PATH

Then you can imitate the Grafana query to call your analysis module using a python script such as:

.. code-block :: python

    #!/usr/bin/python3
    
    import time,sys
    from sosdb import Sos
    from grafanaFormatter import DataFormatter
    from table_formatter import table_formatter
    from time_series_formatter import time_series_formatter
    from dsosTemplate import dsosTemplate
    
    sess = Sos.Session("/<DSOS_CONFIG_PATH>/config/dsos.conf")
    cont = '/<PATH_TO_DATABASE>'
    cont = sess.open(cont)
    
    model = dsosTemplate(cont, time.time()-300, time.time(), schema='meminfo', maxDataPoints=4096)
    
    x = model.get_data(['Active'])
    
    #fmt = table_formatter(x)
    fmt = time_series_formatter(x)
    x = fmt.ret_json()
    print(x)

Note: All imports are python scripts that need to reside in the same directory as the test analysis module in order for it to run successfully.   
