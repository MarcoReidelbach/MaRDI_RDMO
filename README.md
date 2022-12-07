# MaRDI_RDMO

This repository contains a questionnaire and an Export/Query Plugin for the [Research Datamanagement Organizer](https://rdmorganiser.github.io/) developed within Task Area 4 "Interdisciplinary Mathematics" of the [Mathematical research Data Initiative](https://www.mardi4nfdi.de/about/mission) (MaRDI). 

The questionnaire allows a standardized documentation of interdisciplinary workflows related to mathematics, where the connection to "real" experiments as well as to computational simulations is possible and also desired.

The Export/Query Plugin allows the user to export his documented workflow into a standardized Markdown template. It also offers the possibility to publish the documented workflow directly on the [MaRDI Portal](https://portal.mardi4nfdi.de/wiki/Portal)* and to integrate it into the knowledge graph there. The Query App in turn allows the user to search the workflows stored in the portal based on individual components. 

The functionality of the Export/Query App as well as the export method are captured in the question catalog, so that a single button controls everything. 

*For the time being, there is no connection to the MaRDI Knowledge Graph, so a local Knowledge Graph will be used for testing. The three searchable components are Research Objective, Methods and Input Data.

## Repository structure

 -catalog - all files necessary for RDMO questionnaire <br>
  --questions.xml - question-/help-text, relation to domain, options, condition <br>
  --domains.xml - individual domains <br>
  --options.xml - individual options <br>
  --conditions.xml - individual conditions
  
 -func - all files necessary for the Export/Query Plugin <br>
  --export.py - contains Export/Query function (based on RDMO's csv export) <br>
  --para.py - contains question IDs for proper export and query
  
 -kg - all files necessary for the local knowledge graph <br>
  --MaRDI_RDMO.owl - owl based knowledge graph for local workflow exports and queries
 
  
## Installation and Setup

To install and configure RDMO follow their [instructions](https://rdmo.readthedocs.io/en/latest/installation/index.html). Once your RDMO instance is ready make sure that 'numpy (<1.23.0)`, `scikit-learn`, `owlready2` and `nltk` are present. If not install them. These four packages are required to interact with the local knowledge graph, could change once the MaRDI portal is connected.

The mardi_workflow directory has to be placed in the rdmo-app directory. To connect the Plugin with RDMO include 

```python
from django.utils.translation import ugettext_lazy as _  
from . import BASE_DIR, INSTALLED_APPS, PROJECT_EXPORTS
```
at the top of RDMO's local.py file. Furthermore, add

```python
INSTALLED_APPS = ['mardi_workflow'] + INSTALLED_APPS

PROJECT_EXPORTS += [
        ('mde', _('MaRDI Export/Query'), 'mardi_workflow.func.export.MaRDIExport'),
        ]
```

at an arbitrary place in local.py. Thereby, the Export/Query Plugin is connected to RDMO and an `MaRDI Export/Query` Butoon is established. 

Once, the Export/Query Plugin is connected run RDMO and subsequently upload the catalog files e.g. via `Managment/Domain/Import`. Make sure to upload domains.xml first and questions.xml last.

Now, everything is ready to use the MaRDI questionnaire. Therefore, click on `Create new Project`, choose a project name (the name chosen here will also be the name used to identify you workflow) and assign the MaRDI questionnaire. The first question is aimed at the Export/Query Plugin operation modus. If you choose Workflow Finding you will be asked to state and describe the component by which you would like to search the knowledge graph. Once, filled out and saved, return to the project page and click on `MaRDI Export/Query` and an appropriate workflow will be displayed. If you choose Workflow documentation you have to answer several detalied questions about your workflow. Make sure to read the instructions carefully, especially the set numbering (0,1,2,...). Once, all questions are answered you can decide if you would like to download your documentation as Markdown File or add it to the MaRDI Portal (aka local knowledge graph). As before, return to the project page and click on `MaRDI Export/Query`.






