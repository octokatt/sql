---
description: Before getting into the weeds of using SQL...
---

# Context to Remember

Before diving too far into code, it's important to remember the role SQL plays in ETL pipelines.

In the old days, a pipeline was, by default, made nearly entirely of SQL.  The ETL process ran from one carefully-maintained on-site server to another, all under the watchful gaze of a local sysadmin running bash scripts.  Each step was handcrafted, for better and worse, to create better quality.

Since the rise of SaaS like NetSuite and SalesForce, ETL functions often start with an external product, and usually pipe through or to other off-site products.  Parts of the process may be entirely under the watch of external resources, unable to be changed.  Most of the code is automatically generated, with adjustments being in the realm of glue rather than heavy scaffolding.

This new reality is reflected in some best practices \(e.g., SaaS documentation\), but not in others \(e.g., open source documentation\).  Flipping between the two can cause whiplash.  Keeping the context of the documentation in mind while reading about best practices is an important step.  

For the most part, this site is assuming the majority of ETL will involve at least some use of an SaaS.



