.. include:: ../../Includes.txt


.. _configuration:

=============
Configuration
=============

A primary feature of TYPO3 is its configurability. Not only can
it be configured by users with special user privileges in the backend.
Most configuration can also be changed by extensions or
configuration files. Additionally, configuration can be extended by
extensions.

This page will give you an overview of various configuration files and
:ref:`configuration methods <classification-config-methods>` in TYPO3.
For more extensive information we will refer you to the
respective chapter or reference.

**Table of Contents**

The arrow `➜` indicates that
you will be directed to another manual or to a different chapter in "TYPO3
Explained".

.. toctree::
   :maxdepth: 1

   Glossary

* :ref:`Caching ➜ <caching-configuration>`
* :ref:`Extension configuration ➜ <extension-configuration>`

.. toctree::
   :maxdepth: 1

   ../FeatureToggles/Index

* :ref:`Flexforms ➜ <flexforms>`
* :ref:`Form Framework ➜ <form:concepts-configuration>`

.. toctree::
   :maxdepth: 1

   ../GlobalValues/GlobalVariables/Index
   ../GlobalValues/Typo3ConfVars/Index

* :ref:`Logging ➜ <logging-configuration>`
* :ref:`Rich text configuration with rte_ckeditor ➜ <ckedit:configuration>`
* :ref:`Site, language, routing configuration ➜ <sitehandling>`
* :ref:`TCA Reference ➜ <t3tca:start>`

.. toctree::
   :maxdepth: 1


   ../Tsconfig/Index
   ../TypoScriptSyntax/Index

* :ref:`TypoScript Templating Reference ➜ <t3tsref:start>`

.. toctree::
   :maxdepth: 1

   ../UserSettingsConfiguration/Index
   ../Yaml/Index
   ../YamlSyntax/Index





.. _config-overview:

Configuration overview: files
=============================


Global files
------------

:file:`<webroot>/typo3conf/LocalConfiguration.php`:
   Contains the persisted :ref:`$GLOBALS['TYPO3_CONF_VARS'] <typo3ConfVars>` array.
   Settings configured in the backend by system maintainers in
   :guilabel:`ADMIN TOOLS > Settings > Configure Installation-Wide Options`
   are written to this file.

:file:`<webroot>/typo3conf/AdditionalConfiguration.php`:
   Cam be used to **override** settings defined in :file:`LocalConfiguration.php`

:file:`config/sites/<site>/config.yaml`
   This file is located in :file:`webroot/typo3conf/sites` in non-Composer installations.
   The Site configuration configured in the :guilabel:`SITE MANAGEMENT > Sites`
   backend module is written to this file.

Extension files
---------------

:ref:`composer.json <composer-json>`
   Composer configuration

:ref:`ext_emconf.php <extension-declaration>`
   (required) Extension declaration

:ref:`ext_tables.php <extension-configuration-files>`
   Various configuration. Is used only for backend or CLI requests or when a valid BE user is authenticated.

:ref:`ext_localconf.php <extension-configuration-files>`
   Various configuration. Is always included, whether frontend or backend.

:ref:`ext_conf_template.txt <extension-configuration>`
   Define the "Extension Configuration" settings that can be changed in the backend.

:file:`Configuration/Services.yaml`
   Can be used to configure :ref:`Event listeners <EventDispatcher>` and
   :ref:`Dependency injection <DependencyInjection>`

:file:`Configuration/TCA`
   TCA configuration.

:file:`Configuration/TSconfig/`
   TSconfig configuration.

:file:`Configuration/TypoScript/`
   TypoScript configuration.


The files are explained in more depth in:

* :ref:`Extension configuration files <extension-files-locations>`



Configuration overview: methods
===============================


:ref:`TSconfig <t3tsconfig:start>`
----------------------------------

While `Frontend TypoScript` is used to steer the rendering of the frontend, `TSconfig` is used
to configure backend details for backend users. Using `TSconfig` it is possible to enable or
disable certain views, change the editing interfaces, and much more. All that without coding a single
line of PHP. `TSconfig` can be set on a page (Page TSconfig), as well as a user / group (User TSconfig)
basis.

`TSconfig` uses the same syntax as `Frontend TypoScript`, the syntax is outlined in detail
in :ref:`typoscript-syntax-start`. Other than that, TSconfig and Frontend TypoScript
don't have much more in common - they consist of entirely different properties.

A full reference of properties as well as an introduction to explain details configuration usage, API and
load orders can be found in the :ref:`TSconfig Reference document <t3tsconfig:start>`. While Developers
should have an eye on this document, it is mostly used as a reference for Integrators who make life as
easy as possible for backend users.



:ref:`TypoScript Templating <t3tsref:start>`
--------------------------------------------

`TypoScript` - or more precisely `Frontend TypoScript` - is used in TYPO3 to steer
the frontend rendering (the actual website) of a TYPO3 instance. It is based on the
TypoScript syntax which is outlined in detail in :ref:`typoscript-syntax-start`.

Frontend TypoScript is very powerful and has been the backbone of frontend rendering ever since.
However, with the rise of the Fluid templating engine, many parts of Frontend TypoScript are much less
often used. Nowadays, TypoScript in real life projects is often not much more than a way to
set a series of options for plugins, to set some global config options, and to act as a simple
pre processor between database data and Fluid templates.

Still, the :ref:`TypoScript Reference <t3tsref:start>` reference document that goes deep into
the incredible power of Frontend TypoScript is daily bread for Integrators.

.. seealso::

* :ref:`t3ts45:start` - for an introduction to TypoScript templating
* :ref:`t3sitepackage:start` - start a sitepackage extension to creat a theme
  for your site using TypoScript and Fluid



:ref:`PHP $GLOBALS <globals-variables>`
---------------------------------------

.. code-block:: none

   $GLOBALS
   ├── $GLOBALS['TCA'] = "TCA"
   ├── GLOBALS['TYPO3_CONF_VARS'] = "Global configuration"
   │   ├── GLOBALS['TYPO3_CONF_VARS']['EXTENSIONS'] = "Extension configuration"
   │   └── GLOBALS['TYPO3_CONF_VARS']['SYS'][']features'] = "Feature Toggles"
   └── $GLOBALS['TYPO3_USER_SETTINGS'] = "User settings"
   └── ...

The :php:`$GLOBALS` array consists of:

:ref:`$GLOBALS['TYPO3_CONF_VARS'] <typo3ConfVars>`:
   is used for system wide configuration.

:ref:`Extension Configuration <extension-options>`:
   is a subset of :php:`$GLOBALS['TYPO3_CONF_VARS']`.
   It is stored in :php:`$GLOBALS['TYPO3_CONF_VARS']['EXTENSIONS']`.
   It is used for configuration specific to one extension and can
   be modified in the backend :guilabel:`ADMIN TOOLS > Settings > Extension
   Configuration`.

:ref:`TCA <t3tca:start>`:
   TCA is the backbone of database tables displayed in the backend, it configures
   how data is stored if editing records in the backend, how fields are displayed,
   relations to other tables and much more. It is a huge array loaded in almost all
   access contexts. TCA is documented in the :ref:`TCA Reference <t3tca:start>`.
   Next to a small introduction, the document forms a complete reference of all
   different TCA options, with bells and whistles. The document is a must-read for
   Developers, partially for Integrators, and is often used as a reference book on a daily basis.

:ref:`feature-toggles`:
   are used to switch a specific functionality of
   TYPO3 on or off. The values are written to
   :php:`GLOBALS['TYPO3_CONF_VARS']['SYS'][']features']`.
   The feature toggles can be switched on or off in the backend
   :guilabel:`ADMIN TOOLS > Settings > Feature Toggles` with **Admin**
   privileges.

:ref:`User settings <user-settings>`:
   :php:`$GLOBALS['TYPO3_USER_SETTINGS']` defines configuration for backend users

This is not a complete list!

You can find more and view the configuration in the TYPO3 backend
:guilabel:`SYSTEM > Configuration` (read only) or by viewing the
:php:`$GLOBALS` array in a debugger.

:php:`$GLOBALS['TYPO3_CONF_VARS']`, Extension configuration and feature toggles
can be changed in the backend in :guilabel:`ADMIN TOOLS > Settings` by
system maintainers. TCA cannot be modified in the backend.
Configuration of the :ref:`Logging Framework <logging-configuration>` and
:ref:`Caching Framework <caching-configuration>` - while being a part of the
:php:`$GLOBALS['TYPO3_CONF_VARS']` array - can also not be changed in the
backend. They must be modified in the file :file:`typo3conf/AdditionalConfiguration.php`.


:ref:`Flexform <flexforms>`
---------------------------

Flexforms are used to configure plugins and content elements.
With Flexforms, every content element can be configured different.

Flexform values can be changed while editing content elements in the backend.
It is one of the few settings that can be changed by editors without admin
privileges.

A schema defining the values that can be changed in the Flexform is
specified in the extension which supplies the plugin or content element.


YAML
----

Some system extensions use YAML for configuration:

* :ref:`Site <sitehandling>` configuration is stored in :file:`<project-root>/config/sites/<identifier>/config.yaml`.
  It can be configured in the backend module "Site" or changed directly in
  the configuration file.
* :ref:`form <form:concepts-configuration>`: The Form engine is a system
  extension which supplies Forms to use in the frontend
* :ref:`rte_ckeditor <ckedit:configuration>`: RTE ckeditor is a system
  extension. It is used to enable rich text editing in the backend.
* A file :file:`<extension>/Configuration/Services.yaml` can be used to configure
  :ref:`Event listeners <EventDispatcher>` and :ref:`Dependency injection <DependencyInjection>`

There is a :ref:`YamlFileLoader <yamlFileLoader>` which can be used to load YAML
files.







