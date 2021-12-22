# Working with OAS
We would like to deliver an OAS specification to IT, which matches as closely as possible the actualy API, which we would like to see at the end.
For this to work we really need to:
- use OAS 3.1, which, amongst others provide the ability to add a description next to a $ref
- add a number of Saxo specific custom extensions, which are not supported by standard OAS. This includes visibility and security extensions.

Today, however no tool can support all aspectes of OAS 3.1 and the extensions. This blog proposes, what I currently consider the to be the best short term alternative.

# Initially create new oas definition with stoplight
Probably the fastest way to mock up a new API is to use the Stoplight Studio editor. It only supports OAS 3.0.1, which means you will have to cut a few corners.
- DO still define all models under the models (components/schemas) section
- Don't use references $refs when defining request query parameters and request bodies.

While using Stoplight use the Spectral linter to help write good OAS.
Once you are happy with your first draft, use DragonSlayer to test it for syntax etc. Then upload it to the design section of DragonSlayer

# Setting up to refine your OAS
For refining the OAS, you will currently need a combination of:
-   Visual Studio Code with the 42Crunch plugin to edit the RAW OAS
-   Redocly's CLI tool to render your OAS as you are editing it.
-   The CLI version of Spectral to lint the OAS as you are editing it.

## Setting up VS Code
This is simple. Just install the OpenAPI (Swagger) Editor VS Code Extension from 42Crunch. You can find it here https://github.com/42Crunch/vscode-openapi.


## Setting up the Spectral Linter.
The Spectral linter can be downloaded from here: https://github.com/stoplightio/spectral.

For Spectral to run in the current directly you also need a .spectral file. This file tells Spectral which rule-set to use. In the most basic version that file simple includes information to use the standard definitions. In this example I have added a few more rules. The rules will apply to all files in the directory.

## Setting up the Redocly-CLI
The Redocly CLI can be downloaded from here: https://redoc.ly/docs/cli/
Redocly also needs:
- A simple configuration file .redocly as well as
- A couple of files in a docs sub-directory which it uses when formatting the OAS documentation

## Extracting common definitions to a definitions.yaml file
Instead of constantly repeating common definitions in each OAS-Yaml file, we would like to separate these out to a common. definitions.yaml file. This should be placed in the same directory as the "real" OAS definitions.

# Start Editing
You are now setup to start editing.  
Fetch the latest files from TFS. Open a terminal. Move to the directory with all of the definitions and open VS Code:

`C:\projects\OpenAPI> Code .`

In VS code open the definition you would like to edit. (say *cashtransfers.yaml*) Change the OAS version to 3.1.0.

Return to the terminal, and start the redoc document renderer:
`C:\projects\OpenAPI> openapi preview-docs cashtransfers.yaml`
You should now be able to navigate to http://127.0.0.1:8080 and see the API specification.

In VS code open up an integrated terminal **Ctrl-Shift æ**. From there type
`PS C:\projects\OpenAPI> spectral lint cashtransfers.yaml`

You are now good to go:
- Every time you save the file, the preview-docs site will auto update to show the results of you latest changes.
- At regular interval rerun the "spectral lint cashtransfers.yaml" command to re-lint.


# Future plans
Long term we need to develop our own tooling in two ways:
- DragonSlayer should be able to do the same as the redoc documentation pre-viewer.
- DragonSlayer also needs to be able to replace the linter and also run as a CLI tool. Alternatively we need to run two "systems" and extend the ruleset for the Spectral Linter so it matches what we use in "DragonSlayer". This may be the best.
- It would be great if StopLight would switch to OAS 3.1, then we could probably stick to that and stop using the other tools.









