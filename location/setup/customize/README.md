# Customize: make Ollama an external pipeline

New way to [omit Ollama](https://github.com/open-webui/open-webui/discussions/1287) by setting `ENABLE_OLLAMA_API=False`

	docker run -d -p 3000:8080 -e ENABLE_OLLAMA_API=False -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main


TO DO: Test the above and update our [Google Cloud Notes](../google-cloud/)

The above make make all of the following unnecessary.

## Remove Ollama / Reactivate Ollama for Syncing

[OpenWebUI](https://github.com/open-webui/open-webui) uses pipelines to connect to hundreds of LLMs, but the default UX doesn't use a pipeline to connect to the Ollama API  itself. With our modifications, the cost on Google Cloud will hopefully drop from $78/mo to less than $40/mo by connecting to Ollama as an external API pipeline.

The python process below adds/removes the Ollama install within OpenWebUI so we can use an external Ollama pipeline API to reduce hosting costs.

<!--
OpenWebUI is a potential starting point for any AI interface. One of our goals is to wrap our JQuery+React UX around the python OpenWebUI chatbot tools, while integrating Google Data Commons for real-world analytics data, timelines and team tools integrated with the Discord API.
-->

[To see the differences](https://github.com/open-webui/open-webui/compare/main...datascape:open-webui:main), click the "Files change" link here.


These scripts are developed in [github.com/ModelEarth/projects](https://github.com/ModelEarth/projects) (which is a fork of open-webui)

https://github.com/ModelEarth/projects-delete/pull/7

The files included in file-lines.py to make Ollama external for Google Cloud are listed in the following Pull Request (PR): [github.com/ModelEarth/projects-delete/pull/7](https://github.com/ModelEarth/projects-delete/pull/7)

## Customize.py

We comment out Ollama to reduce costs when deploying Docker to Google Cloud.

Update the "customize.py" script to do the reverse of the "sync.py" script.

- If the env block doesn't already exist in cypress.config.ts, it will be added. Otherwise, it will be skipped to avoid duplication.
- Similarly, chat.cy.ts will have the beforeEach code that includes code for Ollama lines.

Run to deactivate Ollama so you can use an external pipeline instead:

	python projects/location/setup/customize/customize.py "open-webui"


## Sync.py

Run in the root of your website to reactivate Ollama prior to syncing.

The following reinstates Ollama so we can sync from [open-webui](https://github.com/open-webui/open-webui).

You can make a fork of our: [github.com/datascape/open-webui](https://github.com/datascape/open-webui)

Pull your fork locally, then run in your webroot to update the open-webui repo:

	python projects/location/setup/customize/sync.py "open-webui"

Click "commits ahead of"
Click "Files changed"

Removes comments we added to deactivate Ollama. 
Files and lines updated are in file-lines.py (which will also be used by customize.py to add comments).

Sync.py first checks if the file is either cypress.config.ts or chat.cy.ts and handle it accordingly. Otherwise, it will start uncommenting the Ollama lines.

- The env block is removed from cypress.config.ts.
- For chat.cy.ts, the beforeEach block is reverted to the previous version.
- The code handles duplicates.

Also see CHANGELOG-workflow.md in the current folder.

TO DO: Change title back to "Open WebUI" and remove inserted localsite.js


## Modifications to apply to src/app.html

TO DO:

Change title="Open WebUI" to "Open WebUI ModelEarth"

Insert:

	<script type="text/javascript" src="https://model.earth/localsite/js/localsite.js?showheader=true&showsearch=true"></script>




