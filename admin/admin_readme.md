# Admin Set Up as of 5.10

**I haven't tested this list specifically, please do so and update - Bryan**

You need to go through the Plaform Configuration steps below and update your fork of the main readme.
**YOU SHOULD NOT NEED TO CHANGE ANY CODE IN THE TEMPLATE**

## Domino Platform Configuration

### Prereqs
- [ ] **Create a Branch or Fork of this Repository**
  - You will use the newly created fork/branch going forward. 
- [ ] Create a new Domino Environment in FleetCommand.
- [ ] Create a new GPU API hardware tier (clone the GPU Small one and set it to be an API tier).
- [ ] Disable the large GPU tier - it doesn't always start up for some as yet unknown reason.
- [ ] Collect some customer/use case specific documents (saved as PDFs).
  - These will be used to pupulate the RAG and provide context.
- [ ] Create an S3 bucket, upload documents to it and create a user/key in AWS that can access only this bucket.
- [ ] Add the S3 bucket to your Domino as a Data Source called: **ddl-rag-workshop**
  - Create data source using a service account and make it readable by everyone
- [ ] Find a banner customer logo image for the chat symbol for your app.
  - Make sure these are somewhere publically accessable, either directly from the customer website or in the poctemppublic s3 bucket in us-west-2 in Internal Eval AWS that is set to be publically accessable.
- [ ] make a copy of [this presentation](https://docs.google.com/presentation/d/1DnDZ6xE7suPLLvFeL6vrZp0eDV5OEG4yFprLkalMUYg/edit#slide=id.p) and tailor for your needs. Needs updating to the latest colour scheme.     


### Domino Central Configuration for 5.10
Create/Modify/Verify the following Central Config Entries:
- [ ] Set `com.cerebro.domino.workbench.project.defaultVolumeSizeGiB` to **`20`**
- [ ] Ensure `com.cerebro.domino.workbench.project.projectTemplateHubEnabled` is set to **`true`**
- [ ] Set `com.cerebro.domino.workbench.project.projectTemplateHubRaw` to the following.
  - Note: change the date if you want it to appear at the top of the list.

```json
[
    {
        "title": "Open Source Enterprise Q&A over your docs",
        "description": "This template shows how to use Meta's Llama2 to do Q&A over information that Llama2 models have not been trained on and will not be able to provide answers out of the box. By using open source models instead of third party services it allows you to maintain all our data and IP within the Domino platform.",
        "base64Logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTIwIiBoZWlnaHQ9IjEwMCIgdmlld0JveD0iMCAwIDEyMCAxMDAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHg9IjIzLjQxNDYiIHk9Ijc2LjQ3MDYiIHdpZHRoPSI4NC44NzgiIGhlaWdodD0iNS44ODIzNSIgZmlsbD0iI0I4RUZCOCIvPgo8cGF0aCBkPSJNMTIuNjIwOCA3NS4xNjI5TDEwLjI4NDQgODEuOTQxMkg4Ljg3MThMMTEuODE1MiA3NC4xMjA5SDEyLjcxNzVMMTIuNjIwOCA3NS4xNjI5Wk0xNC41NzU5IDgxLjk0MTJMMTIuMjM0MSA3NS4xNjI5TDEyLjEzMiA3NC4xMjA5SDEzLjAzOThMMTUuOTkzOSA4MS45NDEySDE0LjU3NTlaTTE0LjQ2MzEgNzkuMDQwOFY4MC4xMDk2SDEwLjIwOTJWNzkuMDQwOEgxNC40NjMxWk0xNi43NTY2IDgxLjI5NjZDMTYuNzU2NiA4MS4wOTYxIDE2LjgyNDYgODAuOTI3OCAxNi45NjA3IDgwLjc5MThDMTcuMDk2NyA4MC42NTIxIDE3LjI4MTEgODAuNTgyMyAxNy41MTM5IDgwLjU4MjNDMTcuNzUwMiA4MC41ODIzIDE3LjkzNDYgODAuNjUyMSAxOC4wNjcxIDgwLjc5MThDMTguMjAzMiA4MC45Mjc4IDE4LjI3MTIgODEuMDk2MSAxOC4yNzEyIDgxLjI5NjZDMTguMjcxMiA4MS40OTcyIDE4LjIwMzIgODEuNjY1NSAxOC4wNjcxIDgxLjgwMTVDMTcuOTM0NiA4MS45Mzc2IDE3Ljc1MDIgODIuMDA1NiAxNy41MTM5IDgyLjAwNTZDMTcuMjgxMSA4Mi4wMDU2IDE3LjA5NjcgODEuOTM3NiAxNi45NjA3IDgxLjgwMTVDMTYuODI0NiA4MS42NjU1IDE2Ljc1NjYgODEuNDk3MiAxNi43NTY2IDgxLjI5NjZaTTE2Ljc2MTkgNzYuNzIwNUMxNi43NjE5IDc2LjUyIDE2LjgzIDc2LjM1MTcgMTYuOTY2IDc2LjIxNTZDMTcuMTAyMSA3Ni4wNzU5IDE3LjI4NjUgNzYuMDA2MSAxNy41MTkzIDc2LjAwNjFDMTcuNzU1NiA3Ni4wMDYxIDE3Ljk0IDc2LjA3NTkgMTguMDcyNSA3Ni4yMTU2QzE4LjIwODUgNzYuMzUxNyAxOC4yNzY2IDc2LjUyIDE4LjI3NjYgNzYuNzIwNUMxOC4yNzY2IDc2LjkyMSAxOC4yMDg1IDc3LjA4OTMgMTguMDcyNSA3Ny4yMjU0QzE3Ljk0IDc3LjM2MTQgMTcuNzU1NiA3Ny40Mjk1IDE3LjUxOTMgNzcuNDI5NUMxNy4yODY1IDc3LjQyOTUgMTcuMTAyMSA3Ny4zNjE0IDE2Ljk2NiA3Ny4yMjU0QzE2LjgzIDc3LjA4OTMgMTYuNzYxOSA3Ni45MjEgMTYuNzYxOSA3Ni43MjA1WiIgZmlsbD0iIzMzMzMzMyIvPgo8cmVjdCB4PSIyMy40MTQ2IiB5PSI1OC44MjM1IiB3aWR0aD0iNTguNTM2NiIgaGVpZ2h0PSI1Ljg4MjM1IiBmaWxsPSIjNjc4MEZGIi8+CjxwYXRoIGQ9Ik0xMy43MTY1IDYzLjI1MjFMMTUuNzczNyA2NC44ODQ5TDE0Ljg5ODIgNjUuNjUzTDEyLjg3ODYgNjQuMDM2M0wxMy43MTY1IDYzLjI1MjFaTTE1Ljc4OTggNjAuMTY5MVY2MC41OTg4QzE1Ljc4OTggNjEuMTg5NiAxNS43MTI4IDYxLjcxOTYgMTUuNTU4OCA2Mi4xODg2QzE1LjQwNDggNjIuNjU3NyAxNS4xODQ2IDYzLjA1NyAxNC44OTgyIDYzLjM4NjRDMTQuNjExNyA2My43MTU4IDE0LjI2OTcgNjMuOTY4MyAxMy44NzIzIDY0LjE0MzdDMTMuNDc0OCA2NC4zMTU2IDEzLjAzNDQgNjQuNDAxNSAxMi41NTEgNjQuNDAxNUMxMi4wNzEyIDY0LjQwMTUgMTEuNjMyNSA2NC4zMTU2IDExLjIzNTEgNjQuMTQzN0MxMC44NDEyIDYzLjk2ODMgMTAuNDk5MiA2My43MTU4IDEwLjIwOTIgNjMuMzg2NEM5LjkxOTE2IDYzLjA1NyA5LjY5MzU3IDYyLjY1NzcgOS41MzI0NCA2Mi4xODg2QzkuMzc0ODkgNjEuNzE5NiA5LjI5NjExIDYxLjE4OTYgOS4yOTYxMSA2MC41OTg4VjYwLjE2OTFDOS4yOTYxMSA1OS41NzgzIDkuMzc0ODkgNTkuMDUwMSA5LjUzMjQ0IDU4LjU4NDZDOS42ODk5OSA1OC4xMTU2IDkuOTEyIDU3LjcxNjMgMTAuMTk4NSA1Ny4zODY5QzEwLjQ4ODUgNTcuMDUzOSAxMC44MzA1IDU2LjgwMTQgMTEuMjI0MyA1Ni42Mjk2QzExLjYyMTggNTYuNDU0MSAxMi4wNjA0IDU2LjM2NjQgMTIuNTQwMyA1Ni4zNjY0QzEzLjAyMzcgNTYuMzY2NCAxMy40NjQxIDU2LjQ1NDEgMTMuODYxNSA1Ni42Mjk2QzE0LjI2MjYgNTYuODAxNCAxNC42MDYzIDU3LjA1MzkgMTQuODkyOCA1Ny4zODY5QzE1LjE3OTMgNTcuNzE2MyAxNS4zOTk1IDU4LjExNTYgMTUuNTUzNCA1OC41ODQ2QzE1LjcxMSA1OS4wNTAxIDE1Ljc4OTggNTkuNTc4MyAxNS43ODk4IDYwLjE2OTFaTTE0LjQzNjIgNjAuNTk4OFY2MC4xNTg0QzE0LjQzNjIgNTkuNzIxNSAxNC4zOTMzIDU5LjMzNjYgMTQuMzA3MyA1OS4wMDM2QzE0LjIyNSA1OC42NjcgMTQuMTAxNSA1OC4zODU5IDEzLjkzNjcgNTguMTYwM0MxMy43NzU2IDU3LjkzMTIgMTMuNTc2OSA1Ny43NTkzIDEzLjM0MDUgNTcuNjQ0N0MxMy4xMDc4IDU3LjUyNjUgMTIuODQxIDU3LjQ2NzUgMTIuNTQwMyA1Ny40Njc1QzEyLjI0NjYgNTcuNDY3NSAxMS45ODM0IDU3LjUyNjUgMTEuNzUwNyA1Ny42NDQ3QzExLjUxOCA1Ny43NTkzIDExLjMxOTIgNTcuOTMxMiAxMS4xNTQ1IDU4LjE2MDNDMTAuOTg5OCA1OC4zODU5IDEwLjg2NDUgNTguNjY3IDEwLjc3ODUgNTkuMDAzNkMxMC42OTI2IDU5LjMzNjYgMTAuNjQ5NiA1OS43MjE1IDEwLjY0OTYgNjAuMTU4NFY2MC41OTg4QzEwLjY0OTYgNjEuMDM1NiAxMC42OTI2IDYxLjQyMjQgMTAuNzc4NSA2MS43NTlDMTAuODY0NSA2Mi4wOTU1IDEwLjk4OTggNjIuMzgwMiAxMS4xNTQ1IDYyLjYxM0MxMS4zMjI4IDYyLjg0MjEgMTEuNTIzMyA2My4wMTU4IDExLjc1NjEgNjMuMTM0QzExLjk5MjQgNjMuMjQ4NSAxMi4yNTc0IDYzLjMwNTggMTIuNTUxIDYzLjMwNThDMTIuODUxOCA2My4zMDU4IDEzLjExODUgNjMuMjQ4NSAxMy4zNTEzIDYzLjEzNEMxMy41ODQgNjMuMDE1OCAxMy43ODEgNjIuODQyMSAxMy45NDIxIDYyLjYxM0MxNC4xMDMyIDYyLjM4MDIgMTQuMjI1IDYyLjA5NTUgMTQuMzA3MyA2MS43NTlDMTQuMzkzMyA2MS40MjI0IDE0LjQzNjIgNjEuMDM1NiAxNC40MzYyIDYwLjU5ODhaTTE3LjA0NjYgNjMuNjQ5NkMxNy4wNDY2IDYzLjQ0OTEgMTcuMTE0NiA2My4yODA4IDE3LjI1MDcgNjMuMTQ0N0MxNy4zODY4IDYzLjAwNTEgMTcuNTcxMiA2Mi45MzUyIDE3LjgwMzkgNjIuOTM1MkMxOC4wNDAzIDYyLjkzNTIgMTguMjI0NyA2My4wMDUxIDE4LjM1NzEgNjMuMTQ0N0MxOC40OTMyIDYzLjI4MDggMTguNTYxMiA2My40NDkxIDE4LjU2MTIgNjMuNjQ5NkMxOC41NjEyIDYzLjg1MDEgMTguNDkzMiA2NC4wMTg0IDE4LjM1NzEgNjQuMTU0NUMxOC4yMjQ3IDY0LjI5MDUgMTguMDQwMyA2NC4zNTg2IDE3LjgwMzkgNjQuMzU4NkMxNy41NzEyIDY0LjM1ODYgMTcuMzg2OCA2NC4yOTA1IDE3LjI1MDcgNjQuMTU0NUMxNy4xMTQ2IDY0LjAxODQgMTcuMDQ2NiA2My44NTAxIDE3LjA0NjYgNjMuNjQ5NlpNMTcuMDUyIDU5LjA3MzRDMTcuMDUyIDU4Ljg3MjkgMTcuMTIgNTguNzA0NiAxNy4yNTYxIDU4LjU2ODVDMTcuMzkyMSA1OC40Mjg5IDE3LjU3NjUgNTguMzU5MSAxNy44MDkzIDU4LjM1OTFDMTguMDQ1NiA1OC4zNTkxIDE4LjIzIDU4LjQyODkgMTguMzYyNSA1OC41Njg1QzE4LjQ5ODYgNTguNzA0NiAxOC41NjY2IDU4Ljg3MjkgMTguNTY2NiA1OS4wNzM0QzE4LjU2NjYgNTkuMjczOSAxOC40OTg2IDU5LjQ0MjIgMTguMzYyNSA1OS41NzgzQzE4LjIzIDU5LjcxNDQgMTguMDQ1NiA1OS43ODI0IDE3LjgwOTMgNTkuNzgyNEMxNy41NzY1IDU5Ljc4MjQgMTcuMzkyMSA1OS43MTQ0IDE3LjI1NjEgNTkuNTc4M0MxNy4xMiA1OS40NDIyIDE3LjA1MiA1OS4yNzM5IDE3LjA1MiA1OS4wNzM0WiIgZmlsbD0iIzMzMzMzMyIvPgo8cmVjdCB4PSIyMy45MTQ2IiB5PSIxMy43MzUzIiB3aWR0aD0iMjUuMzQxNSIgaGVpZ2h0PSIzMS4zNTI5IiBmaWxsPSJ3aGl0ZSIgc3Ryb2tlPSIjRDBEMEQwIi8+CjxyZWN0IHg9IjI3LjgwNDkiIHk9IjI1IiB3aWR0aD0iMTcuNTYxIiBoZWlnaHQ9IjEuNDcwNTkiIGZpbGw9IiNEMEQwRDAiLz4KPHJlY3QgeD0iMjcuODA0OSIgeT0iMjAuNTg4MiIgd2lkdGg9IjUuODUzNjYiIGhlaWdodD0iMS40NzA1OSIgZmlsbD0iI0QwRDBEMCIvPgo8cmVjdCB4PSIyNy44MDQ5IiB5PSIyOS40MTE4IiB3aWR0aD0iMTcuNTYxIiBoZWlnaHQ9IjEuNDcwNTkiIGZpbGw9IiNEMEQwRDAiLz4KPHJlY3QgeD0iMjcuODA0OSIgeT0iMzMuODIzNSIgd2lkdGg9IjEzLjE3MDciIGhlaWdodD0iMS40NzA1OSIgZmlsbD0iI0QwRDBEMCIvPgo8L3N2Zz4K",
        "categories": [
            "Generative AI",
            "Q&A",
            "LLM",
            "RAG"
        ],
        "mainRepository": {
            "uri": "https://github.com/dominodatalab/reference-project-local-customqa",
            "ref": {
                "type": "head",
                "value": "refs/heads"
            },
            "serviceProvider": "github"
        },
        "created": "2024-02-15T01:00:51.00Z",
        "owner": {
            "name": "Domino",
            "link": "https://domino.ai"
        },
        "models": [
            {
                "name": "llama-2-7b",
                "link": "https://huggingface.co/NousResearch/Nous-Hermes-llama-2-7b",
                "size": null,
                "source": null
            }
        ],
        "license": {
            "name": "Apache 2.0",
            "link": "https://github.com/dominodatalab/reference-project-local-customqa#license"
        },
        "data": {
            "name": "This template reads data from PDF files, examples of which are provided in the template source code",
            "link": "https://github.com/dominodatalab/reference-project-local-customqa/blob/main/sample_data/MLOps_whitepaper.pdf"
        },
        "dataFormat": "PDF",
        "recommended": true,
        "prerequisites": [
            {
                "value": "Before using this project, please set up an environment based on the Domino Standard Environment, with additional Dockerfile instructions",
                "link": "https://github.com/dominodatalab/reference-project-local-customqa#setup-instructions"
            }
        ],
        "goals": null,
        "hardwareTier": {
            "value": "small-k8s",
            "link": null
        },
        "dependencies":[
            {
               "templateName":"Open RAG Workshop",
               "created":"2024-04-29T21:30:25.00Z",
               "kind":"Environment"
            }
         ],
        "environmentReqs": null,
        "importedRepositories": null,
        "templateId": "675b7c68471e13aba55437e6bb6131d33d56d51f",
        "revisionId": "02fb4ac535e08ea35054bcc56ceb79e6a87fedaf"
    }
]
```

## Compute Environment (updated for 5.10)
Templates in 5.10 can have an environment as a dependency, they are linked by *templateName* and *created* in case you want to switch it out.
- [ ] Set `com.cerebro.domino.workbench.project.environmentTemplatesRaw` to the following:
```json
[
    {
        "title": "Open RAG Workshop",
        "description": "Workshop for running the Llama2 RAG workshop",
        "environmentBaseImage": "quay.io/domino/pre-release-environments:project-hub-gpu.main.latest",
        "dockerfile": "RUN pip install qdrant_client streamlit_chat pypdf\nRUN pip install --upgrade ipywidgets",
    "created": "2024-04-29T21:30:25.00Z",
        "workspaceTools": "jupyterlab:\n  title: \"JupyterLab\"\n  iconUrl: \"/assets/images/workspace-logos/jupyterlab.svg\"\n  start: [ \"/opt/domino/workspaces/jupyterlab/start\" ]\n  httpProxy:\n    internalPath: \"/{{ownerUsername}}/{{projectName}}/{{sessionPathComponent}}/{{runId}}/{{#if pathToOpen}}tree/{{pathToOpen}}{{/if}}\"\n    port: 8888\n    rewrite: false\n    requireSubdomain: false"
    }
]
```

## Readme Changes
In your recently created Branch or Fork, in the top-level `README.md` file, make the following value changes:
- [ ] Line 17 - add Domino URL
- [ ] Line 441 - Change the file `working_app.png`, to a screenshot of the application.
- [ ] Line 352 - Change the image to reflect the header image you have chosen.
- [ ] Line 356 - Change the image to reflect the header image you have chosen.
- [ ] Line 363 - Change the image to reflect the logo image you have chosen.
- [ ] Line 367 - Change the image to reflect the logo image you have chosen.
- [ ] Line 441 - Change the file `working_app.png`, to a screenshot of the application.

Do a "Find & Replace for the following values with values appropriate for your workshop:
- [ ] **Customer_Name** - e.g. *Acme Insurance*
- [ ] **Customer_Name_NoSpace** - e.g. *acme_insurance*
- [ ] **Use_Case** - a short form title of the use case e.g. *Car Policy*
- [ ] **Example_Question** - e.g. *What is my excess?*

## TODO:
Get added as an official template?

