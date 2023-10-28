# FOSS4GNA 2023 Notes

I am adding the notes I took from the [FOSS4GNA 2023](https://foss4gna.org) in Baltimore, with my own commentary in the **TL;DR** section of each talk.


## Finding the World: What on Earth is Global Building Intelligence

Jason Kaufman @ Oak Ridge National Laboratory

**TL;DR**: ORNL's research project to use Bayesian modeling approach to classify building attributes, primarily for disaster preparedness purposes. Very much focused on US right now - modeling results are in "alpha" phase and expected to be publicly released in the coming months; long term goal is to make model for global coverage.

Because the "prior" was from data sources like USA Structures, Open Street Maps, etc, the "cold start" cost of a new jurisdiction is likely significant.

[Slides](https://github.com/ikding/foss2gna_2023/issues/1)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3361589/)

Imagine a single building.  What do we need to know about it?  How can we learn information about it?  Can we identify buildings that are at higher risk of failure in natural disasters and conflict zones?  What if we wanted to do this for every building in the world?

Significant AI advances have made this challenge feasible by allowing detection and delineation of building footprints maps from satellite imagery possible on a global scale.  These advances have attracted the stakeholders in urban planning, transportation, emergency response, energy, and natural security who operate in environments where building detail is a critical function to mission success. While embracing the knowledge advances made in building detection, stakeholders often still find remaining unknown elements that are crucial to their models, raising questions as to how the remaining data could be estimated.

The Global Building Intelligence (GBI) project aims to provide a complete set of 14 key attributes (e.g., construction materials, occupancy, size, purpose, etc.) by first harmonizing open-source data and then filling remaining data gaps through a spatial Bayesian inference model.  To meet this challenge, we have developed a geospatial workflow comprised of three primary components.  First is the development a robust taxonomic strategy flexible enough to handle the width and breadth of global geospatial data including OpenStreetMap, Census, Footprints.  Second, building our material knowledge database of building stocks and inference rules that will allow us to simplify data from dozens of different sources and varied construction types worldwide into a standardized schema that will inform the model.  Third, data identified through this workflow must be ingested into an Extract, Transform, Load (ETL) pipeline designed to interface with a novel modified Global Earthquake Model (GEM) taxonomy while leaving open the ability to incorporate additional taxonomies from stakeholders as needed.

We will discuss the GEM taxonomy’s value for global building attribution and why it was modified to account for other attributes.  Additionally, we will present our strategy for the creation of data dictionaries, translation tables, and our ETL pipeline that allow for translations between the source taxonomy and the modified GEM taxonomy.  Finally, we will discuss using a small case study the GBI workflow and the next steps for research.

</details>


## Geowombat - Remote Sensing with a Strong Backend

Michael Mann @ George Washington Univ

**TL;DR**: GeoWombat is a remote sensing processing focused library that likely has some overlap with KoBold's internal raster processing tooling. There are some things they do differently (e.g. GeoWombat leverages Ray and Jay whenever possible, to avoid building large task graphs), and it comes with some capabilities that I wasn't sure how to do with internal tooling (e.g. co-registration of Landsat8 and Sentinel2 images), plus some support of scikit-learn style pipelines for remote sensing data. Worth keeping an eye out for sure - although I am getting the impression that this project was mostly done by two people with different full time jobs, so I wasn't sure how much development activity it will see.

Also - the reason that the project is called "GeoWombat" is because Wombat poops cubes (that is, their poop is cube shaped). There, now you are also blessed with this piece of forbidden knowledge, LOL.

[Slides](https://github.com/ikding/foss2gna_2023/issues/2)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3297627/)

Python has become the leading programming language in a wide variety of sciences including remote sensing. Developments in N-dimensional array handling using numpy, and particularly xarray and its integration with projects like Dask, jax, and CUDA allow for intuitive and scalable analysis of multi-dimensional data in Python. Spatial data types can now be handle by projects like rasterio and GDAL for gridded spatial data formats like GeoTIFF, and geopandas for spatial vectors. Once gridded arrays are accessible, they can be passed to tools like scikit-learn for machine learning or for use in deep-learning with packages like Keras to predict labels. Together these packages provide all the core components for an end-to-end raster and remote sensing operations.

Although these operations are possible outside of GeoWombat, implementing even simple operations, like co-registering or mosaicing multiple rasters requires a deep understanding of the underlying data structures, coordinate reference systems, and even affine transformations. GeoWombat aims to fill this gap by providing an intuitive end-to-end platform for a variety of common spatial modeling and remote sensing operations in Python. It provides a wide variety of common methods like cloud and STAC reads, sub-pixel image co-registration, BRDF correction, mosaicing, point and polygon sampling/extraction, moving window operations, vegetation indices and transforms like EVI, NDVI and Tasseled-Cap, deep-learning through PyTorch/Tensorflow/Keras, and Sklearn modeling and prediction.

On it's face it is designed with novice python users in mind, but likes its namesake, it's back end is strong enough to scale up cubes for a continental level analysis. In order to scale, GeoWombat uses Dask arrays to build task graphs built on smaller chunks of data for parallel computations of one or more raster files of any size. To speed up computationally intensive methods - like generating time-series statistics on a pixel-by-pixel basis - also GeoWombat provides an intuitive interface for applying functions using a GPU with CUDA.

GeoWombat is an excellent tool for anyone working with spatial data, including GIS professionals, remote sensing scientists, and machine learning practitioners who need to process, analyze, and visualize large-scale raster data. Its extensive documentation and increasingly active development community make it a valuable resource for anyone looking to efficiently handle large-scale spatial data in Python.

</details>


## Open Sourcing the Future of Flood Risk Data

Seth Lawler @ Dewberry

**TL;DR**: high level overview talk about efforts that are going on in FEM, NOAA, and US Army Corps of Engineers (USACE) for developing large scale flood risk data. The development efforts are very much focused on leveraging open source technology (cloud services, Zarr, parquet, gdal... etc), and also innovative approaches of tracking and visualizing model outputs.

[Slides](https://github.com/ikding/foss2gna_2023/issues/3)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3297606/)

The state of the art for developing large scale flood risk data in support of climate resiliency planning is undergoing a significant change—at FEMA, USACE, NOAA, USGS, and at select leading state agencies. Some of the drivers of this change are scientific:

1. Improvements in availability of high-resolution (spatial and temporal) meteorological datasets offer opportunities for new computational methods and approaches.

2. Suggestions from scientific technical communities overseeing federal data development have called for probabilistic methodologies to replace deterministic approaches.

3. Advances in 2D computational engines and model building tools that make high quality 2D models more accessible to build and compute.

4. The need for real-time, impact-informed decision-making tools and systems.


Other drivers are technical:

A. The desktop computer cannot handle the volume of simulations required for the scientific driver above for large scale studies.

B. Software built for desktop development cannot reasonably achieve expected outcomes in terms of time or ability to compute when attempting to process many Gigabytes to Terabytes of data.

C. Data access and retrieval becomes a bottleneck in the development process.

This talk will address approaches currently underway at the agencies listed above to manage these scientific and technical challenges, unlocking the power of large datasets, through the adoption of open-source software development and the movement toward open data. Examples will include:

* Deployment of the OGC API’s, with special emphasis on the Process-API for managing cloud compute resources asynchronously for large scale flood modeling campaigns.
* Containerized deployments of USACE computational engines traditionally accessible only via a desktop GUI for hydrologic and hydraulic modeling.
* GDAL based geospatial tools written in Go and Python that perform highly optimized processing of large datasets in the cloud or desktop.
* Evaluating RDF based metadata tools for managing linkages between models, data, and services across the federal agencies.
* Cloud native pipelines for transforming NetCDF served in NOAA FTP to a cloud mirror of zarr files for gridded time series for applications at USACE/FEMA.

Discussion will also include how the FAIR principles are guiding development activities at the National Water Center and in FEMA’s Future of Flood Risk Data initiative. Throughout the talk, challenges in implementation, acceptance, and understanding by federal partners and existing IT teams threaten to slow progress in adoption of open-source solutions, with a special emphasis on PostgreSQL.

</details>


## AI-Powered Automated Landscape Monitoring at Global Scale

Mark Mathis @ Impact Observatory

**TL;DR**: this is one of the coolest geospatial ML talk I've seen. Impact Observatory publishes an annual land use and land cover (LULC) categorization with Sentinel2 imagery; each year's model requires processing over 2M Sentinel2 scenes (0.6 petabytes), and they use a U-Net CNN trained on 5 billion pixel hand-labeled dataset. They also talked about approaches to suppress spurious classifications using other features (e.g. terrain roughness, annual max NDVI, Ecoregions, etc). Big geospatial data, to be sure!

(I learned later that Mark was a co-founder of Descartes Labs.)

[Slides](https://github.com/ikding/foss2gna_2023/issues/4)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3297615/)

Recent advances in deep learning, cloud computing, and Earth observation datasets enable a breakthrough in automated mapping and monitoring at global scale in near real time. We report on work generating a sequence of annual, global land use and land cover maps at 10m spatial resolution for years 2017 through 2022, publicly available as an open science product. Each map required processing over 2 million Sentinel-2 scenes (0.6 petabytes). Each map was completed in approximately one week using a cloud computing system. We report our map accuracy and recent work to improve the maps for detecting changes across years.

Recently, we developed a deep learning UNet model for land use and land cover (LULC) categorization of Copernicus Sentinel-2 10m-resolution images [Karra, Kontgis, et al., IEEE IGARSS 2021], trained on a 5 billion pixel hand-labeled dataset developed by our team [Brown, Brumby, et al., Scientific Data 2022], and applied this model to over 400,000 Sentinel-2 images collected in 2020 to produce a global LULC map.

We now report on new work generating a time sequence of global, annual maps at 10m resolution for years 2017 through 2022, released as an open science product. Each map required processing over 2 million Sentinel-2 scenes (0.6 petabytes). Our UNet uses 6 spectral bands from the Copernicus Sentinel-2 Level 2A at-surface-reflectance product: 10m spatial resolution visible blue, green, red, near infrared, and two 20m-resolution short wave infrared bands resampled to 10m. Instead of analyzing mosaicked imagery, we applied our UNet to each individual satellite image and used a weighted sum across time to produce an annual thematic map per Sentinel scene, and then mosaicked these per-scene maps to produce the global map. We followed the standard categories of USGS LCMAP: built area, cropland, tree cover, grass/shrub, water, wetland, ice/snow, and bare ground. Each global map was completed in under one week using a scalable, parallel, cloud-native processing system running on 10,000 cpu cores, leveraging open standard STAC services and cloud-optimized satellite imagery. We report our accuracy, and recent work to improve the maps for detecting changes across years.

</details>


## An Introduction and Background to the E84 Geospatial Radar Report

Robert Cheetham @ Element 84

**TL;DR**: this talk can be think of the "how the sausage is made" talk that accompanies the recently released [Geospatial Technology Radar 2023](https://element84.com/geospatial-tech-radar-23/#) by Element 84. I found the document and the talk useful in bringing me attention to technologies that are actually quite mature, but I didn't know about it because I was not the main user. For more details, one should check out the document directly. (e.g. I didn't know about STAC, SpatioTemporal Asset Catalogs, until this conference, but it was apparently a very mature technology that are useful for organizing remote sensing imagery.)

[Slides](https://github.com/ikding/foss2gna_2023/issues/5)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3297650/)

Element 84 is developing a “geospatial technology radar” and we are looking forward to sharing it for the first time with folks at FOSS4G-NA in October 2023.

Element 84 is a woman-owned small business that uses open source tooling to create geospatial data processing pipelines and build web software that helps answer big questions about our health, our infrastructure, and our changing planet. Element 84 has more than a dozen years of experience building geospatial software, and in early 2023, Element 84 acquired Azavea, another open source professional services business with more than 20 years under its belt.

The combined firms represent a significant breadth of engineering skill and experience and several dozen engineers and designers with a long-term commitment to geospatial open source, open standards, UX, cloud infrastructure, and Earth observation data. Inspired by the Thoughtworks Tech Radar, an annual report on engineering tools and platforms, Element 84 has set out to create a similar Geospatial Technology Radar. What does that mean? The Radar will be a document that outlines the contemporary geospatial technology practice that we think is currently interesting and where we think it is going. It aims to represent technology and practices that are “in motion” based on our day-to-day work experience. As such, it’s not intended to be a comprehensive overview of everything that’s happening in geospatial, but, rather, an idiosyncratic and opinionated guide to what our teams tell us is working and not working. What are we seeing and where do we think it’s going?

This talk will introduce the idea of a technology radar and provide an overview of the process we’ve used to develop this initial version, how topics (blips) are generated and assessed, and some key themes we have observed over the past year.

</details>


## Geo-Unleashed: How Apache Sedona is Revolutionizing Geospatial Data Analysis

Jia Yu @ Wherobots

**TL;DR**: this talk was given by the co-founder of Wherobots and co-creator of Apache Sedona, which extended big data systems to support spatial data processing capabilities. Think of the following analogy:

* Big Tabular Data : Apache Spark : DataBricks
* Big Geospatial Data (mostly tabular?) : Apache Sedona : Wherobots

I remember coming across GeoSpark (precursor of Sedona) back in 2019 when I was looking for scaling out geospatial computations to Spark; great to see that the project is now much more mature and supported by a venture-backed startup!

[Slides](https://github.com/ikding/foss2gna_2023/issues/6)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3344728/)

Apache Sedona is a cluster computing system designed to revolutionize the way we process large-scale spatial data. By extending the capabilities of existing systems such as Apache Spark and Apache Flink, Sedona provides a comprehensive set of out-of-the-box distributed Spatial Datasets and Spatial SQL that enable efficient loading, processing, and analysis of massive amounts of spatial data across multiple machines. With its ability to handle big data at scale, Sedona has the potential to transform industries such as transportation, logistics, and geolocation-based services. In 2020, Sedona joined the Apache Software Foundation, and in 2022, it was recognized as a top-level project, further solidifying its position as a leading technology in the big data space. Apache Sedona receives over 800K downloads per month and is listed among the top 1% most downloaded Python packages on PyPi.

In this presentation, we will delve into the key features of Apache Sedona and showcase its powerful capabilities in handling large-scale spatial data. Additionally, we will highlight the recent developments in Apache Sedona and how they have further enhanced the system's performance and scalability. We will also showcase examples of how Sedona has been used in various industries such as transportation, logistics, and geolocation-based services, to gain insights and improve decision-making.

Overall, this presentation will give you a comprehensive understanding of Apache Sedona and its capabilities, and how it can help you unlock the full potential of your spatial data.

</details>


## The Cloud-Native Geospatial Foundation

Jed Sundwall @ Radiant Earth

**TL;DR**: Radiant Earth is the parent organization behind Cloud-Native Geospatial Foundation. This talk goes through a great summary of what cloud native formats are, its current state, and what te organization is working on (e.g. development sprints, paid fellowship for software developers, etc).

More references can be found on the organization's website: https://cloudnativegeo.org

[Slides](https://github.com/ikding/foss2gna_2023/issues/7)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3297659/)

Join us for a presentation that explores how cloud-native data formats such as Cloud-Optimized GeoTIFF (COG), GeoParquet, and Spatiotemporal Asset Catalog (STAC) can enable efficient access and analysis of geospatial data over the Internet. By leveraging generic HTTP and hosting data in cloud object storage services, organizations can make geospatial datasets available to a wider array of users and applications. This session will showcase the benefits, implementation strategies, and real-world use cases of cloud-native geospatial data, empowering attendees to harness its transformative capabilities.

During this session, we will explore the foundational principles behind cloud-native geospatial data formats. Attendees will gain a deep understanding of how these formats, including Cloud-Optimized GeoTIFF (COG), GeoParquet, and Spatiotemporal Asset Catalog (STAC), are designed to optimize data transfer and enable efficient retrieval of specific data subsets using HTTP range requests. By leveraging cloud object storage services and RESTful/HTTP protocols, these formats ensure scalable and performant access to large volumes of geospatial data. We will also provide an overview of community research that we have performed to learn the needs of cloud-native geospatial data users.

Real-world use cases will be presented to demonstrate the benefits of cloud-native geospatial data using popular open source tools. Attendees will witness firsthand how organizations have leveraged cloud-native formats to overcome challenges related to data volume, access, and processing. From interactive web mapping applications to large-scale data analysis, the session will highlight the many benefits of cloud-native data structures in enabling faster and more efficient geospatial workflows.

Furthermore, the presentation will shed light on the broader implications and benefits of adopting cloud-native geospatial data formats. Attendees will gain insights into the collaboration possibilities that arise from hosting data in the cloud, enabling seamless sharing and integration across projects and organizations. We will discuss the cost-efficiency of cloud-native data formats, as they eliminate the need for data duplication and enable on-demand access, reducing storage costs and improving scalability.

</details>


## Seeing Everything: Digital Cartography and Strategies for Visualizing Large Datasets

Brad Andrick @ Element 84

**TL;DR**: this was a great intro-level talk, aimed at providing high-level overview of strategies for visualizing large geospatial datasets. The speaker outlined two paths and relevant solutions. I have to admit that the path 2 is a bit over my head, since I have absolutely no front-end experience.

Path 1: Why you shouldn't try to see everything

* Scale visibility
* Simplification
* Clustering
* Icon Overlap
* Heatmaps
* Aggregations

Path 2: If you still want to try to see everything

* Canvas
* WebGL
* PostGIS + mvt
* static tiles
* server side maps

[Slides](https://github.com/ikding/foss2gna_2023/issues/8)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3297672/)

Datasets today can be big, sometimes really big. Visualizing data at these large scales introduces unique challenges - particularly when a user is asking to see all of that data at once. The discussion presented here will address two aspects of the problem: technical limitations of rendering and the limited ability to derive useful insights from looking at such high data volumes on a map. The first part of the talk will discuss map visualization techniques including scale visibility, feature simplification, clustering, icon overlap, heatmaps, and aggregations. The second part will present an overview of several map rendering libraries and tooling combinations showing each one's ability and limitations to render increasingly large datasets. These will include canvas based renderers (leaflet), webGL based renderers (maplibre, openLayers), dynamic vector tiles with postGIS + mvt queries, and heavy DB + vega server side rendering. Example applications will be shown for each to demonstrate their capabilities ranging from rendering thousands of features up to millions.

</details>


## GeoServer Used in Fun and Interesting Ways

Jody Garnett @ OSGeo; Andrea Aime @ GeoSolutions

**TL;DR**: The talk is way over my head since I have no experience with GeoServer or the problem it solves. Including the slide here in case it is helpful for others.

[Slides](https://github.com/ikding/foss2gna_2023/issues/9)

<details>

<summary>Abstract</summary>

[Abstract](https://whova.com/embedded/session/o1YpY82NroS%40g96BXSdjSFqD1guuzvqH%40nHM-IGs2hA%3D/3297673/)

GeoServer is the start of so many great open source success stories.

This talk introduces the core GeoServer application and explores the ecosystem that has developed around this beloved OSGeo application. Our presentation draws on the GeoServer ecosystem for use-cases and examples of how the application has been used successfully by a wide range of organizations.

Each use-case highlights a capability of GeoServer providing an overview of the technology drawn from practical examples.

* Andrea Aime is on hand to share success stories highlighting GeoServer use in managing vulnerable ecosystems, agriculture information management, and marine data management.
* Jody Garnett will look at how GeoServer technology powers cloud services
* Gabriel will look at am amazing remixes for Cloud Native GeoServer
* GeoServer technology powering the OSGeo community, including GeoNode, geOrchestra
* A showcase of examples collected from our user list

Attend this talk to learn what GeoServer is good for out-of-the-box, and for inspiration on what is possible using GeoServer and the FOSS4G community.

</details>
