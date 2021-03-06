
.. _datasets-km:

.. _odh-vkg:

The |odh| Virtual Knowledge Graph
---------------------------------

.. versionadded:: 2021.02 Description of the Knowledge Model
   underlying datasets Accommodation, Gastronomy, and Event datasets

Some datasets in the |odh|, namely :ref:`Accommodation
<accommodation-dataset>`, :ref:`Gastronomy <gastronomy-dataset>`, and
:ref:`Event <event-dataset>`, are organised into a `Virtual Knowledge
Graph` that can be accessed using SPARQL from the dedicated `SPARQL
endpoint <https://sparql.opendatahub.bz.it>`_. In order to define more
precise queries, this section describes the Knowledge Models (`KM`)
underlying these datasets; the description of each |km| is accompanied
by an UML diagram which shows the KM at a glance.


Besides standard W3C's OWL and RDF vocabularies, the |odh| VKG
uses:

* `schema.org <http://schema.org/>`_ for most of the entities used
* `geosparql <http://www.opengis.net/ont/geosparql#>`_ for
  geo-references and coordinates of objects
* `purl <http://purl.org/dc/terms/>`_ for linking to related resources

.. panels::

   Common Notation
   ^^^^^^^^^^^^^^^

   Diagrams use UML class diagram formalism widely adopted in
   Knowledge Representation and in particular in the `W3C's
   Recommendation` documents for the Semantic Web.  The following
   additional notation applies:

   Prefix
      The default prefix used for classes and properties is
      :strong:`http://schema.org/`. This means that, unless
      differently stated, the definition of classes and properties,
      including their attributes, rely on a common standard as defined
      in schema.org's vocabulary. As examples, see the
      `LodgingBusiness <http:schema.org//LodgingBusiness>`_ class and
      the `containedInPlace <https://schema.org/containedInPlace>`_
      property.

      .. hint:: Other prefixes are explicitly pre-pended to the Class
	 or Property name, like e.g., `noi:numberOfUnits`.

   Arrows
     Arrows with a white tip denote a `sub-class` relationship, while
     black tips denote `object properties`.

   Cardinality
     Cardinality of :strong:`1` is usually not shown, but implied; the
     `look across
     <https://www.quora.com/How-do-we-read-cardinality-in-a-UML-diagram-or-in-E-A-diagram>`_
     notation is used. For example, the image on the right-hand
     side--excerpt from the :ref:`event dataset <event-dataset-kg>`
     VKG--can be read as `0 to N` :strong:`MeetingRoom`\s are
     `ContainedInPlace` :strong:`Place`.


   ---

   .. image::  /images/sparql/cardinality.png

.. _accommodation-dataset-kg:

.. dropdown:: Accommodation Dataset

   .. panels::

      .. postalAddress has one attribute more in Event than in other
	 datasets.

      Central class in this dataset is :strong:`LodgingBusiness`, to
      which belong multiple :strong:`Accommodation`\s.

      A :strong:`LodgingBusiness` has as attributes `geo:asWKT`,
      `email`, `name`, `telephone`, and `faxNumber` and relations

      * `address` to class :strong:`PostalAddress`, which consists of
	`streetAddress`, `postalCode`, and `AddressLocality`
      * `geo`, i.e., a geographical location, to class
	:strong:`GeoCoordinates`, consisting of `latitude`,
	`longitude`,  and `elevation`

      There are (sub-)types of :strong:`LodgingBusiness`--called
      :strong:`Campground`, :strong:`Hotel`, :strong:`Hostel`, and
      :strong:`BedAndBreakfast`--sharing its attributes and relations.

      An :strong:`Accommodation` is identified by a `name` and a
      `noi:numberOfUnits` and has relations

      * `containedInPlace` to :strong:`LodgingBusiness` (multiple
	:strong:`Accommodation`\s can belong to it)
      * `occupancy` to :strong:`QuantitativeValue`, which gives the
	`maxValue` and `minValues` of available units of accommodation
	and a `unitCode`.

      +++

      `noi:numberOfUnits` is the number of available
      rooms, suites, apartments, etc. that are available in that
      :strong:`Accommodation`

      `geo:asWKT` is a method used by opengis.net's `geosparql
      <http://www.geosparql.org/>` to express geographic coordinates
      in a standard, textual form based on :abbr:`WKT (Well-known
      text)`.

      ---

      .. figure:: /images/sparql/odh-accommodation.png
	 :width: 100%

	 The UML diagram of the :ref:`Accommodation Dataset
	 <accommodation-dataset>`.


.. _gastronomy-dataset-kg:

.. dropdown:: Gastronomy Dataset

   .. panels::

      The main class of this dataset is :strong:`FoodEstablishment`,
      described by `geo:asWKT`, `description`, `name`, `telephone`,
      and `url`.

      A :strong:`FoodEstablishment` has

      * a :strong:`PostalAddress`--consisting of `streetAddress`,
	`postalCode`, and `AddressLocality`--as `address`
      * a :strong:`GeoCoordinates`--`latitude`, `longitude`, and
	`elevation`--as a geographical location `geo`

      There are different (sub-)\types of
      :strong:`FoodEstablishment`, all sharing the same attributes:
      :strong:`Restaurant`, :strong:`FastFoodRestaurant`,
      :strong:`BarOrPub`, :strong:`Winery`, and
      :strong:`IceCreamShop`.


      +++

      `geo:asWKT` is a method used by opengis.net's `geosparql
      <http://www.geosparql.org/>` to express geographic coordinates
      in a standard, textual form based on :abbr:`WKT (Well-known
      text)`.

      ---

       .. figure:: /images/sparql/odh-food-establishment.png
	  :width: 100%

	  The UML diagram of the :ref:`Gastronomy Dataset <gastronomy-dataset>`.

.. _event-dataset-kg:

.. dropdown:: Event Dataset

   .. panels::

      The main classe in this dataset is :strong:`Event`, described by
      a `startDate`, an `endDate`, and a `description`.  Every
      :strong:`Event` has an `organizer`, either a :strong:`Person` or
      an :strong:`Organization` and a `location`.

      A :strong:`Person`--identified by `givenName`, `familyName`,
      `email`, and `telephone`--`worksFor` an :strong:`Organization`,
      which has a `name` and an `address`, i.e., a
      :strong:`PostalAddress` consisting of `streetAddress`,
      `postalCode`, `AddressLocality`, and `AddressCountry`.

      Finally, an :strong:`Event` has as `location` a
      :strong:`MeetingRoom`--identified by a `name`-- which is
      `containedInPlace` a :strong:`Place`--which has also a `name`

      -----

      .. figure:: /images/sparql/odh-event.png
	 :width: 100%

	 The UML diagram of the :ref:`Event Dataset <event-dataset>`.

.. seealso::

   The :ref:`SPARQL howto <howto-sparql>`, which guides you in
   interacting with the SPARQL endpoint.

   W3C Recommendation for `OWL2
   <http://www.w3.org/TR/2012/REC-owl2-syntax-20121211/>`_ and `RDF
   <http://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/>`_.

   Official Specification of `UML Infrastructure
   <http://www.omg.org/spec/UML/2.1.2/Infrastructure/PDF/>`_ are
   available from `Object management group <https://www.omg.org/>`_
