.. SUOD documentation master file, created by
   sphinx-quickstart on Sat Feb 15 17:15:28 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to SUOD's documentation!
================================


**Deployment & Documentation & Stats**

.. image:: https://img.shields.io/pypi/v/suod.svg?color=brightgreen
   :target: https://pypi.org/project/suod/
   :alt: PyPI version


.. image:: https://readthedocs.org/projects/suod/badge/?version=latest
   :target: https://suod.readthedocs.io/en/latest/?badge=latest
   :alt: Documentation Status


.. image:: https://img.shields.io/github/stars/yzhao062/suod.svg
   :target: https://github.com/yzhao062/suod/stargazers
   :alt: GitHub stars


.. image:: https://img.shields.io/github/forks/yzhao062/suod.svg?color=blue
   :target: https://github.com/yzhao062/suod/network
   :alt: GitHub forks


.. image:: https://pepy.tech/badge/suod
   :target: https://pepy.tech/project/suod
   :alt: Downloads


.. image:: https://pepy.tech/badge/suod/month
   :target: https://pepy.tech/project/suod
   :alt: Downloads


----


**Build Status & Coverage & Maintainability & License**


.. image:: https://travis-ci.org/yzhao062/suod.svg?branch=master
   :target: https://travis-ci.org/yzhao062/suod
   :alt: Build Status


.. image:: https://circleci.com/gh/yzhao062/SUOD.svg?style=svg
   :target: https://circleci.com/gh/yzhao062/SUOD
   :alt: Circle CI


.. image:: https://ci.appveyor.com/api/projects/status/5kp8psvntp5m1d6m/branch/master?svg=true
   :target: https://ci.appveyor.com/project/yzhao062/combo/branch/master
   :alt: Appveyor


.. image:: https://coveralls.io/repos/github/yzhao062/SUOD/badge.svg
   :target: https://coveralls.io/github/yzhao062/SUOD
   :alt: Coverage Status


.. image:: https://img.shields.io/github/license/yzhao062/suod.svg
   :target: https://github.com/yzhao062/suod/blob/master/LICENSE
   :alt: License


----

**SUOD** (**S**\calable **U**\nsupervised **O**\utlier **D**\etection) is an **acceleration framework for large-scale unsupervised outlier detector training and prediction**.
Notably, anomaly detection is often formulated as an unsupervised problem since the ground truth is expensive to acquire.
To compensate for the unstable nature of unsupervised algorithms, practitioners often build a large number of models for further combination and analysis, e.g., taking the average or majority vote.
**However, this poses scalability challenges in high-dimensional, large datasets**, especially for proximity-base models operating in Euclidean space.

**SUOD** is therefore proposed to address the challenge at three complementary levels:  random projection (**data level**), pseudo-supervised approximation (**model level**), and balanced parallel scheduling (**system level**).
As mentioned, the key focus is to **accelerate the training and prediction when a large number of anomaly detectors are presented**, while preserving the prediction capacity.
Since its inception in Jan 2019, SUOD has been successfully used in various academic researches and industry applications, include PyOD :cite:`zhao2019pyod` and `IQVIA <https://www.iqvia.com/>`_ medical claim analysis.


SUOD is featured for:

* **Unified APIs, detailed documentation, and examples** for the easy use.
* **Optimized performance with JIT and parallelization** when possible, using `numba <https://github.com/numba/numba>`_ and `joblib <https://github.com/joblib/joblib>`_.
* **Fully compatible with the models in PyOD**.
* **Customizable modules and flexible design**: each module may be turned on/off or totally replaced by custom functions.


----

**API Demo**\ :


   .. code-block:: python


       from suod.models.base import SUOD

       # initialize a set of base outlier detectors to train and predict on
       base_estimators = [
           LOF(n_neighbors=5, contamination=contamination),
           LOF(n_neighbors=15, contamination=contamination),
           LOF(n_neighbors=25, contamination=contamination),
           HBOS(contamination=contamination),
           PCA(contamination=contamination),
           OCSVM(contamination=contamination),
           KNN(n_neighbors=5, contamination=contamination),
           KNN(n_neighbors=15, contamination=contamination),
           KNN(n_neighbors=25, contamination=contamination)]

       # initialize a SUOD model with all features turned on
       model = SUOD(base_estimators=base_estimators, n_jobs=6,  # number of workers
                    rp_flag_global=True,  # global flag for random projection
                    bps_flag=True,  # global flag for balanced parallel scheduling
                    approx_flag_global=False,  # global flag for model approximation
                    contamination=contamination)

       model.fit(X_train)  # fit all models with X
       model.approximate(X_train)  # conduct model approximation if it is enabled
       predicted_labels = model.predict(X_test)  # predict labels
       predicted_scores = model.decision_function(X_test)  # predict scores
       predicted_probs = model.predict_proba(X_test)  # predict outlying probability

----

A preliminary version (`accepted at AAAI-20 Security Workshop <http://aics.site/AICS2020/>`_) can be accessed on `arxiv <https://www.andrew.cmu.edu/user/yuezhao2/papers/20-preprint-suod.pdf>`_.
The extended version (under submission at `KDD 2020 (ADS track) <https://www.kdd.org/kdd2020/>`_) can be accessed `here <http://www.andrew.cmu.edu/user/yuezhao2/papers/20-kdd-insubmission-suod.pdf>`_.


If you use SUOD in a scientific publication, we would appreciate citations to the following paper::

    @inproceedings{zhao2020suod,
      author  = {Zhao, Yue and Ding, Xueying and Yang, Jianing and Haoping Bai},
      title   = {{SUOD}: Toward Scalable Unsupervised Outlier Detection},
      journal = {Workshops at the Thirty-Fourth AAAI Conference on Artificial Intelligence},
      year    = {2020}
    }

::

    Yue Zhao, Xueying Ding, Jianing Yang, Haoping Bai, "Toward Scalable Unsupervised Outlier Detection". Workshops at the Thirty-Fourth AAAI Conference on Artificial Intelligence, 2020.




----


.. toctree::
   :maxdepth: 2
   :caption: Contents: Getting Started

   install
   example


.. toctree::
   :maxdepth: 2
   :caption: Contents: Documentation

   api_cc
   api


.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Additional Information

   about
   faq
   whats_new


----

.. rubric:: References

.. bibliography:: zreferences.bib
   :cited:



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
