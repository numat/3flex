3flex
=====

Raw TCP/IP driver and command line tool for [Micromeritics 3Flex surface characterization analyzers](http://www.micromeritics.com/Product-Showcase/3Flex-Surface-Characterization-Analyzer.aspx).

<p align="center">
  <img src="http://www.micromeritics.com/Repository/Images/3500_front_small.jpg" height="400" />
</p>

This is a read-only driver, as there is currently no support for
programmatically controlling the 3Flex.

Installation
============

```
pip install threeflex
```

If you don't like pip, you can also install from source:

```
git clone https://github.com/numat/threeflex.git
cd threeflex
python setup.py install
```

Usage
=====

###Command Line

For basic tasks, this driver includes a command-line interface. This command
will output either a JSON representation of the current state, or a raw stream
of tab-separated values with the `--stream` flag. Read the help for more.

```
threeflex --help
```

###Python

For more complex projects, use python to automate your workflow.

```python
from threeflex import Analyzer
analyzer = Analyzer("192.168.77.100")
print(analyzer.get())
```

If the analyzer is currently running, this should return an object of the form:

```python
{
  "time": 4816304.0,             # Time since beginning, in milliseconds
  "manifold": {
    "pressure": 6.3,             # Manifold pressure, torr  
    "temperature": 318.1,        # Manifold temperature, K
    "volume": 35.0               # Manifold volume, cc
  },
  "ports": [
    {
      "adsorbed": 2.8,           # Quantity adsorbed, cc (STP)
      "data_points_taken": 0,    # Number of data points taken
      "dosed": 3.0142,           # Quantity dosed, cc (STP)
      "freespace": {
        "cold": 22.205,          # Cold freespace, cc (STP)
        "warm": 22.205           # Warm freespace, cc (STP)
      },
      "has_manifold": False,     # Don't know.
      "last_data_point": {       # Data saved from previous measurement
        "adsorbed": 0.0,
        "dosed": 0.0,
        "elapsed_time": 0.0,
        "p0": 0.0,
        "pressure": 0.0
      },
      "pressure": 4.2,           # Pressure, torr
      "pressure_table_index": 0, # Don't know.
      "temperatures": {          # Port temperature readings at various points
        "ambient": 298.0,
        "analysis": 298.0,
        "overall": 318.1
      },
      "valve_open": False,       # Whether or not port valve is open
      "volume": 15.7195          # Port volume, cc
    },
    ...                          # Two more of above for the other ports
  ]
}
```
