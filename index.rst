..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

The long-under-defined "User Batch" facility has been examined by the DM-SST and a proposed set of use cases and quasi-requirements on the facility has been produced.

This document is not binding on the construction or operations project until further consideration - it is intended as a more concrete starting point for design efforts and commitments.

What we colloquially call "User Batch" is a shorthand for a set of user-facing computational capabilities called for in the SRD, LSR, OSS, and DMSR using very generic language.

The relevant existing requirements are summarized in the Confluence document `"Level 3 Definition and Traceability" <https://confluence.lsstcorp.org/display/DM/Level+3+Definition+and+Traceability>`__ (note that that page also covers Level 3 / "User Generated" data products).

This note proposes that we recognize that "User Batch" should cover the following capabilities:

#. The user computing capability should allow running in bulk over catalog data.
#. The user computing capability should allow running in bulk over image data.
#. The system capacity is defined as an "amount of computing capacity equivalent to at least **userComputingFraction** (10%) of the total LSST data processing capacity (computing and storage) for the purpose of scientific analysis of LSST data and the production of Level 3 Data Products by external users".
#. We have to provide a software framework to facilitate both catalog- and image-based user computation, which has to support systematic runs over collections of data and has to preserve provenance.
#. The framework(s) has/have to support re-running standard computations from the pipelines in addition to running more free-form user jobs.
#. There has to be a resource allocation mechanism to allow users to be given quotas, which can be modified per-user.  The association of quotas with defined groups of users (e.g., ad-hoc collaborations and/or formal Science Collaborations) would be a useful further capability.


Additional Details and Assumptions
==================================

Catalog processing
------------------

We assume that most user analysis cases that involve spatially localized searches of the Object or Source table, and/or the retrieval of ForcedSource data for limited sets of Objects, will go to Qserv via the TAP service, with the users then either analyzing the resulting tabular data in notebooks or externally.
Therefore the catalog access use cases considered for "User Batch" should focus on wide-area processing, from, say, LMC scale up to the full sky.

We anticipate that most users will want to use community-standard bulk-numeric-data-processing frameworks for these use cases.
The priority should be on enabling their use.

We assume that the major catalogs from LSST will not only be available through Qserv but also through a spatially sharded set of Parquet files.
(This is well-established "conventional wisdom" but perhaps not yet fully documented.)
In order to process such a dataset, contemporary frameworks such as Dask and/or Spark may be good choices, especially if supplemented by some Rubin-provided tooling and templates to assist users in applying these frameworks to the complete datasets.
These have been discussed in the context of the *Next to the Data Analysis* system, which has historically been discussed somewhat separately from "User Batch", but which we suggest will be one aspect of this system.

What distinguishes "User Batch" catalog processing from, for instance, interactive use of Dask might be that it is an "offline", asynchronous model for submitting work to the system.


Image processing
----------------

We assume that many users wishing to do image processing will want to start with the components and pipelines of the standard image processing, modified for their own needs, over limited areas of sky.
Others will wish to use community image processing tools, in most cases to perform further analysis of calibrated images, but perhaps also to do independent processing from earlier stages in our pipeline.

A key use case is likely to involve processing of postage stamps from calibrated images around objects of particular interest and needing special treatment, such as time-dependent lenses.


Capacity
--------

It has always been recognized that the "10%" would in no way be able to satisfy the needs of the dark energy analyses, with the DESC assumed to have access to substantial additional DOE-provided and other resources.
Discussion of the use cases for the "User Batch" facility should focus on supporting a larger variety of smaller needs, consistent with the vision of the Rubin Observatory providing meaningful access to data, and the computing resources to explore it, to the broadest possible community.


Framework support
-----------------

We anticipate considerable demand for running elements of the Science Pipelines code for image processing.
It seems highly desirable for users to be able to use this code at the PipelineTask / QuantumGraph / "activator" level and not just at the Task level (though the latter is possible for users who wish to reuse our code in their own way).
For this reason, as well as because we have a requirement to enable users to run the standard pipelines and recreate the canonical processing, the User Batch system clearly has to support working with the full Gen 3 workflow.
We wish users both to be able to re-run standard pipelines and to construct pipelines of their own.

For whatever actual batch execution system is provided (e.g., ``slurm``), basic support for mapping Gen3 "quantum" executions into that system should be provided.
Experience from other projects suggests that users, left to their own devices, can manage this, but tend to do so in ways that may be unreliable, e.g., through incomplete understanding of the middleware, or produce access patterns stressful to underlying resources.

While PipelineTask can also be used to organize catalog-to-catalog processing, it seems to provide less obvious value to user processing jobs for catalog data.
As noted above, we anticipate that users will be more likely to want to use community-standard bulk-data-processing frameworks for these use cases.


Use Cases
=========

Bulk reprocessing of commissioning data
---------------------------------------

During Rubin/LSST commissioning it will be necessary to reprocess the data collected to date with evolving versions of the pipelines.
Much of this will be done as a formal operations activity, but during commissioning we will also rely on assistance from community scientists in assessing the Observatory's performance and planning observations to be taken.
This work may require the use of User Batch resources if someone in this group wishes to perform a reprocessing with non-standard conditions or on special selections of data.


Processing of simulated images
------------------------------

Many analyses will benefit from being able to compare the LSST data products with the same analysis run on simulated images or catalogs.


Reprocessing of images with synthetic sources injected
------------------------------------------------------

Injecting synthetic sources into real survey images and then reprocessing the images to produce to final imaging and catalogue data products is an important technique to evaluate the performance of data reduction pipelines.
Images with synthetic sources are an important tool to develop metrics with which to quantify the performance.


Re-estimation of sky background
-------------------------------

Low Surface Brightness (LSB) science is extremely sensitive to sky estimation.
Traditional sky estimation techniques tend to compromise light from low surface brightness objects.
Some LSB science can be achieved with the standard LSST data products; however, sky oversubtraction will still occur around bright sources, destroying some LSB flux in the process.
To fully exploit the potential of LSST to discover LSB objects, alternative approaches for robust sky estimation that mitigate sky oversubtraction will need to be evaluated.
Evaluating the efficacy of different approaches will require bulk reprocessing of PVIs, and eventually possibly running an alternative sky background estimation and subtraction algorithm on a subset of the LSST images.
This is a task that would be done in the user community, with the option to feed proven rersults back into the standard pipelines for future releases. 

Weak lensing and large-scale structure measurements are another example where optimizing sky subtraction is important.


Analysis of unusually-shaped sources
------------------------------------

The analysis of unusually-shaped sources, such as strongly gravitationally lensed systems, and especially time-dependent lenses, is a key user case and is out of scope for the standard shape analysis in the pipelines.
This will have to be done by users.
Analysis of time-dependent lenses will require access to single-epoch (PVI) imagery, and may require access to the non-persisted uncompressed PVIs.


Reprocessing to build systematic error budgets
----------------------------------------------

The Dark Energy Science Collaboration (DESC) anticipate that building systematic error budgets will require some reprocessing of the LSST images,
at the level of ∼10 runs through ∼10% of the dataset (`LSST DESC Science Roadmap Version v2.6 <https://lsstdesc.org/assets/pdf/docs/DESC_SRM_latest.pdf>`__).

While processing at this scale is likely beyond the planned User Batch capacity, the User Batch system will be useful during the earlier stages of developing the processes involved,
and it would ease the DESC's work if the workflow tooling allowed a workflow from the User Batch system to be readily tranferred to DESC-specific external resources.


Training machine-learning classifiers
-------------------------------------

Supervised machine learning algorithms are central to the classification of astronomical objects.


Cross matching with other astronomical catalogs
-----------------------------------------------

Computing cross match catalogs between LSST and other catalogs, such as Gaia, is central to modern astronomical analyses.
Some level of this, for very widely used external catalogs, especially of the weaker "neighbor table" variety, may be done by the project (for instance, cross-matching with Gaia will be essential to internal quality control).
However, true cross-matching is generally a science-case-specific activity and will be done by users with custom procedures.


Computing periodograms
----------------------

Users will want to compute periodograms on time-series data obtained from the ForcedSource catalog - for example, to yield parameters to be used in supervised classification algorithms.
Note that periodograms are compute-intensive and can benefit from the availability of GPUs.


.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
