# Gepetto

Gepetto is a Python script which uses OpenAI's gpt-3.5-turbo and gpt-4 models to provide meaning to functions decompiled
by IDA Pro. At the moment, it can ask [gpt-3.5-turbo](https://platform.openai.com/docs/models), [gpt-4](https://platform.openai.com/docs/models) as well as a locally hosted [CodeLLama](https://github.com/facebookresearch/codellama) model to explain what a function does, and to automatically rename its variables. Here is a simple example of what results it can provide in mere seconds:

![](https://github.com/JusticeRage/Gepetto/blob/main/readme/comparison.png?raw=true)

## Setup

Simply drop this script (as well as the `gepetto/` folder) into your IDA plugins folder (`$IDAUSR/plugins`). 
By default, on Windows, this should be `%AppData%\Hex-Rays\IDA Pro\plugins` (you may need to create the folder).

You will need to add the required packages to IDA's Python installation for the script to work.
Find which interpreter IDA is using by checking the following registry key: 
`Computer\HKEY_CURRENT_USER\Software\Hex-Rays\IDA` (default on Windows: `%LOCALAPPDATA%\Programs\Python\Python39`).
Finally, with the corresponding interpreter, simply run: 

```
[/path/to/python] -m pip install -r requirements.txt
```

⚠️ You will also need to edit the configuration file (found as `gepetto/config.ini`) and add your own API key, which 
can be found on [this page](https://beta.openai.com/account/api-keys).
Please note that OpenAI API queries are not free (although not very expensive) and you will need to set up a payment 
method.

*Note for CodeLLaMa/local LLM models*: You will have to install and run a _local_ CodeLLaMa model on your own and configure the `OPENAI_BASE_API` path accordingly to point to your server.
The disadvantage of this approach is: it requires resources and is more work to get running. The clear advantage of this approach is that you won't be sending your dissassembly code to OpenAI's API. Which in some cases, is a policy requirement for many large organisations.
Learn more about how to configure a local CodeLLaMa model [here](https://github.com/oobabooga/text-generation-webui/tree/main/extensions/openai#api-documentation--examples) 

⚠️ In order to use GPT-4, you will need to get access to the API. It may be requested at 
[this address](https://openai.com/waitlist/gpt-4-api). If GPT-4 is not available for your account, the API will
return the following error message:

```
The model: `gpt-4` does not exist
```

## Usage

Once the plugin is installed properly, you should be able to invoke it from the context menu of IDA's pseudocode window,
as shown in the screenshot below:

![](https://github.com/JusticeRage/Gepetto/blob/main/readme/usage.png?raw=true)

Switch between models supported by Gepetto from the Edit > Gepetto menu:

![](https://github.com/JusticeRage/Gepetto/blob/main/readme/select_model.png?raw=true)

You can also use the following hotkeys:

- Ask the model to explain the function: `Ctrl` + `Alt` + `H`
- Request better names for the function's variables: `Ctrl` + `Alt` + `R`

Initial testing shows that asking for better names works better if you ask for an explanation of the function first – I
assume because the model then uses its own comment to make more accurate suggestions.
There is an element of randomness to the AI's replies. If for some reason the initial response you get doesn't suit you,
you can always run the command again.

## Limitations

- The plugin requires access to the HexRays decompiler to function.
- gpt-3.5-turbo and gpt-4 are general-purpose language models and may very well get things wrong! Always be critical of 
results returned!
- in some cases currently the CodeLLaMa model can't properly output JSON yet. This confuses Gepetto. WIP.

## Translations

You can change Gepetto's language by editing the locale in the configuration. For instance, to use the plugin
in French, you would simply add:

```ini
[Gepetto]
LANGUAGE = "fr_FR"
```
The chosen locale must match the folder names in `gepetto/locales`. If the desired language isn't available,
you can contribute to the project by adding it yourself! The translation portal to get involved is on 
[Transifex](https://app.transifex.com/gepetto/).

## Acknowledgements

- [OpenAI](https://openai.com), for making this incredible chatbot, obviously
- [Hex Rays](https://hex-rays.com/), the makers of IDA for their lightning fast support
- [Kaspersky](https://kaspersky.com), for funding all my research
- [Aaron Kaplan](https://lo-res.org/~aaron/) or [here](https://github.com/aaronkaplan/) for local CodeLLama-2 support.
