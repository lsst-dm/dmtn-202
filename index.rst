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

.. TODO: Delete the note below before merging new content to the master branch.

.. note::

   **This technote is not yet published.**

The long-under-defined user batch facility has been examined by the DM-SST and a proposed set of use cases and quasi-requirements on the facility has been produced.

This document is not binding on the construction or operations project until further consideration - it is intended as a more concrete starting point for design efforts and commitments.

What we colloquially call "User Batch" is a shorthand for a set of user-facing computational capabilities called for in the SRD, LSR, OSS, and DMSR using very generic language.

The relevant existing requirements are summarized in the Confluence document `"Level 3 Definition and Traceability" <https://confluence.lsstcorp.org/display/DM/Level+3+Definition+and+Traceability>`__ (note that that page also covers Level 3 / "User Generated" data products).

This note proposes that we recognize that "User Batch" should cover the following capabilities:

#. The user computing capability should allow running in bulk over catalog data.
#. The user computing capability should allow running in bulk over image data.
#. The system capacity is defined as an "amount of computing capacity equivalent to at least **userComputingFraction** (10%) of the total LSST data processing capacity (computing and storage) for the purpose of scientific analysis of LSST data and the production of Level 3 Data Products by external users".
#. We have to provide a software framework to facilitate both catalog- and image-based user computation, which has to support systematic runs over collections of data and has to preserve provenance.
#. The framework(s) has/have to support re-running standard computations from the pipelines in addition to running more free-form user jobs.
#. There has to be a resource allocation mechanism to allow users to be given quotas, which can be modified per-user.




.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
