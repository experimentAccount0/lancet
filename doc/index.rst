.. Lancet documentation master file, created by
   sphinx-quickstart on Fri Dec  6 11:24:15 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. Differences to Topographica's conf.py
   sys.path.insert(0, os.path.abspath('../external/param/'))
   html_title = 'The Topographica Neural Map Simulator'
   html_logo = 'images/topo-banner7.png'
   html_static_path = ['_static','Reference_Manual']
   html_domain_indices = True

|
|

.. figure:: _static/main_logo.png
   :align:   center

   **Launch jobs, organize the output, and dissect the results.**

Introduction
____________

Lancet is designed to help you organize the output of your research
tools, store it, and dissect the data you have collected. The output
of a single simulation run or analysis rarely contains all the data
you need; Lancet helps you generate data from many runs and analyse it
using your own Python code.

Parameter spaces often need to be explored for the purpose of
plotting, tuning, or analysis. Lancet helps you extract the
information you care about from potentially enormous volumes of data
generated by such parameter exploration.


Features
________

Lancet is pure, platform-independent Python with minimal dependencies,
and supports both Python 2 and Python 3. It is designed to be
extremely flexible and lightweight, allowing researchers to use only
the components they need without having to understand the whole design
or all the component available.

* A simple, useful core with advanced functionality strictly
  optional. Use what you need without learning all components.

* All components use a declarative style, helping to ensure
  reproduciblity.

* Succinctly express the high dimensional parameter spaces, without
  nested loops.

* Easily interface with external tools using ``ShellCommand`` or
  quickly build a flexible, reusable interfaces to advanced simulators
  or analysis tools. Currently, the Topographica neural simulator is
  actively supported.

* Seamlessly switch from running jobs locally to launching jobs on a
  compute cluster.

* Keep your output organized together with key metadata for
  reproducibility.

* Quickly load your data and view the parameters from previous runs.

* Integrates well with other popular tools such as `IPython Notebook
  <http://ipython.org/notebook>`_ and the `pandas
  <http://pandas.pydata.org>`_ data analysis library.

* Actively used in scientific research and publication.


Quickstart Example
__________________

The following example demonstrates how Lancet can be used to specify,
launch and collate jobs (requires a factor command for factorizing
integers in the style of the command in `GNU coreutils
<http://www.gnu.org/software/coreutils/manual/coreutils.html>`_): ::

   >>> import lancet
   >>> example_name   = 'prime_quintuplet'
   >>> integers       = lancet.Range('integer', 100, 115, steps=16, fp_precision=0)
   >>> factor_cmd     = lancet.ShellCommand(executable='factor', posargs=['integer'])
   # Runs locally. A QLauncher could be used to launch jobs with Grid Engine.
   >>> lancet.Launcher(example_name, integers, factor_cmd, output_directory='output')()
   # Collate and print the the primes in the input range of integers
   >>> def load_factors(filename):
   ...    "Return output of 'factor' command as dictionary of factors."
   ...    with open(filename, 'r') as f:
   ...        factor_list = f.read().replace(':', '').split()
   ...    return dict(enumerate(int(el) for el in factor_list))

   >>> output_files   = lancet.FilePattern('filename', './output/*-prime*/streams/*.o*')
   >>> output_factors = lancet.FileInfo(output_files, 'filename',
   ...                                  lancet.CustomFile(metadata_fn=load_factors))
   >>> primes = sorted(factors[0] for factors in output_factors.specs
                       if factors[0]==factors[1]) # i.e. if the input integer is the 1st factor
   >>> primes  # A prime quintuplet, the closest admissable constellation of 5 primes.
   [101, 103, 107, 109, 113]  

Installation
____________

A PyPI package will be made available shortly. The development version
can be obtained from Lancet's `repository on GitHub
<https://github.com/ioam/lancet>`_  using::

   git clone git://github.com/ioam/lancet.git

Lancet uses the lightweight `param <https://github.com/ioam/param>`_
package. It may be installed using::

   pip install param


Documentation
_____________

Lancet's code is well documented and the Python code may also be
inspected and read directly as the majority of modules, classes and
methods contain extensive docstrings. You can start with the above 
example, and then study the first paper below to see how Lancet can be 
integrated in a real research workflow.

Papers published using Lancet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Paper describing `reproducible workflows using Lancet and IPython Notebook <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3874632/>`_: ::

   @article{Stevens2013,
     author    = {Jean-Luc Stevens and Marco Elver and James A. Bednar},
     title     = {{An automated and reproducible workflow for running and analysing
                    neural simulations using Lancet and IPython Notebook}},
     journal   = {{Frontiers in Neuroinformatics}},
     year      = {2013},
     volume    = {7},
     pages     = {44},
     url       = {http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3874632/}
   }

All the code, notebooks and other assets used in the following paper along with
information on how to reproduce this paper may be found  `here <https://github.com/ioam/topographica/tree/master/models/stevens.jn13>`_.
Lancet was exclusively used to launch and analyse simulations results: ::

   @article{Stevens2013,
      author = {Stevens, Jean-Luc R. and Law, Judith S. and
      Antol\'{i}k, J\'{a}n and Bednar, James A.},
      title = {Mechanisms for Stable, Robust, and Adaptive
      Development of Orientation Maps in the Primary Visual Cortex},
      journal = {The Journal of Neuroscience},
      volume = {33}, 
      number = {40}, 
      pages = {15747-15766}, 
      year = {2013}, 
      doi = {10.1523/JNEUROSCI.1037-13.2013}, 
      url = {http://www.jneurosci.org/content/33/40/15747.full}
   }

The following paper used Lancet to collect the results of thousands of microprocessor simulations::

   @inproceedings{ElverN2014,
     author    = {Marco Elver and Vijay Nagarajan},
     title     = {{TSO-CC: Consistency directed cache coherence for TSO}},
     booktitle = {HPCA},
     year      = {2014},
     pages     = {165-176},
     website = {http://homepages.inf.ed.ac.uk/s0787712/research/tsocc}
   }


Contributors
~~~~~~~~~~~~

The following people have contributed to Lancet's design and
implementation:

Jean-Luc Stevens: Original coding and design

`Marco Elver <https://github.com/melver/lancet>`_ : Python 3 fork,
cleaned up many aspects of the design.

James A. Bednar: For supporting the development of a solution that
works with any tool and not just `Topographica
<http://www.topographica.org>`_ .

Philipp Rudiger: Testing, feedback and suggestions.

And now for something completely different...
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|
.. figure:: _static/pythons.svg
   :align: center
   :scale: 100 %

.. Contents:

  .. toctree::
     :maxdepth: 2

.. Indices and tables

  * :ref:`genindex`
  * :ref:`modindex`
  * :ref:`search`
